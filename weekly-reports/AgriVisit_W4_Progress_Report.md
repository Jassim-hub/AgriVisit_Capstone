**AgriVisit**

**Week 4 Progress Report**

|                     |                                                                   |
|---------------------|-------------------------------------------------------------------|
| **Group / project** | AgriVisit_Capstone — AgriVisit                                    |
| **Week**            | Week 4 — Tool Use and Function Calling (Brief §7)                 |
| **Reported by**     | Nabanoba Yunia (Project / Requirements Lead)                      |
| **Repository**      | https://github.com/arthursuuna/AgriVisit_Capstone                 |
| **ClickUp board**   | https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1 |

## 1. Work completed against weekly objectives

| **Week 4 activity (Brief §7)**                                                                                 | **Status**             | **Evidence**                                                                                          |
|----------------------------------------------------------------------------------------------------------------|------------------------|-------------------------------------------------------------------------------------------------------|
| Define at least two tools with purpose, input and output schema, authorization and failure behaviour           | Complete — three tools | Tool Catalogue, generated from the schemas the gate validates against                                 |
| Implement tool calling through the application layer                                                           | Complete               | ToolSession: guard, declarations, proposed call, gate, result returned to the model                   |
| At least one tool retrieves current data or performs a low-risk simulated side effect                          | Complete               | farm_profile_lookup reads; schedule_visit and log_followup write to visits and followups              |
| Test failure paths: missing parameters, unauthorized requests, unavailable services, unexpected tool responses | Complete               | Ten person-caused cases pass with record counts unchanged; model-caused rows in the evidence document |
| Add human approval before higher-impact actions                                                                | Complete               | Write tools are held; the officer sees a preview and decides; every decision is recorded              |

## 2. Key engineering decisions

- **One schema per tool, used three ways.** The same declaration validates arguments at runtime, generates the model's function declaration, and produces the catalogue. Keywords Gemini cannot express are folded into property descriptions rather than dropped, so the model is still told the rule.

- **The model proposes; the gate disposes.** A proposed call takes the same seven checks as one typed by a person. There is no separate path, and the model has no field, prompt or tool by which it can set approval.

- **Write tools are held, not performed.** A write returns APPROVAL_REQUIRED carrying the pending call, and the approved call replays those exact arguments. That is what makes "the officer approved this action" provable rather than assumed.

- **The officer approves a described change, not arguments.** The preview names the farm and its crops, spells the date as a weekday, counts existing visits and warns on a same-date clash. A duplicate is advised, not blocked: two visits in one day is unusual, not impossible, and judgement stays with the officer.

- **Every decision is recorded, including refusals.** Approve, reject and expire each write a record holding the summary and preview the officer actually saw, not a version regenerated later.

- **Pending calls expire and are single-use.** Ten minutes, consumed on decision. Without expiry an approval could land after the state it was based on changed; without single use a double submission would create two visits.

## 3. Challenges and current response

| **Challenge**                                                          | **Why it matters**                                                                                                                                                       | **Current response**                                                                                                                                          |
|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A standing instruction became false after approval                     | The model reported a completed booking as still awaiting approval. Each state was individually correct; only an end-to-end run exposed the contradiction                 | The approved result now carries a flag saying approval has happened, and the prompt states what that means. Unit tests could not have caught it               |
| A failure after the write discarded the record of it                   | The audit record would show a decision with no result, for a visit that existed. An audit trail that under-reports a real side effect reads as evidence nothing happened | The resumed session keeps the tool result even when the follow-up model call fails, and reports a model error rather than a failed action                     |
| A model turn carrying a function call cannot be rebuilt                | Gemini attaches a signature to the call part; a reconstructed turn is rejected, and the error does not say why                                                           | The raw turn is kept and resent verbatim. The pending call carries it so an approved call can resume without re-proposing                                     |
| A timeout mismatch turned a recoverable error into a fatal one         | The HTTP client was willing to wait longer than PHP would run, so a throttled call killed the process and bypassed the tool layer's failure handling entirely            | The client now times out well inside the PHP limit, and rate limits and outages surface as handled failures                                                   |
| Free-tier quota of twenty model requests per day, and provider outages | A full verification run exceeds it, and repeated HTTP 503s blocked live testing                                                                                          | Tests use a fake model client and need no quota. A separate project was used for live runs, and the model was switched while the preferred one was overloaded |

## 4. Roles and individual contributions

Roles rotate weekly. Each member moved one step along the cycle at the start of Week 4, so every member works across the whole system over the eight weeks rather than owning one slice of it.

| **Member**          | **Role in Week 3**        | **Role in Week 4**             | **Deliverable owned**                   |
|---------------------|---------------------------|--------------------------------|-----------------------------------------|
| **Nabanoba Yunia**  | DevOps / Documentation    | Project / Requirements Lead    | Week 4 progress report                  |
| **Jassim Kasule**   | Project / Requirements    | Application / Integration Lead | Updated architecture diagram            |
| **Arthur Ssuuna**   | Application / Integration | AI Engineering Lead            | Tool Catalogue and schemas              |
| **Nakayiza Nairah** | AI Engineering            | Quality / Security Lead        | Failure and authorization evidence      |
| **Jovan Bwire**     | Quality / Security        | DevOps / Documentation Lead    | Working demonstration of the tool layer |

| **Member**          | **Work owned this week**                                                                        | **Artefact**                                                 |
|---------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| **Nabanoba Yunia**  | Weekly coordination; scope control against the Week 1 charter; this report                      | weekly-reports/                                              |
| **Jassim Kasule**   | Approval record, pending-call store, audit page, migrations                                     | ApprovalService; PendingCallStore; tool_approvals            |
| **Arthur Ssuuna**   | Schema translation and function declarations; the tool-calling client and session layer         | SchemaTranslator; GeminiFunctionCallingClient; ToolSession   |
| **Nakayiza Nairah** | The gate, schema validator, the three tools, probe instrumentation, evidence command            | ToolInvoker; SchemaValidator; Catalogue/; agrivisit:evidence |
| **Jovan Bwire**     | Tool Console and Ask panel; approval preview and panels; repository evidence and the week-4 tag | ToolConsoleController; ApprovalPreview; views                |

## 5. Plan for Week 5 — Agent Architecture and Bounded Autonomy

| **Activity**                                                      | **Owner**       | **Deliverable**                |
|-------------------------------------------------------------------|-----------------|--------------------------------|
| Define the agent loop with explicit iteration and stop conditions | Nakayiza Nairah | Agent design note              |
| Implement multi-step tool use with a hard iteration cap           | Arthur Ssuuna   | Bounded agent loop             |
| Define escalation and hand-off when a limit is reached            | Jovan Bwire     | Hand-off behaviour             |
| Trace every iteration: plan, action, observation                  | Jassim Kasule   | Agent execution traces         |
| Test runaway, looping and non-terminating cases                   | Nakayiza Nairah | Bounded-autonomy test evidence |
| Commit traces, tag week-5                                         | Jassim Kasule   | Repository evidence            |

## 6. Note on scope and identity

The weather tool remains deferred to Week 6, as the Week 1 charter states, where it serves as the project's external integration. Officer identity is configured rather than authenticated: every decision is recorded against a configured officer, and the audit record would need real authentication before it carries weight in deployment. This is stated here rather than implied, and is carried as a Week 8 hardening item.
