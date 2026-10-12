**AgriVisit**

**Working Demonstration of the Tool Layer**

  -------------------------------------------------------------------------------------
  **Group /         AgriVisit_Capstone --- AgriVisit
  project**         
  ----------------- -------------------------------------------------------------------
  **Week**          Week 4 --- Tool Use and Function Calling (Brief §7)

  **Owner**         Jovan Bwire (DevOps / Documentation Lead)

  **Deliverable**   Working demonstration of at least two tools

  **Repository**    https://github.com/arthursuuna/AgriVisit_Capstone

  **ClickUp board** https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1
  -------------------------------------------------------------------------------------

Three tools are demonstrated, exceeding the two the brief requires, each
exercised both by a person through the manual picker and by the model
through the Ask panel. Every call takes the same path through the gate
regardless of who initiated it.

## **1. What was demonstrated**

  -------------------------------------------------------------------------------------
  **Tool**                  **Access**   **Demonstrated   **Outcome observed**
                                         by**             
  ------------------------- ------------ ---------------- -----------------------------
  **farm_profile_lookup**   read         person and model Executes immediately. No
                                                          approval, because reading
                                                          changes nothing.

  **schedule_visit**        write        person and model Held for approval, then
                                                          created V-0006 on approval.
                                                          Also rejected once, with
                                                          nothing written.

  **log_followup**          write        person           Held for approval, then
                                                          recorded F-0001 against
                                                          V-0001.
  -------------------------------------------------------------------------------------

## **2. The sequence**

An officer prepares to follow up a reported problem, books the visit,
and records the outcome afterwards.

  ---------------------------------------------------------------------------
          **Step**             **What happens**
  ------- -------------------- ----------------------------------------------
  **1**   Look up the farm     Returns district, crops with varieties, last
                               visit, outstanding issues and previous advice.
                               Only the declared fields; the record\'s other
                               fields are projected away.

  **2**   Ask to book a visit  The model turns \"25 November\" into
                               2026-11-25 unaided, proposes schedule_visit,
                               and the call is held.

  **3**   Read the preview     Not raw arguments but the farm named and
                               located, the date as a weekday with days
                               remaining, existing visits, and whether the
                               action is reversible.

  **4**   Reject once          Recorded with the reason. Nothing written.

  **5**   Approve              Visit created, stamped with the approval
                               reference and the deciding officer.

  **6**   Log the follow-up    Held, approved, recorded against the visit.
  ---------------------------------------------------------------------------

## **3. Evidence**

Captured from the running console. Each figure is described by what it
actually shows.

![](media/image1.png){width="5.833333333333333in"
height="3.4791666666666665in"}

*Figure 1 --- Validation failure. A malformed farm ID entered by hand
through the manual picker. The error names the parameter, the value
supplied and the pattern it failed. Nothing was executed.*

![](media/image2.png){width="5.833333333333333in"
height="4.135416666666667in"}

*Figure 2 --- The approval panel. The officer sees the farm named and
located with its crops, the date spelled as a weekday with days
remaining, the count of existing visits, and whether the action is
reversible. The raw arguments are present but collapsed. The footer
states when the approval expires and which officer the decision will be
recorded against.*

![](media/image3.png){width="5.833333333333333in"
height="2.7395833333333335in"}

*Figure 3 --- A rejection. The call was discarded, nothing was written,
and the decision was recorded as A-0005 together with the reason the
officer gave.*

![](media/image4.png){width="5.833333333333333in"
height="3.8958333333333335in"}

*Figure 4 --- A model-proposed call. The officer asked in plain English;
the assistant selected schedule_visit and converted \"3 December\" into
2026-12-03 unaided. The header reads APPROVAL_REQUIRED --- Proposed by
the assistant.*

![](media/image5.png){width="5.833333333333333in" height="2.90625in"}

*Figure 5 --- A provider outage handled rather than fatal. An HTTP 503
from Gemini is reported as a plain panel with its status. Before the
timeout alignment, the same condition arrived as an unhandled fatal
error that bypassed the tool layer entirely.*

![](media/image6.png){width="5.833333333333333in" height="1.78125in"}

*Figure 6 --- The approval audit. Five decisions: three approved, one
rejected, one expired. The Origin column separates calls proposed by the
assistant from one entered manually, and each row carries the summary
the officer was shown at the time.*

+-----------------------------------------------------------------------+
| **What the audit column shows**                                       |
|                                                                       |
| Origin distinguishes an action the assistant proposed from one a      |
| person entered. Both took the same path through the gate and both     |
| required the same approval, but a supervisor can tell them apart      |
| afterwards --- which is the Week 1 commitment that AI-driven and      |
| officer-authored actions remain distinguishable.                      |
+=======================================================================+
+-----------------------------------------------------------------------+

## **4. What the demonstration shows**

-   **The model chooses the tool.** Given loose English, it selects
    among three declared tools and fills their arguments, including
    converting a date phrase into the required format.

-   **The gate decides, not the model.** A proposed call goes through
    the same seven checks as one typed by a person. There is no separate
    path and no shortcut.

-   **A write is held, not performed.** Nothing reaches the database
    until the officer approves the specific call they were shown.

-   **Saying no is recorded.** A rejection writes an approval record
    with its reason, which is arguably better evidence than an approval:
    it shows the veto being exercised.
