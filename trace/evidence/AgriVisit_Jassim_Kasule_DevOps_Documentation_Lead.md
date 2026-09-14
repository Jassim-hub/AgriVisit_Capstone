# AgriVisit

**Agricultural Extension Field-Visit Preparation Agent**

## Repository & Task Management Setup

*DevOps / Documentation Lead — Week 1 Individual Submission*

| Field | Value |
| --- | --- |
| **Member** | Jassim Kasule |
| **Role** | DevOps / Documentation Lead |
| **Group / project** | AgriVisit_Capstone |
| **Course** | BSE4104 — Emerging Trends in Software Engineering |
| **Week** | Week 1 (31 Aug – 4 Sept 2026), Brief §7 |
| **Sections owned** | §6 Repository and Task Management Setup |
| **Repository** | https://github.com/arthursuuna/AgriVisit_Capstone |
| **ClickUp board** | https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1 |

> **Scope of this submission**
> This document contains the Week 1 work owned by Jassim Kasule as DevOps / Documentation Lead. It is one of five role-scoped submissions that together form the group's complete Week 1 deliverable. Section numbering follows the consolidated group document so that cross-references remain valid.

---

# 6. Repository and Task Management Setup

The repository and ClickUp board are live; both URLs are recorded on the cover page. This section records what was set up and how Week 1 tasks are tracked.

## 6.1 GitHub Repository

The folder structure below is taken directly from Brief §5 and should be created exactly as named, since it is what the assessment expects to find. It maps onto the architecture in §5: `src/` holds the orchestrator and console, `prompts/` the model instructions, `knowledge/` the corpus metadata, and `evidence/traces/` the audit log output.

```
README.md
docs/
    requirements/
    architecture/
    weekly-reports/
    evaluation/
prompts/
knowledge/        # metadata & provenance only — no restricted data
src/
tests/
evidence/
    traces/
    screenshots/
demo/
.env.example      # never commit real secrets
```

- Create the repository (`agrivisit-capstone`) with default branch `main` and branch protection enabled on `main`.
- Add `README.md` carrying the problem statement and the minimum proposal statement from §1.8.
- Add `.gitignore`, `LICENSE` and `.env.example`. Confirm no secret or real data is tracked.
- Create the folder structure above, each with a placeholder file so empty directories are tracked.
- Commit the Week 1 deliverables: charter to `requirements/`, diagram to `architecture/`, weekly report to `weekly-reports/`.
- Add all members and the course convener as collaborators; create a Week 1 milestone and one issue per Week 1 activity.
- Tag the Week 1 submission point (for example `week-1`) so the state is fixed and citable.

## 6.2 ClickUp Board

- Create the Space 'AgriVisit Capstone' with one List per week, Week 1 through Week 8, so the eight-week arc is visible from the outset.
- Set the status workflow to To Do / In Progress / Review / Done, as named in the brief.
- Add three Space-level custom fields: GitHub Evidence (URL), Brief Reference (text), Deliverable (dropdown).
- Populate the Week 1 List with one task per activity: use-case selection; Project Charter; success criteria; user stories; AI Boundary Matrix; architecture diagram; GitHub setup; ClickUp setup; Week 1 progress report.
- Assign every task to a named member with a due date inside 31 August – 4 September 2026.
- Link each completed task to the corresponding GitHub commit, issue or tag, as Brief §5 requires task-to-evidence traceability.

## 6.3 Week 1 Task Assignment

| Task | Assignee | Status | Brief ref. |
| --- | --- | --- | --- |
| Select use case and define primary end-to-end workflow | Arthur Ssuuna | Done | §7 W1 |
| Write Project Charter | Arthur Ssuuna | Done | §7 W1 |
| Define success criteria S1–S8 | Arthur Ssuuna | Done | §7 W1 |
| Write 12 user stories with acceptance criteria | Nabanoba Yunia | Done | §7 W1 |
| Build AI Boundary Matrix | Jovan Bwire | Done | §7 W1 |
| Produce architecture / context diagram | Nakayiza Nairah | Done | §7 W1 |
| Set up ClickUp board and weekly Lists | Jassim Kasule | Done | §7 W1 |
| Set up GitHub repository and folder structure | Jassim Kasule | Done | §7 W1 |
| Compile Week 1 progress report and evidence | Arthur Ssuuna | Done | §7 W1 |

---

*AgriVisit — DevOps / Documentation Lead — Week 1*
