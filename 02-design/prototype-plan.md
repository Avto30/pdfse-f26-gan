
# Prototype Plan

## 1. The riskiest assumption

> We assume that KIU dormitory residents can independently identify and submit a specific washing-machine or dryer problem through a simple digital reporting interface, without assistance.

**Why this one is riskiest:**

Our interviews show that malfunctioning washing machines and dryers cause inconvenience, wasted time, and frustration. Interviews 02 and 03 describe dryers stopping unexpectedly or failing to dry clothes properly. Interview 03 also describes a washing machine failing to drain and spin correctly. Interview 05 expresses a need for someone responsible for addressing equipment problems.

However, our interviews do not establish whether students would actually use a digital reporting system. If students do not contribute reports, the system will have insufficient information to support future features, such as viewing recent problems reported by others.

Our first prototype will test whether students can discover and complete the reporting action in a realistic scenario. Their willingness to report problems voluntarily in everyday life will require further testing.

## 2. The one action

> A KIU dormitory resident can submit a report about a specific washing-machine or dryer problem.

## 3. Fidelity

| Choice | Why it is the cheapest thing that can kill the assumption in Section 1 | Tool | Time we will spend |
|---|---|---|---|
| Clickable, phone-sized prototype | We can observe whether students discover and complete the reporting process without building a database, integrating with machines, or developing a full application. | Lovable | Up to 2 hours (planned) |

The prototype will use simulated machine identifiers and will not submit reports to an actual maintenance system.

## 4. Screens, each traced to evidence

| Screen | What the tester does on it | The log or problem-statement sentence that earns it |
|---|---|---|
| 1. Report a Problem — Start | Opens the prototype and selects "Report a Problem". | Interview 05 describes the need for someone students can approach about equipment problems. The reporting interface tests a possible way to communicate these issues. |
| 2. Problem Report Form | Selects a washing machine or dryer, identifies the machine, chooses the experienced problem, and submits the report. | Interviews 02 and 03 describe dryers failing to dry properly or stopping unexpectedly. Interview 03 describes a washing machine that failed to drain and spin. These experiences justify collecting specific symptoms rather than labeling equipment simply as working or broken. |
| 3. Report Confirmation | Sees a summary of the reported machine, issue, and submission time after submitting the form. | Interview 05 identifies a need to communicate equipment problems. Confirmation is a proposed design mechanism for making successful submission clear; it was not directly requested by interview participants. |

**Important limitations:**

- Reports describe student experiences, not verified technical diagnoses.
- A machine completing a cycle does not necessarily mean it washed or dried clothes properly.
- The prototype will not automatically determine whether equipment is functioning correctly.
- Submitting a report does not guarantee that maintenance staff will review or fix the problem.
- The prototype will use fictional machine identifiers for testing until the team verifies the actual machine labels.

**Will not appear, even if a tool generates it:**

- Viewing other students' reports
- Automatic working/broken machine statuses
- Live machine availability
- Automatic fault detection
- Equipment reservations
- Laundry notifications or reminders
- AI chatbots
- Payments or subscriptions
- Student accounts or login systems
- Maintenance dashboards
- Promises of automatic repair requests

Viewing other students' reports may be explored in a later version, but it is not part of this first prototype.

## 5. The test that can fail

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

## 6. Five testers

Testers must match our ICP, be KIU dormitory residents who use shared laundry facilities, and not be enrolled in this course. At least two must be new participants who were not interviewed during Discovery.

The following recruitment assignments are proposed and not yet confirmed.

| Code name | From log, or new | Who on the team books them |
|---|---|---|
| Log-02 | Interview 02; recontact pending | Gigi |
| Log-05 | Interview 05; recontact pending | Nita |
| Log-06 | Interview 06; recontact pending | Avto |
| log-01 | New participant; recruitment pending | Nita |
| log-04 | New participant; recruitment pending | Avto |

Before testing, the team will confirm participant eligibility, availability, and recruitment responsibilities. Real names and personal contact information will remain outside the public repository.

---

## Self-check before committing

- [x] Section 1 describes user behavior rather than technical functionality.
- [x] Section 5 contains observable success and failure conditions.
- [x] Every screen in Section 4 is connected to interview evidence.
- [x] The prototype focuses on one primary action: reporting a problem.
- [x] The team has agreed on the reporting concept and selected the prototyping tool.
- [x] Tester recruitment responsibilities have been confirmed.
