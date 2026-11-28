# Sprint 2: The Field Week

*Project:* KIU Laundry — Campus Laundry Problem Reporting

*Team:* GAN

*Production URL:* https://laundrykiu.vercel.app

*Analytics dashboard:* https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg

*Sprint period:* Lab 7 to Lab 8

*Planning status:* Initial draft, 28 November 2026. Participant availability, team working hours and predictions require confirmation before field testing begins.

## Sprint 1 review

| Story | Owner | Done by our definition? | If not: which clause is missing, and what happened |
|---|---|---|---|
| S1 — Successful submission / North Star | NJ | Yes, for the observed production journey | AG completed the reporting journey on a phone using mobile data. The report was saved and its identifiers matched the PostHog event. Additional negative-path verification remains advisable. |
| S2 — Username login | NJ | No | Implementation and production event capture confirmed. Independent GP phone review remains pending. |
| S3 — Reporting form opening | NJ | Yes | AG independently opened the reporting form on the production phone site and the event was observed in PostHog. |
| S4 — Machine selection | GP | No | Implementation, deployment and independent NJ phone verification remain pending. |
| S5 — Problem selection | GP | No | Implementation, deployment and independent NJ phone verification remain pending. |
| S6 — Deployment, statistics event and reconciliation | AG | No | Statistics event implemented and independently tested. Reconciliation passed and the dashboard is publicly accessible. Full journey instrumentation, required phone verification and resolution of the course code-location requirement remain pending. |

### Implementation evidence

- [S1 — North Star event](https://github.com/NitaJ5/Laundry/commit/92888833ec5f309a6a9784df2d0fb677b2f69cec)
- [S2 — Username login](https://github.com/NitaJ5/Laundry/commit/165dcfab72de658f5689ee89fea9f3c43f6f0288)
- [S3 — Reporting form opening](https://github.com/NitaJ5/Laundry/commit/1bdc692b2921613e65a740e510c9619e709703b9)
- [S6 — Statistics opening](https://github.com/NitaJ5/Laundry/commit/04fcffe4694f3ab1ea09aa59bfed2ef51e18289d)
- [Analytics verification and reconciliation](../analytics/event-schema.md)

*Slip rule:* Sprint 1 instrumentation was not fully completed by the intended checkpoint. The existing functional reporting journey was preserved, and the North Star event and database reconciliation were prioritized. S4, S5 and the remaining independent reviews are carried forward until completed. No unfinished journey event has been removed from the required scope.

## The gate

| Rung | Reached? | Evidence: a link or a number | If not: owner, and by when |
|---|---|---|---|
| 1 — Production URL loads on a phone | Yes | AG opened https://laundrykiu.vercel.app using LTE | — |
| 2 — Journey completes there | Yes | AG completed login, machine selection, problem selection and successful report submission | — |
| 3 — North Star event in Activity, from production | Yes | student_submitted_problem_report observed with all required properties | — |
| 4 — Counts reconcile | Yes | PostHog 3 = Supabase 3 in the post-fix window | — |
| 5 — Dashboard, linked in README | Yes | [Public dashboard](https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg) and [README](../../README.md) | — |

### Reconciliation evidence

The original reconciliation window on 27 November 2026, 20:40–21:35 UTC, returned four Supabase reports and three PostHog events.

After correcting the PostHog project token configuration, a separate post-fix window of 20:50–21:50 UTC returned three Supabase reports and three matching PostHog events.

The original discrepancy remains documented.

- [Saved reconciliation insight](https://eu.posthog.com/project/371831/insights/nCZCXBjm)
- [Event schema and reconciliation documentation](../analytics/event-schema.md)

The five technical gate rungs have supporting evidence. However, passing the technical gate does not mean all Sprint 1 stories, Lab 7 deliverables or independent usability tests have been completed.

## Real hours before Lab 8

The following allocations are proposed and must be confirmed by each teammate against their actual availability.

| Initials | Hours free (proposed) | When, specifically |
|---|---:|---|
| NJ | 3 h | Actual working window to be confirmed |
| GP | 5 h | Actual working window to be confirmed |
| AG | 4 h | Actual working window to be confirmed |
| *Total* | *12 h* | Pending team confirmation |

## Stories this week

The priority is to complete the remaining Sprint 1 instrumentation and independent reviews before adding new functionality.

### S4 — Machine-selection analytics · owner GP · reviewer NJ · 1.5 h

*As* a KIU dormitory resident, *I* select the affected machine *so that* my report identifies the correct equipment.

*Acceptance criteria:*

- Selecting a valid machine generates student_selected_laundry_machine.
- The event includes participant_id, building, machine_type and machine_id.
- Empty or invalid machine selections do not generate a valid selection event.
- NJ independently verifies the event on the production phone site.

### S5 — Problem-selection analytics · owner GP · reviewer NJ · 2 h

*As* a KIU dormitory resident, *I* select or describe the experienced problem *so that* my report explains what happened.

*Acceptance criteria:*

- Selecting a predefined problem generates student_selected_problem_type with selection_kind: predefined.
- Selecting the custom problem option generates the same event with selection_kind: custom.
- The event includes participant_id and machine_type.
- Custom problem descriptions are not sent to PostHog.
- NJ independently verifies both selection types on the production phone site.

### S2 — Complete independent login review · owner NJ · reviewer GP · 0.5 h

*Acceptance criteria:*

- GP completes an explicit username login on the production phone site.
- The corresponding student_completed_username_login event is observed in PostHog.
- The event contains the expected participant identifier and login_success: true.
- Restoring an existing session does not generate an unnecessary login event.

### S6 — Complete dashboard and production verification · owner AG · reviewer NJ · 2 h

*Acceptance criteria:*

- Complete the remaining phone-based statistics-event verification.
- Add S4 and S5 to the journey funnel after their implementation.
- Verify all five journey events in the correct order.
- Confirm the public dashboard remains accessible without authentication.
- Exclude all teammate test participants from student adoption analytics.
- Verify the project timezone and weekly grouping boundaries against the Asia/Tbilisi ISO-week metric definition.
- Update the event schema and README with verified results.
- Record the status of the course application code-location requirement.

### Proposed sprint allocation

| Work | Hours |
|---|---:|
| Remaining stories and implementation verification | 6 h |
| Additional independent teammate reviews | 1.5 h |
| Participant recruitment, field-week monitoring and documentation | 4.5 h |
| *Total* | *12 h* |

These allocations are estimates rather than hours already worked.

No new application feature is proposed before observing actual student behavior.

## Field week: who uses it

The following candidates come from our existing discovery interviews and prototype testing plan.

Each participant must be a KIU dormitory resident who uses shared laundry facilities and is not enrolled in our PDFSE course.

Eligibility, availability and invitations must be confirmed before treating any candidate as a recruited field-week participant.

| Code name and role | Log or session they come from | How the link reaches them | Sent by, and by when | Opened? |
|---|---|---|---|---|
| Log-02 — KIU dormitory resident and shared laundry user | Discovery Interview 02 | Private direct message | GP; before field testing begins | Not yet verified |
| Log-05 — KIU dormitory resident and shared laundry user | Discovery Interview 05 | Private direct message | NJ; before field testing begins | Not yet verified |
| Log-06 — KIU dormitory resident and shared laundry user | Discovery Interview 06 | Private direct message | AG; before field testing begins | Not yet verified |

Additional eligible participants may be recruited, up to five in total.

Only code names and participant roles will appear in the public repository.

### What we tell them

"We are testing a student laundry-problem reporting website for our university project: https://laundrykiu.vercel.app. If you experience a washing-machine or dryer problem, you can use it to report what happened; we count completed reports and anonymous-to-the-public usage events, and reporting does not guarantee maintenance or repair."

Participation is voluntary. Participants should not submit invented equipment problems merely to increase our metric.

### Field-week responsibilities

- GP contacts Log-02.
- NJ contacts Log-05.
- AG contacts Log-06.
- Each teammate records whether their participant received and opened the production link.
- Actual reporting behavior is monitored through PostHog and reconciled against Supabase.
- Internal teammate activity is excluded from field-week adoption analysis.

The field-week experiment is separate from the frozen prototype usability test.

## Prediction

The following is a proposed prediction. The team must confirm it and record the actual confirmation date before sending the field-week links or observing participant results.

*By Lab 8:* At least two distinct student_submitted_problem_report events from eligible independent participants, and at least three independent participants generating student_opened_problem_form.

*We are wrong if:* None of the recruited independent participants successfully submits a report by Lab 8.

Students may not encounter equipment problems during the observation period. A low report count alone does not establish that the interface is unusable.

*Mid-week look:* AG opens the dashboard midway between the start of field testing and Lab 8. The exact date is to be confirmed before field testing begins.

AG records:

- Number of independent participants who opened the reporting form.
- Number of distinct successful reports.
- Where participants stopped in the reporting funnel.
- One evidence-based action to take, or no change if the evidence is insufficient.

### Mid-week observation

Pending. No field-week results have been claimed.

The confirmed prediction must remain unchanged after participant testing begins.

## The DECISIONS.md line

The Week 7 technical gate result is recorded in Decision 13 of [DECISIONS.md](../../DECISIONS.md).

Wk7 · Gate passed: live at https://laundrykiu.vercel.app, north star event firing from production, PostHog 3 = database 3 · evidence: README links, 03-build/analytics/event-schema.md · owner: AG/NJ

Decisions 14 and 15 document analytics privacy configuration, incremental instrumentation and the remaining implementation responsibilities.

## Before Lab 8

- [x] Production URL available.
- [x] North Star event observed in production.
- [x] Post-fix database reconciliation passed.
- [x] Public analytics dashboard linked in README.
- [x] Statistics-opening event implemented.
- [ ] Complete S4 and S5.
- [ ] Complete all remaining independent teammate reviews.
- [ ] Finalize the five-event journey funnel.
- [ ] Confirm actual working hours and dates.
- [ ] Confirm field-week participants.
- [ ] Confirm and freeze the field-week prediction.
- [ ] Send production links to eligible participants.
- [ ] Complete the mid-week analytics check.
- [ ] Resolve outstanding course deliverable requirements.
- [ ] Complete and submit individual Homework 2 files.
