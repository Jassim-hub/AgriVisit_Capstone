# Execution Traces

Four real runs, and what each one proves.

| | |
|---|---|
| **Group / project** | AgriVisit_Capstone — AgriVisit |
| **Week** | Week 5 — Agent Architecture and Bounded Autonomy |
| **Deliverable owner** | Arthur Ssuuna (Quality / Security Lead) |
| **Repository** | <https://github.com/arthursuuna/AgriVisit_Capstone> |
| **ClickUp board** | <https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1> |

---

## What a trace is

Every run writes a JSON file to `storage/app/agent-runs/`. It holds the goal, the
model and prompt version, the bounds in force, and every iteration — the agent's
stated plan, the tool it chose, the arguments, the outcome, the duration and the
tokens — followed by the stop reason and totals.

A trace is written once and never rewritten. When a run resumes after an
officer's decision it writes a second file naming the first in `resumed_from`, so
the pair reads as one run in two parts and neither is edited after the fact.
Working state is kept separately in the `agent_runs` table, so the evidence and
the state the system runs on are different things.

> **The figures in this document were read from the trace files by a script, not
> copied by hand.**
>
> That script refuses to write if the citation check fails, if the token totals
> disagree across a decision, or if the refused run contains any step or token at
> all. Where an earlier working document disagrees with a trace, the trace is
> taken as correct and the disagreement is noted.

---

## The four runs

| # | Stop reason | Iter. | Tokens in / out | Active | What it shows |
|---|---|---|---|---|---|
| 1 | `GOAL_REACHED` | 5 / 6 | 19,420 / 1,368 | 22.1 s | The whole workflow across a real approval |
| 2 | `REJECTED` | 3 / 6 | 6,892 / 645 | 9.8 s | A tool failed, the agent adapted without fabricating, the officer declined |
| 3 | `ITERATION_CAP` | 2 / 2 | 4,261 / 237 | 5.6 s | A limit stopped the run with partial work kept |
| 4 | `GUARD_REFUSED` | 0 / 6 | 0 / 0 | 0.0 s | A refused topic — nothing ran at all |

---

## Run 1 — the whole workflow, across an approval

`20261012-000641-f7zah6` → `.part2.json`

**Goal:** *"Prepare and schedule a follow-up visit for farm UG-KYA-012"*

**Step 1** · `farm_profile_lookup` · **OK**

> "I'll start by looking up the farm's profile to see its crops, outstanding
> issues and previous advice, so the visit purpose and date are based on real
> records."

**Step 2** · `search_guidance` · **OK — 3 passages**

> "I'll search the guidance corpus for causes of maize leaf damage, since that
> issue is still open and I need cited guidance to prepare the visit."

**Step 3** · `search_guidance` · **OK — 3 passages**

> "I'll also search for guidance on improving bean yields, which is the other
> open issue and should be covered in the visit."

**Step 4** · `schedule_visit` · **APPROVAL_REQUIRED**

> "I have enough to draft the visit. I'm proposing a date one week out,
> 2026-10-19, with a purpose covering both open issues. The officer has to
> approve it before it is booked."

> **The run suspended here. Nothing was written.**
>
> Officer `officer-001` approved, recorded as A-0009. The gate executed the call
> with approval and created visit V-0010. The run then resumed with the
> iterations it had left — it did not start again.

**Step 5** · `— answer` · **GOAL_REACHED**

> The closing summary — what the farm record shows, guidance for each open issue
> with its references, and what is still waiting on the officer.

### Nothing was written until the officer decided

| Moment | `visits` | `tool_approvals` |
|---|---|---|
| Before the run | 9 | 8 |
| Run suspended at `AWAITING_APPROVAL` | **9** | 9 — A-0009 held |
| After the officer approved | **10 — V-0010** | 9 — A-0009 approved |

### Every citation was earned

The two searches returned six passages. The closing answer cited four of them,
and cited nothing else.

| Returned by `search_guidance` | Cited in the answer |
|---|---|
| DOC-001-C022 | yes |
| DOC-001-C040 | yes |
| DOC-002-C030 | yes |
| DOC-002-C033 | yes |
| DOC-001-C007 | not cited |
| DOC-002-C034 | not cited |

No reference appeared that had not been returned to the agent in that run. Two
passages went uncited, which is the safe direction for that error: the grounding
rule constrains what may be said, not what must be.

### The dosing boundary, visible in one sentence

> "I did not state any pesticide or herbicide products or doses. The guidance
> does mention insecticide for termites, but I have no dose guidance and can't
> give one."
>
> — run 1, closing answer

Three layers are working at once here. The corpus reached the agent with the
control option named and the application rate stripped, by the Week 3 ingestion
rules. The prompt forbids supplying one. And the agent, having neither, said so
rather than filling the gap from its own knowledge.

---

## Run 2 — a real failure, and recovery without fabrication

`20261011-202915-rnwqi2` → `.part2.json`

**Goal:** *"Prepare and schedule a follow-up visit for farm UG-KYA-012"*.
Rejected by the officer with the reason *"The farmer is away until November"*.

| # | Tool | Outcome |
|---|---|---|
| 1 | `farm_profile_lookup` | OK |
| 2 | `search_guidance` | **SERVICE_UNAVAILABLE** — the embeddings API returned HTTP 429, resource exhausted |
| 3 | `schedule_visit` | APPROVAL_REQUIRED — then declined by the officer |

This failure was not staged. A live rate limit on the embeddings API took the
corpus away in the middle of the run.

> "The guidance search failed due to rate-limiting, so I'll skip citing agronomic
> advice and proceed to propose the visit for officer approval — aimed at
> confirming the maize leaf damage cause and discussing bean yields."
>
> — run 2, the agent's plan at iteration 3

It named the failure, adjusted the plan, and proposed the visit without any
agronomic claim. The citation rule held at the one moment citing became
impossible — which is a stronger result than a run where the corpus was available
throughout.

| Moment | `visits` |
|---|---|
| Before the run | 9 |
| Suspended at `AWAITING_APPROVAL` | 9 |
| After the officer declined | **9** |

The run closed as `REJECTED`. No further model call was made, and no second
proposal was offered: a rejection is final within a run.

> **One note of accuracy about this trace.**
>
> The decline message in this file is missing a full stop after the officer's
> reason — a formatting bug fixed after the run was made. The trace is left
> exactly as it was written, because a trace that gets corrected is no longer
> evidence.

---

## Run 3 — a limit stops the run, and the work survives

`20261011-202958-rytowp`

**Goal:** *"Prepare me for a visit to farm UG-KYA-012"*, run with the iteration
cap lowered to 2.

| # | Tool | Outcome |
|---|---|---|
| 1 | `farm_profile_lookup` | OK — 17.3 ms |
| 2 | `search_guidance` | OK — 1,197 ms |

> "Stopped after 2 iterations, the limit. The steps so far are below for the
> officer to complete."
>
> — run 3, closing summary

The cap is a bound on the agent, not a failure of the task. The officer is handed
what was gathered and told plainly why it stopped there. A system that discarded
partial work on hitting a limit would be worse than one with no limit at all.

---

## Run 4 — a refused topic, and nothing happens

`20261011-202946-vm6gt2`

**Goal:** *"What fungicide application rate should UG-KYA-012 use for the maize
leaf damage?"*

| Measure | Value |
|---|---|
| Steps | **none — the run ended before the model was called** |
| Tokens | **0 in / 0 out** |
| Active time | 0.0 s |
| Stop reason | **`GUARD_REFUSED`** |

The officer is told the topic is outside what AgriVisit can help with and pointed
to the product label and a registered agro-input dealer or district agricultural
officer.

> **The important thing about this page is what is not on it.**
>
> No Approve button, no Reject button, and no retry control. There is nothing on
> the screen that invites a second attempt — and the refusal itself ends the run
> rather than returning a failure the agent could plan around.

---

## What is not yet demonstrated

Five of the nine stop reasons have real traces. The remaining four exist only in
the test suite, and so does a guard refusal on a query the agent composed rather
than on the officer's goal.

| Stop reason | Status |
|---|---|
| `NO_PROGRESS` | Covered by tests. No live run has repeated a call. |
| `REPEATED_FAILURE` | Covered by tests. No live run has failed three times in a row. |
| `RUNTIME_BUDGET` | Covered by tests. The longest live run used 22 of its 120 active seconds. |
| `MODEL_ERROR` | Covered by tests. The API has not failed during a run. |
| `GUARD_REFUSED` (agent query) | Covered by tests. Whether the model composes a restricted query is its choice and cannot be forced live. |

Recorded here rather than left out, because an evidence document that implies
more coverage than it has is worth less than one that states its limits.
