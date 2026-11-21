
# MVP Boundary

The smallest complete user journey that tests whether the product creates value. Complete: the person gets the value, start to finish. Smallest: nothing that is not on that path is in Sprint 1.

**Value hypothesis under test:** A KIU dormitory resident experiencing a problem with shared laundry equipment can more easily document the affected machine and specific issue through a structured digital report and receive confirmation that the problem has been recorded. This tests the value of reporting, not whether equipment is repaired or waiting times decrease.

**The one person walking this journey:** Luka, 20, the fictional KIU student described in `00-foundation/icp.md`, who lives on campus, regularly uses shared washing machines, and experiences inconvenience caused by equipment problems and limited availability.

## The journey

1. Luka opens the KIU Laundry website, selects "Report a Problem", and signs up or logs in using a username.
2. The application identifies Luka's participant account and opens the problem-reporting form.
3. Luka selects the dormitory building (F, E, or G), machine type (washer or dryer), and specific machine identifier.
4. Luka selects the problem experienced or chooses "Other problem" and enters a custom description.
5. Luka submits the report. The server validates and saves it in Supabase, and the application displays confirmation that the report was recorded.

**Why this is complete:**

After Step 5, Luka's equipment problem has been stored in the database with the relevant building, machine, problem, and submission time. He receives confirmation that his report was saved.

Before completing the journey, the problem was not recorded in the application. Afterwards, a persistent report exists and is associated with Luka's participant account.

The reporting action is complete, although this does not mean the reported problem has been independently verified, received by maintenance staff, or repaired.

**Why this is smallest:**

We considered removing the specific machine-identification and problem-selection steps.

However, without identifying the affected machine and experienced symptoms, the submission would not communicate a sufficiently specific equipment problem. Interviews 02 and 03 describe different equipment failures, including dryers stopping unexpectedly, clothes remaining wet, and washing machines failing to drain or spin correctly.

Username login is retained because the current application associates submissions with persistent participant identifiers, enabling the selected per-user North Star metric and personal reporting history.

We also considered including personal reporting statistics in the core journey. These statistics are already implemented, but viewing them is not necessary for Luka to submit a meaningful report. They therefore remain available outside the minimum reporting journey.

The journey deliberately ends at successful report persistence and confirmation. Additional features are not required to test the team's immediate reporting hypothesis.

## Cut list

The following features are excluded from the core MVP boundary. Their exclusion does not mean that the problems they address are unimportant.

| Feature | Reason (cite the log, session, or hypothesis) | Comes back when |
|---|---|---|
| Real-time washing-machine and dryer availability | Interviews 02, 03, and 05 identify uncertainty about availability as a significant problem. However, availability requires current occupancy information that our reporting system does not collect or verify. This experiment tests problem reporting, not occupancy prediction. | When reliable machine-availability information can be obtained and a separate availability hypothesis is tested. |
| Automatic working/broken machine status | Interviews 03 and 05 describe malfunctioning machines and the need to know which equipment works. Unverified student reports cannot establish a machine's current technical condition. | When verified equipment-status data or a reliable verification process becomes available. |
| Automatic fault detection and diagnosis | Interviews 02 and 03 describe different symptoms, but the interviews do not establish the technical causes. The reporting hypothesis requires recording symptoms, not automatically diagnosing equipment. | When diagnostic information can be collected reliably and a separate fault-detection hypothesis is supported. |
| Viewing other students' reports | `02-design/prototype-plan.md` explicitly excludes this feature from the initial reporting experiment. The first assumption is whether students can independently submit reports. Publicly displaying reports introduces questions about accuracy, freshness, and interpretation. | After successful reporting behavior is validated and a separate usability experiment establishes whether shared reports provide value. |
| Automatic maintenance requests and repair guarantees | Interview 03 identifies the need for repairs, and Interview 05 suggests someone responsible for equipment problems. However, the team has not established an authorized maintenance workflow or guaranteed response process. | When the university or an authorized maintenance team agrees to receive, process, and respond to reports. |
| Maintenance staff dashboard | A maintenance dashboard would support a later operational workflow, but the current experiment concerns the student reporting journey. No maintenance staff workflow has been validated. | When a responsible maintenance partner is identified and its needs are investigated. |
| Washing-machine and dryer reservations | Interviews 02, 03, and 05 describe difficulty finding available equipment, but the current value hypothesis concerns documenting equipment problems. Reservations introduce a different scheduling and equipment-allocation problem. | When the team tests a reservation-specific hypothesis and establishes a feasible way to manage reservations. |
| Laundry-cycle notifications and reminders | The original prototype plan excludes cycle reminders. The current application does not observe real machine cycles, so it cannot reliably detect completion. Reporting a machine problem does not require cycle notifications. | When reliable cycle-status information becomes available and research establishes the value of notifications. |
| AI chatbot or automated repair advice | The discovery evidence supports concrete equipment problems and waiting, not a demonstrated need for an AI assistant. A structured selection form is sufficient to test the reporting hypothesis without generating potentially unreliable technical advice. | Only if subsequent research shows that structured reporting is inadequate and participants need additional guidance. |
| Payments and subscriptions | The selected reporting action does not involve purchasing a service. Interviews and the value hypothesis do not establish a payment requirement for documenting equipment problems. | If a future validated service requires a payment model; not part of the current reporting experiment. |
| Institutionally verified student authentication | The deployed application already supports username-based participant accounts, but it does not securely verify student identity. Stronger authentication is not necessary to demonstrate the reporting flow in a controlled experiment, although the existing limitation must be disclosed. | Before expanding to a production service requiring verified identities, stronger account protection, or sensitive operational permissions. |
| Adding or replacing physical laundry equipment | Interviews 03, 04, and 05 describe limited working equipment and the need for repairs or additional capacity. These are important underlying problems, but changing the physical equipment is outside the scope of this software reporting experiment. | When the university evaluates equipment capacity, maintenance, and infrastructure improvements separately. |

**Scope clarification:**

The application already implements persistent participant sessions, username login, database-backed problem reports, confirmation screens, and personal reporting statistics.

Those existing capabilities are not being removed.

The cut list identifies features outside the minimum journey or capabilities requiring separate validation. It does not propose deleting working application functionality.

No formal external participant usability findings are claimed as evidence for these cuts.

## Step to event

Event names are taken from `03-build/analytics/event-schema.md`.

The Step 4 event must be added to that schema in the same commit as this boundary.

| Step | Event (actor_verb_object) | In the schema? |
|---|---|---|
| 1 | `student_completed_username_login` | Yes |
| 2 | `student_opened_problem_form` | Yes |
| 3 | `student_selected_laundry_machine` | Yes |
| 4 | `student_selected_problem_type` | Added with this boundary |
| 5 | `student_submitted_problem_report` | Yes |

**North star step:** Step 5.

**North star:** Successfully submitted laundry equipment-problem reports per week per user.

**North star event:** `student_submitted_problem_report`

The North Star event represents a successfully persisted problem report, not a button click or an attempted submission.

It must fire only after Supabase confirms the saved record. Repeated delivery of the same event must not count one saved report multiple times.

The event's `report_id` allows reconciliation against database records, while `participant_id` supports counting successfully submitted reports per participant per ISO week using the Asia/Tbilisi time zone.

### Instrumentation status

The existing application implements the reporting journey and saves reports in Supabase.

The five journey events in this document describe the intended analytics instrumentation. PostHog event capture has not yet been independently verified.

The existing database records and participant statistics demonstrate technical functionality, but they do not establish that students will independently choose to submit reports in ordinary use.

## The DECISIONS.md line

Wk6 · MVP boundary: 5-step journey (student opens reporting and signs in to saved problem report confirmation); cut real-time machine availability, automatic fault detection, and automatic maintenance workflows · evidence: `03-build/mvp-boundary.md`, `01-discovery/interview-logs/real/02-student-campus.md`, `01-discovery/interview-logs/real/03-student-campus.md`, `01-discovery/interview-logs/real/05-student-campus.md`, `02-design/prototype-plan.md` · owner: AG/NJ/GP
