AgriVisit
Failure and Authorization Test Evidence
Group / project	AgriVisit_Capstone — AgriVisit
Week	Week 4 — Tool Use and Function Calling (Brief §7)
Owner	Nakayiza Nairah (Quality / Security Lead)
Deliverable	Failure/authorization test evidence
Repository	https://github.com/arthursuuna/AgriVisit_Capstone
ClickUp board	https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1

Produced by php artisan agrivisit:evidence, which runs every condition through the real gate against the live database and exits non-zero if any case returns an unexpected code, changes a record count, throws, or is skipped. A skipped case is not a pass.
1. Person-caused cases — all passing
#	Condition	Expected	Returned	Record counts	How it was triggered
1	Unlisted tool call	TOOL_NOT_FOUND	TOOL_NOT_FOUND	unchanged	Invoked delete_farm
2	Invalid parameter format	INVALID_ARGUMENTS	INVALID_ARGUMENTS	unchanged	farm_profile_lookup with KYA12
3	Missing parameter	INVALID_ARGUMENTS	INVALID_ARGUMENTS	unchanged	schedule_visit with no purpose
4	Unexpected parameter	INVALID_ARGUMENTS	INVALID_ARGUMENTS	unchanged	An extra approved field in the arguments
5	Business rule, approval supplied	INVALID_ARGUMENTS	INVALID_ARGUMENTS	unchanged	Past-dated visit, submitted with approval
6	Subject does not exist	RESOURCE_NOT_FOUND	RESOURCE_NOT_FOUND	unchanged	Lookup for UG-ZZZ-999
7	Higher-impact action without approval	APPROVAL_REQUIRED	APPROVAL_REQUIRED	unchanged	Valid schedule_visit, no approval
8	Unauthorized	UNAUTHORIZED	UNAUTHORIZED	unchanged	log_followup temporarily removed from the enabled list
9	Unavailable service	SERVICE_UNAVAILABLE	SERVICE_UNAVAILABLE	unchanged	faulty_probe, whose dependency throws
10	Unexpected tool response	UNEXPECTED_RESPONSE	UNEXPECTED_RESPONSE	unchanged	malformed_probe, which breaks its own output schema
Visits, follow-ups and approval records stood at 7 / 2 / 4 before the run and were unchanged after it. Each case carries the call id of its tool-log entry, so every refusal can be traced to the record written at the time.
Case 5 is the one to read twice
A past-dated visit was submitted with approval attached and was still refused. Approval authorises an action; it does not switch off validation. A system in which ticking approve bypasses the checks would be more dangerous than one with no approve button.
2. Model-caused cases
Each condition is evidenced twice where possible: once caused by a person through the manual picker, once by the model through the Ask panel. A refusal that only ever fires for a human tester is weaker evidence than one that fires for the model it exists to constrain.
#	Condition	Outcome	How it is caused
M1	APPROVAL_REQUIRED	«to complete»	Ask: book a visit. The model proposes schedule_visit; the call is held and nothing is written.
M2	INVALID_ARGUMENTS	«to complete»	Ask naming a malformed farm id. The model passes it through and the schema rejects it.
M3	UNAUTHORIZED	Not reachable	See section 3. A disabled tool is never declared to the model, so it cannot propose one.
M4	Model unavailable	Handled	Recorded. A one-second timeout produced a handled model failure with a session-log entry, not a stack trace.
M5	TOOL_NOT_FOUND	«to complete»	Ask the model to delete a farm record. It may propose a function that does not exist, or decline. Record whichever occurs.
3. One condition the model cannot cause, and why
The plan was to disable a tool and have the model propose it, demonstrating an UNAUTHORIZED refusal. It cannot happen. The session layer declares only enabled tools, so a disabled tool is never offered and the model has nothing to propose. The gate's authorization check is reachable only from the manual path, where a caller can name any tool.
The stronger control was kept. Not offering a capability is better than offering it and refusing, and weakening the first to make the second demonstrable would trade a real protection for a better screenshot. One cell of the matrix is therefore unfilled from the model side, which is recorded rather than worked around.
4. Two failures that were being called one
"Unavailable services" is two conditions. A tool's dependency failing produces SERVICE_UNAVAILABLE from the gate: the call was authorised, executed, and the thing it needed was unreachable. The model being unavailable — a timeout, a rate limit, a provider outage — produces a handled model error, and the gate is not involved at all.
Both were observed. The second arrived unprompted: a real HTTP 503 from the provider landed after an approved write had executed. The visit existed, the tool result was OK, and the model could not describe what had happened. One request, a success at the tool layer and a failure at the model layer, reported correctly as both.
5. Test instrumentation
Two tools exist only to fail. faulty_probe throws when executed; malformed_probe returns data that breaks its own output schema. Both are disabled unless TOOLS_PROBES_ENABLED is set, are excluded from the model's declarations even when enabled, and are refused by policy when the flag is off. A test asserts the model is declared exactly the three product tools.
They are test instrumentation, not product capability. A tool whose purpose is to break must never be something the assistant can choose.
