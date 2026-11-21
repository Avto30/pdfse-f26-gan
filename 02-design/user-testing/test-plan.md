# Prototype Test Plan

**Prototype v1 link:** https://kiulaundryproblemreportingprototype.lovable.app

**Riskiest assumption under test:**

We assume that KIU dormitory residents can independently identify and submit a specific washing-machine or dryer problem through a simple digital reporting interface, without assistance.

## The frozen test (copied from `02-design/prototype-plan.md`, section 5, unchanged)

**Task, read aloud to the tester, word for word:**

"You have just finished using dryer D2. The drying cycle completed, but your clothes are still wet. Here is an early prototype of a laundry information system. Show me what you would do, if anything, using it."

**Success looks like:**

Within 90 seconds, without assistance, the tester:

1. Finds and opens the reporting interface.
2. Selects dryer D2.
3. Selects the issue "Clothes remained wet" or an equivalent option.
4. Submits the report and reaches the confirmation screen.

**Failure looks like:**

- The tester does not attempt to report the problem.
- The tester cannot find the reporting interface.
- The tester selects the wrong machine or issue.
- The tester cannot complete the report within 90 seconds.
- The tester requires guidance from the interviewer.

**Decision rule:**

We will test the prototype with five students who match our ICP.

If at least four out of five testers complete the reporting action correctly within 90 seconds without assistance, we will proceed to investigate whether students voluntarily submit reports after experiencing real equipment problems.

If two or more testers fail, we will review the reasons for failure, reconsider the reporting process, and revise the prototype before developing additional features.

This test evaluates the usability and initial appeal of reporting. It does not prove that students will regularly submit reports in real life.

**Our prediction of the first thing a tester gets wrong:**

A tester may hesitate to report the problem because they do not understand what happens after submission or whether reporting provides any benefit.

After the task, we will ask what the tester expected to happen after submitting the report. This answer will be recorded separately from the observed success or failure.

## Five testers

All five testers must match our ICP: KIU dormitory residents who use shared campus laundry facilities.

None may be enrolled in the PDFSE-F26 course. At least two must be new participants who were not interviewed during Discovery.

Real names and contact information will remain in private team communication. Only participant codes will appear in the public repository.

The following candidates, runners, loggers, and dates are proposed. Actual invitations and bookings have not yet been verified.

| # | Code name | Role (as in our ICP) | Why they fit the ICP | Session runner (asks) | Session logger (writes) | Target day | Status (messaged / booked / done) |
|---|---|---|---|---|---|---|---|
| 1 | Log-02 | KIU dormitory resident and shared laundry user | Interview 02 documents recurring use of shared washers and dryers, equipment problems, and repeated availability checks | Gigi | Nita | 23 Nov 2026 | Recontact pending |
| 2 | Log-05 | KIU dormitory resident and shared laundry user | Interview 05 documents frequent laundry use and problems with equipment condition and availability | Nita | Avto | 24 Nov 2026 | Recontact pending |
| 3 | Log-06 | KIU dormitory resident and shared laundry user | Interview 06 documents regular use of campus laundry, waiting, and repeated trips to find machines | Avto | Gigi | 25 Nov 2026 | Recontact pending |
| 4 | New-01 | Prospective KIU dormitory resident using shared laundry | New participant required; eligibility and absence from discovery interviews must be confirmed | Nita | Gigi | 26 Nov 2026 | Recruitment pending |
| 5 | New-02 | Prospective KIU dormitory resident using shared laundry | New participant required; eligibility and absence from discovery interviews must be confirmed | Avto | Nita | 27 Nov 2026 | Recruitment pending |

**Recruitment note:**

The three returning candidates were identified in the existing prototype plan. Their previous discovery participation does not confirm their availability or consent for usability testing.

The two new participant slots have not yet been filled.

Before finalizing this plan, the team must confirm the participants' eligibility and contact at least three candidates specifically about prototype usability testing. Once a message has actually been sent, update the relevant status to `messaged`. Change it to `booked` only after the session is arranged, and `done` only after the session takes place.

## Session protocol (same every time)

1. The tester opens the original clickable prototype link on their own phone, not a teammate's phone.
2. The runner reads the frozen task word for word, then says nothing and does not guide the tester.
3. The logger starts the timer on the last word of the task and records:
   - Time to first tap.
   - Time to completed report submission and confirmation.
   - Every unprompted sentence, verbatim.
   - Wrong selections, hesitation, navigation errors, and assistance requested.
4. The session ends when the tester stops or the 90-second limit is reached.
5. The runner asks one question only: "What were you expecting to happen?"
6. The logger records the answer separately from observations.
7. The logger records a PASS or FAIL against the frozen success rule before anyone discusses the session.

## Testing preparation and evidence

The original frozen scenario specifies Dryer D2 but does not specify a building.

The updated prototype requires building selection. The team will verify that the simulated Dryer D2 is accessible in the selected building before conducting sessions, without changing the frozen task wording or success threshold.

The building chosen by each participant will be recorded. A building-selection problem will be documented as an observation.

Each formal session will be documented in:

`02-design/user-testing/usability-findings.md`

The findings will include the participant code, actual session date, observed actions, timing, direct comments, assistance required, and PASS/FAIL verdict.

Internal technical checks, teammate reviews, and database verification are not counted as participant usability sessions.

## Current status

- Original clickable prototype: Built and published.
- Frozen task and decision rule: Documented.
- Five candidate testing slots: Proposed.
- Confirmed usability-test invitations: Not yet verified.
- Confirmed session bookings: Not yet verified.
- Completed formal participant sessions: Not yet verified.

The plan will be updated as actual recruitment and testing evidence becomes available.
