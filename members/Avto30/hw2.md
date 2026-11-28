# Homework 2 · Avtandili Gogoladze (@Avto30) · GAN

*Production URL:* https://laundrykiu.vercel.app

*Analytics dashboard:* https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg

*Status:* Draft pending final Sprint 1 verification and field-week confirmation.

## 1. Ship report

| What I shipped | Story or rung | Commit link(s), from my account | Live at the production URL? | Walked or checked by |
|---|---|---|---|---|
| Implemented statistics-opening analytics | S6 | [Implementation](https://github.com/NitaJ5/Laundry/commit/04fcffe4694f3ab1ea09aa59bfed2ef51e18289d) | Production observed; phone review confirmation pending | NJ |
| Independently tested production reporting and reconciled Supabase against PostHog | S6, rungs 1–4 | [Verification documentation](https://github.com/Avto30/pdfse-f26-gan/commit/8eb5de3bb2839304d38a270f63af01382b58546a) | Yes | AG, NJ |
| Configured PostHog privacy and internal test-user filtering | S6 | [Analytics documentation](https://github.com/Avto30/pdfse-f26-gan/commit/8eb5de3bb2839304d38a270f63af01382b58546a) | Verified for subsequent production events | AG, NJ |
| Created and published the three-tile analytics dashboard | Rung 5 | [README update](https://github.com/Avto30/pdfse-f26-gan/commit/68fdbafd525e5f169d583c59c10c2b02f8be864a) | Public dashboard available; complete funnel pending | AG, NJ |
| Documented the shipping gate and remaining analytics work | Lab 7 | [DECISIONS.md update](https://github.com/Avto30/pdfse-f26-gan/commit/512b560e62caa5e61a769d23b94dbfb9c3a2ed3c) | Documentation | AG |
| Prepared the initial Sprint 2 field-week plan | Lab 7 | [Sprint 2 plan](https://github.com/Avto30/pdfse-f26-gan/commit/dc4e37f8c28d8498c8be7432bf6273e0087c2edf) | Documentation; confirmation pending | AG |

*The piece I am least sure works for a real person, and how I will know by Lab 8:*

I am least certain whether students will voluntarily submit reports when experiencing equipment problems. I will compare independent participants' reporting-form openings against their distinct successful submissions, excluding teammate activity.

*One thing I did that was not my assigned story:*

I independently reviewed Nita's S1 and S3 analytics events and checked whether they corresponded to actual production actions. This helped verify the reporting journey beyond my own statistics-event implementation. [Evidence](../../03-build/analytics/event-schema.md).

## 2. Pattern journal, Weeks 5 to 7

These entries were written retrospectively during HW2 preparation.

### Week 5 · One metric that matters

I participated in selecting successful problem reports per week per user as our North Star. I supported measuring completed actions rather than website visits or unfinished reporting attempts. Comparing these alternative metrics helped us avoid treating activity without a saved report as delivered value. This decision is recorded in Decision 10.

### Week 6 · The smallest complete journey

I focused on keeping the product centered on reporting a specific equipment problem rather than building an entire laundry-management system. Our existing clickable prototype helped us examine the reporting interaction before completing analytics instrumentation. I contributed to planning the verification responsibilities and separating essential reporting functionality from optional features. The MVP boundary and prototype-track decision are recorded in Decisions 11 and 12.

### Week 7 · Ship to learn

I tested production reporting using mobile data and compared the resulting PostHog events against Supabase records. The initial count was three analytics events versus four database records, which exposed a configuration-related discrepancy. A shorter post-fix testing window returned three matching records in both systems. I also implemented statistics-opening analytics and configured a dashboard that separates internal testing from independent student behavior. The reconciliation and dashboard provide evidence rather than relying on the application appearing to work.

## 3. My DECISIONS.md lines

8. Wk5 · Selected student reporting of washing-machine and dryer problems as our first prototype direction. Although our strongest discovery pattern concerned unattended laundry, interviews also revealed recurring equipment failures and unreliable dryers (Patterns 4 and 5). We chose to test a reporting interaction because we cannot reliably determine machine functionality or availability automatically. Our first prototype will test whether students can independently submit a specific equipment-problem report. Viewing reports, validating their accuracy, and investigating voluntary reporting behavior remain possible later experiments, not features validated by this prototype. · evidence: 01-discovery/synthesis/patterns-analysis.md; 01-discovery/problem-statement.md; 01-discovery/interview-logs/real/02-student-campus.md; 01-discovery/interview-logs/real/03-student-campus.md; 01-discovery/interview-logs/real/05-student-campus.md; 02-design/prototype-plan.md · owner: Team


The discovery patterns supported testing equipment-problem reporting.

9. Wk6 · Completed the first clickable prototype for student reporting of washing-machine and dryer problems. Before formal usability testing, the team refined the interface to include building selection, clearer simulated machine identifiers, additional machine options, and custom problem descriptions. The core prototype remains focused on independently submitting a simulated machine-problem report. Live machine availability, automatic problem verification, and real maintenance requests remain outside its scope. Internal technical checks do not count as formal usability testing. · evidence: 02-design/prototype-plan.md; published Lovable prototype · owner: Team


The prototype demonstrated the reporting interaction without establishing independent usability.

10. Wk5 · North star: Successfully submitted laundry equipment-problem reports per week per user; rejected website visits (measures presence rather than completed reporting value) and report form starts (measures attempts rather than successfully submitted reports) · evidence: README.md; 02-design/prototype-plan.md; 01-discovery/interview-logs/real/02-student-campus.md; 01-discovery/interview-logs/real/03-student-campus.md; 01-discovery/synthesis/patterns-analysis.md · owner: AG/NJ/GP


The selected metric is documented in the README and analytics schema.

11. Wk6 · MVP boundary: 5-step journey (student opens reporting and signs in to saved problem report confirmation); cut real-time machine availability, automatic fault detection, and automatic maintenance workflows · evidence: 03-build/mvp-boundary.md, 01-discovery/interview-logs/real/02-student-campus.md, 01-discovery/interview-logs/real/03-student-campus.md, 01-discovery/interview-logs/real/05-student-campus.md, 02-design/prototype-plan.md · owner: AG/NJ/GP


The MVP boundary defined the journey I subsequently tested in production.

12. Wk6 · Prototype track: B — published Lovable clickable prototype exists, but fewer than three independent usability sessions are documented; preserve the frozen Dryer D2 reporting task and complete at least three real sessions before the Week 7 lab · evidence: 02-design/prototype-plan.md, 02-design/user-testing/test-plan.md, 03-build/sprints/sprint-1.md · owner: AG/NJ/GP


The test plan distinguishes formal participant sessions from teammate checks.

13. Wk7 · Gate passed: live at https://laundrykiu.vercel.app, north star event firing from production, PostHog 3 = database 3 in the post-fix reconciliation window of 27 November 2026, 20:50–21:50 UTC. The original reconciliation returned PostHog 3 versus database 4; the missing event remains documented. AG independently completed the reporting journey on a phone using mobile data. The public analytics dashboard was created, tested without authentication, and linked in the README. · evidence: README.md; 03-build/analytics/event-schema.md; https://eu.posthog.com/project/371831/insights/nCZCXBjm · owner: AG/NJ


I performed the production walkthrough and compared the analytics events with saved database reports.

14. Wk7 · Configured privacy-conscious analytics by reducing unnecessary automatic event collection, disabling client IP storage, pausing GeoIP enrichment, and excluding internal test participants from student adoption analytics. Participant-level North Star analysis remains private, while the public dashboard displays aggregated results. Internal technical testing is not evidence of independent student adoption. · evidence: 03-build/analytics/event-schema.md; README.md; https://github.com/NitaJ5/Laundry/commit/ea9d662b6873a22c10a1c5113252e75c7ee47400 · owner: AG/NJ


I configured and checked the PostHog privacy settings and internal participant filters.

15. Wk7 · Retained the existing reporting journey while completing analytics instrumentation incrementally. NJ implemented S1–S3, and AG implemented the optional statistics-opening event within S6. Production verification confirmed that opening, closing, and reopening the statistics panel generated exactly two statistics-opening events. S4 and S5 remain assigned to GP, and the temporary three-event journey funnel will be expanded after their implementation. Remaining independent reviews, metric timezone verification, Sprint 2 planning, and field-week participant testing are not yet complete. · evidence: 03-build/sprints/sprint-1.md; 03-build/analytics/event-schema.md; https://github.com/NitaJ5/Laundry/commit/04fcffe4694f3ab1ea09aa59bfed2ef51e18289d · owner: AG/NJ


My implementation commit and Nita's independent production verification support this decision.

## 4. My person this week

*The field-week person I am responsible for:* Log-06 — KIU dormitory resident and shared laundry user, previously interviewed during discovery.

*How and when the link reaches them:* I will send the production URL through a private direct message before field testing begins, after confirming eligibility and availability.

I will explain that participation is voluntary and that the website records completed reports without guaranteeing equipment repairs.

*Evidence:* [Sprint 2 field-week plan](../../03-build/sprints/sprint-2.md).

## Remaining verification

- [ ] Finalize the five-event reporting funnel after S4 and S5.
- [ ] Confirm the remaining independent phone reviews.
- [ ] Verify timezone and weekly metric boundaries.
- [ ] Confirm field-week participant and contact date.
- [ ] Capture the final analytics dashboard screenshot.
- [ ] Finalize this homework and submit the individual HW2 form.

