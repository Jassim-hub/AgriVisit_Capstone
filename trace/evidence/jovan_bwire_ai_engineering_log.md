**AgriVisit**

Agricultural Extension Field-Visit Preparation Agent

**AI Boundary Matrix & AI Engineering Log**

*AI Engineering Lead --- Week 1 Individual Submission*

  ------------------ -------------------------------------------------------------------
  **Member**         Jovan Bwire

  **Role**           AI Engineering Lead

  **Group /          AgriVisit_Capstone
  project**          

  **Course**         BSE4104 --- Emerging Trends in Software Engineering

  **Week**           Week 1 (31 Aug -- 4 Sept 2026), Brief §7

  **Sections owned** §1.4 AI Justification · §4 AI Boundary Matrix · §7 AI Use
                     Declaration and Engineering Log

  **Repository**     https://github.com/arthursuuna/AgriVisit_Capstone

  **ClickUp board**  https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1
  ------------------ -------------------------------------------------------------------

+-----------------------------------------------------------------------+
| **Scope of this submission**                                          |
|                                                                       |
| This document contains the Week 1 work owned by Jovan Bwire as AI     |
| Engineering Lead. It is one of five role-scoped submissions that      |
| together form the group\'s complete Week 1 deliverable. Section       |
| numbering follows the consolidated group document so that             |
| cross-references remain valid.                                        |
+-----------------------------------------------------------------------+

# **1.4 Why AI Is Justified Here**

The preparation task is a natural-language retrieval-and-synthesis
problem: an officer must map a free-text farm profile and a set of
reported symptoms onto the relevant sections of a heterogeneous body of
guidance, then express the result as concrete things to inspect and ask.
This is precisely the class of task a foundation model performs well,
and it is poorly served by keyword search or fixed templates because the
mapping from symptom to guidance is semantic rather than lexical.

Equally important is where AI is not justified. Every fact the system
asserts comes from a cited corpus document rather than model memory.
Every side effect --- reading a farm record, calling the weather
service, creating a visit task, writing follow-up notes --- is
deterministic code behind an explicit schema. Every restricted topic is
blocked by rule before generation, not by asking the model to behave.
And no checklist reaches the field without officer approval. The matrix
below states this allocation step by step.

# **4. AI Boundary Matrix**

For each step of the workflow this matrix states what the AI may decide
autonomously, what must remain deterministic rule-based code, and what
always requires human approval. It is the controlling document for
Sections 5 to 8 of the final engineering report and is revised only by
explicit team decision recorded in a weekly progress report.

+---+-------------+------------------+------------------+----------------+
| * |             | **AI may do      | **Must stay      | **Requires     |
| * |             | (autonomous)**   | deterministic**  | human          |
| W |             |                  |                  | approval**     |
| o |             |                  |                  |                |
| r |             |                  |                  |                |
| k |             |                  |                  |                |
| f |             |                  |                  |                |
| l |             |                  |                  |                |
| o |             |                  |                  |                |
| w |             |                  |                  |                |
| s |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| p |             |                  |                  |                |
| / |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| c |             |                  |                  |                |
| i |             |                  |                  |                |
| s |             |                  |                  |                |
| i |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+===+=============+==================+==================+================+
| * |             | May paraphrase   | Database lookup  | None ---       |
| * |             | the retrieved    | by farm ID;      | read-only      |
| F |             | record in plain  | field selection; | operation.     |
| a |             | language for the | formatting;      |                |
| r |             | officer.         | access control.  |                |
| m |             |                  |                  |                |
| p |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| f |             |                  |                  |                |
| i |             |                  |                  |                |
| l |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| e |             |                  |                  |                |
| t |             |                  |                  |                |
| r |             |                  |                  |                |
| i |             |                  |                  |                |
| e |             |                  |                  |                |
| v |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | Selects which    | Index            | None ---       |
| * |             | retrieved        | construction;    | surfaced with  |
| G |             | passages are     | embedding        | citations for  |
| u |             | relevant and     | search; ranking; | officer        |
| i |             | explains why     | citation         | verification.  |
| d |             | they apply to    | attachment;      |                |
| a |             | this farm.       | corpus boundary. |                |
| n |             |                  |                  |                |
| c |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| e |             |                  |                  |                |
| t |             |                  |                  |                |
| r |             |                  |                  |                |
| i |             |                  |                  |                |
| e |             |                  |                  |                |
| v |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| ( |             |                  |                  |                |
| R |             |                  |                  |                |
| A |             |                  |                  |                |
| G |             |                  |                  |                |
| ) |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | Drafts the       | Template         | REQUIRED ---   |
| * |             | initial items    | structure;       | officer        |
| C |             | and talking      | item-count       | reviews and    |
| h |             | points from      | limits; citation | edits before   |
| e |             | retrieved, cited | enforcement;     | any field use. |
| c |             | guidance.        | refusal of       |                |
| k |             |                  | uncited          |                |
| l |             |                  | assertions.      |                |
| i |             |                  |                  |                |
| s |             |                  |                  |                |
| t |             |                  |                  |                |
| d |             |                  |                  |                |
| r |             |                  |                  |                |
| a |             |                  |                  |                |
| f |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | May phrase the   | API call;        | None ---       |
| * |             | forecast in      | parameter        | informational  |
| W |             | plain language   | validation;      | only.          |
| e |             | and note its     | caching; timeout |                |
| a |             | relevance to the | and failure      |                |
| t |             | visit.           | handling.        |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| c |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| c |             |                  |                  |                |
| k |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | May suggest a    | Conflict         | Officer        |
| * |             | date and time    | checking; task   | confirms the   |
| V |             | based on officer | creation;        | final date     |
| i |             | input and        | persistence;     | before the     |
| s |             | forecast.        | identifier       | task is        |
| i |             |                  | assignment.      | created.       |
| t |             |                  |                  |                |
| s |             |                  |                  |                |
| c |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| d |             |                  |                  |                |
| u |             |                  |                  |                |
| l |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | May help         | Storage;         | Officer        |
| * |             | structure        | timestamping;    | confirms       |
| F |             | free-text notes  | attribution;     | content before |
| o |             | into the defined | history update;  | it is saved to |
| l |             | fields.          | retention rules. | history.       |
| l |             |                  |                  |                |
| o |             |                  |                  |                |
| w |             |                  |                  |                |
| - |             |                  |                  |                |
| u |             |                  |                  |                |
| p |             |                  |                  |                |
| l |             |                  |                  |                |
| o |             |                  |                  |                |
| g |             |                  |                  |                |
| g |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | NEVER generates  | Rule and         | Always         |
| * |             | this content     | classifier layer | escalated to a |
| P |             | under any prompt | blocks the       | licensed       |
| e |             | or phrasing.     | request before   | specialist;    |
| s |             |                  | generation and   | refusal is     |
| t |             |                  | returns an       | logged.        |
| i |             |                  | approved         |                |
| c |             |                  | redirect         |                |
| i |             |                  | message.         |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| d |             |                  |                  |                |
| o |             |                  |                  |                |
| s |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| ; |             |                  |                  |                |
| v |             |                  |                  |                |
| e |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| r |             |                  |                  |                |
| y |             |                  |                  |                |
| o |             |                  |                  |                |
| r |             |                  |                  |                |
| m |             |                  |                  |                |
| e |             |                  |                  |                |
| d |             |                  |                  |                |
| i |             |                  |                  |                |
| c |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| d |             |                  |                  |                |
| i |             |                  |                  |                |
| a |             |                  |                  |                |
| g |             |                  |                  |                |
| n |             |                  |                  |                |
| o |             |                  |                  |                |
| s |             |                  |                  |                |
| i |             |                  |                  |                |
| s |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | Decides which    | Allow-list       | None for       |
| * |             | approved tool to | enforcement;     | approved read  |
| T |             | call and with    | schema           | tools; write   |
| o |             | what parameters. | validation;      | tools follow   |
| o |             |                  | authorisation;   | their own row  |
| l |             |                  | rate limiting;   | above.         |
| i |             |                  | error handling.  |                |
| n |             |                  |                  |                |
| v |             |                  |                  |                |
| o |             |                  |                  |                |
| c |             |                  |                  |                |
| a |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | May decide to    | Iteration cap;   | Officer        |
| * |             | re-plan within   | stop conditions; | receives       |
| A |             | the configured   | timeout; forced  | hand-off with  |
| g |             | limit.           | hand-off on      | partial        |
| e |             |                  | breach.          | results when a |
| n |             |                  |                  | limit is       |
| t |             |                  |                  | reached.       |
| i |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| a |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| d |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| m |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | Composes the     | The trigger rule | None needed;   |
| * |             | refusal message  | defines what is  | every refusal  |
| E |             | from an approved | restricted; the  | is logged for  |
| s |             | template.        | model does not   | supervisor     |
| c |             |                  | decide what is   | audit.         |
| a |             |                  | restricted.      |                |
| l |             |                  |                  |                |
| a |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| d |             |                  |                  |                |
| r |             |                  |                  |                |
| e |             |                  |                  |                |
| f |             |                  |                  |                |
| u |             |                  |                  |                |
| s |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| w |             |                  |                  |                |
| o |             |                  |                  |                |
| r |             |                  |                  |                |
| d |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             | None. The model  | Automatic        | Supervisor     |
| * |             | has no write     | capture of       | reviews        |
| A |             | access to the    | prompts,         | periodically;  |
| u |             | log.             | retrieved        | records are    |
| d |             |                  | sources, tool    | immutable.     |
| i |             |                  | calls,           |                |
| t |             |                  | approvals,       |                |
| l |             |                  | latency and      |                |
| o |             |                  | outcomes.        |                |
| g |             |                  |                  |                |
| g |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+
| * |             |                  |                  |                |
| * |             |                  |                  |                |
| C |             |                  |                  |                |
| o |             |                  |                  |                |
| n |             |                  |                  |                |
| t |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| l |             |                  |                  |                |
| l |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| p |             |                  |                  |                |
| r |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| c |             |                  |                  |                |
| i |             |                  |                  |                |
| p |             |                  |                  |                |
| l |             |                  |                  |                |
| e |             |                  |                  |                |
| * |             |                  |                  |                |
| * |             |                  |                  |                |
|   |             |                  |                  |                |
| T |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| m |             |                  |                  |                |
| o |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| l |             |                  |                  |                |
| p |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| p |             |                  |                  |                |
| o |             |                  |                  |                |
| s |             |                  |                  |                |
| e |             |                  |                  |                |
| s |             |                  |                  |                |
| ; |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| m |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| i |             |                  |                  |                |
| s |             |                  |                  |                |
| t |             |                  |                  |                |
| i |             |                  |                  |                |
| c |             |                  |                  |                |
| c |             |                  |                  |                |
| o |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| d |             |                  |                  |                |
| i |             |                  |                  |                |
| s |             |                  |                  |                |
| p |             |                  |                  |                |
| o |             |                  |                  |                |
| s |             |                  |                  |                |
| e |             |                  |                  |                |
| s |             |                  |                  |                |
| ; |             |                  |                  |                |
| t |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| o |             |                  |                  |                |
| f |             |                  |                  |                |
| f |             |                  |                  |                |
| i |             |                  |                  |                |
| c |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| c |             |                  |                  |                |
| i |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| s |             |                  |                  |                |
| . |             |                  |                  |                |
| N |             |                  |                  |                |
| o |             |                  |                  |                |
| t |             |                  |                  |                |
| h |             |                  |                  |                |
| i |             |                  |                  |                |
| n |             |                  |                  |                |
| g |             |                  |                  |                |
| t |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| m |             |                  |                  |                |
| o |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| l |             |                  |                  |                |
| p |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| d |             |                  |                  |                |
| u |             |                  |                  |                |
| c |             |                  |                  |                |
| e |             |                  |                  |                |
| s |             |                  |                  |                |
| r |             |                  |                  |                |
| e |             |                  |                  |                |
| a |             |                  |                  |                |
| c |             |                  |                  |                |
| h |             |                  |                  |                |
| e |             |                  |                  |                |
| s |             |                  |                  |                |
| a |             |                  |                  |                |
| f |             |                  |                  |                |
| a |             |                  |                  |                |
| r |             |                  |                  |                |
| m |             |                  |                  |                |
| , |             |                  |                  |                |
| a |             |                  |                  |                |
| r |             |                  |                  |                |
| e |             |                  |                  |                |
| c |             |                  |                  |                |
| o |             |                  |                  |                |
| r |             |                  |                  |                |
| d |             |                  |                  |                |
| o |             |                  |                  |                |
| r |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| e |             |                  |                  |                |
| x |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| s |             |                  |                  |                |
| e |             |                  |                  |                |
| r |             |                  |                  |                |
| v |             |                  |                  |                |
| i |             |                  |                  |                |
| c |             |                  |                  |                |
| e |             |                  |                  |                |
| e |             |                  |                  |                |
| x |             |                  |                  |                |
| c |             |                  |                  |                |
| e |             |                  |                  |                |
| p |             |                  |                  |                |
| t |             |                  |                  |                |
| t |             |                  |                  |                |
| h |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| u |             |                  |                  |                |
| g |             |                  |                  |                |
| h |             |                  |                  |                |
| v |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| i |             |                  |                  |                |
| d |             |                  |                  |                |
| a |             |                  |                  |                |
| t |             |                  |                  |                |
| e |             |                  |                  |                |
| d |             |                  |                  |                |
| c |             |                  |                  |                |
| o |             |                  |                  |                |
| d |             |                  |                  |                |
| e |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| d |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| e |             |                  |                  |                |
| x |             |                  |                  |                |
| p |             |                  |                  |                |
| l |             |                  |                  |                |
| i |             |                  |                  |                |
| c |             |                  |                  |                |
| i |             |                  |                  |                |
| t |             |                  |                  |                |
| h |             |                  |                  |                |
| u |             |                  |                  |                |
| m |             |                  |                  |                |
| a |             |                  |                  |                |
| n |             |                  |                  |                |
| a |             |                  |                  |                |
| p |             |                  |                  |                |
| p |             |                  |                  |                |
| r |             |                  |                  |                |
| o |             |                  |                  |                |
| v |             |                  |                  |                |
| a |             |                  |                  |                |
| l |             |                  |                  |                |
| . |             |                  |                  |                |
+---+-------------+------------------+------------------+----------------+

# **7. AI Use Declaration and AI Engineering Log**

Required by Brief §6. AI-generated work is permitted and expected;
unexplained work is not. The team remains accountable for correctness,
security, requirements, architecture, testing, documentation and final
system behaviour.

## **7.1 Declared Tools**

  ------------------------------------------------------------------------
  **Tool / model**   **Used for**        **Ownership and verification**
  ------------------ ------------------- ---------------------------------
  **Claude           Drafting and        All content reviewed, edited and
  (Anthropic)**      structuring Week 1  accepted by the named section
                     documentation       owner; factual claims verified
                                         against the brief.

  **Coding           Not yet used --- no To be declared from Week 2
  assistant**        code written in     onward.
                     Week 1              
  ------------------------------------------------------------------------

## **7.2 AI Engineering Log --- Week 1**

A concise record of material AI-assisted decisions and generated
artefacts. This log runs for all eight weeks and its summary becomes an
appendix of the final report.

  -----------------------------------------------------------------------------
  **Entry**   **Date**    **AI-assisted artefact   **Human review and outcome**
                          or decision**            
  ----------- ----------- ------------------------ ----------------------------
  **W1-01**   31 Aug 2026 Comparative analysis of  Team reviewed the analysis
                          the nine candidate use   and independently selected
                          cases in Brief §3        the extension
                          against feasibility,     visit-preparation agent.
                          corpus availability and  Reasoning recorded in §8.3.
                          evaluation tractability. 

  **W1-02**   1 Sept 2026 Initial draft of the     Each story reviewed by the
                          Project Charter and the  Quality / Security Lead;
                          twelve user stories.     acceptance criteria
                                                   rewritten to be
                                                   independently testable and
                                                   mapped to success measures.

  **W1-03**   2 Sept 2026 Draft AI Boundary Matrix AI Engineering Lead added
                          rows.                    the iteration and
                                                   termination row and moved
                                                   refusal-trigger definition
                                                   out of the AI column into
                                                   deterministic code.

  **W1-04**   3 Sept 2026 Architecture and context Application / Integration
                          diagram.                 Lead verified every
                                                   component and data flow
                                                   against the boundary matrix
                                                   before acceptance.
  -----------------------------------------------------------------------------

*Verify these dates against what the team actually did and correct any
that differ.*

## **7.3 Standing Commitments**

-   No confidential, personal or restricted data is sent to any external
    AI service. All farm data is synthetic.

-   Generated code is reviewed, tested and refactored before it reaches
    the main branch.

-   Important prompts are versioned under prompts/ from Week 2, with
    evidence retained for meaningful changes.

-   No destructive or high-impact action is permitted without an
    explicit human approval control.

-   All AI-suggested references and technical claims are verified before
    use.

-   Every member can explain the work submitted under their name.
