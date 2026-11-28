# GAN

*Project:* Campus Laundry Problems / KIU Laundry

*Investigating:* KIU dormitory residents experience difficulties using shared washing machines and dryers due to limited equipment availability, unattended laundry, and machine performance problems. These issues can lead to waiting, repeated trips to the laundry room, and disruptions to students' schedules.

*Team:* Avtandili Gogoladze (@Avto30) · Nita Javelidze (@NitaJ5) · Gigi Peikrishvili (@GigiP909)

## Project Status

| Deliverable | Status |
|---|---|
| Discovery | Initial research and synthesis completed; six real interview logs documented |
| Prototype | [KIU Laundry clickable prototype](https://kiulaundryproblemreportingprototype.lovable.app) — Built; formal usability testing pending |
| Live product | [KIU Laundry on Vercel](https://laundrykiu.vercel.app) — Developed, deployed and tested on mobile data |
| Application source code | [NitaJ5/Laundry](https://github.com/NitaJ5/Laundry) — Separate application repository |
| North Star metric | Implemented and observed in production; post-fix reconciliation passed with three Supabase reports and three matching PostHog events |
| Analytics dashboard | [Public North Star Dashboard](https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg) — Public access verified; full journey funnel pending S4 and S5 |
| Formal usability testing | Pending |
| 12-month model | Planned for Week 12 |
| Pitch deck | Planned for Week 13 |
| One-pager | Planned for Week 15 |

## Problem Discovery

Our team investigated the experiences of KIU dormitory residents using shared laundry facilities.

We conducted six documented real student interviews and analyzed the findings to identify recurring problems. Additional discovery interviews are required to meet the course target of ten.

The main findings include:

- Students sometimes have to wait because completed laundry remains inside washing machines.
- Students cannot reliably determine machine availability without visiting the laundry room.
- Some washing machines and dryers experience performance problems, even when they can turn on and complete a cycle.
- Limited usable equipment and uncertainty about machine availability can disrupt students' schedules.

Our original hypothesis focused on unattended completed laundry causing delays.

*Hypothesis verdict: WEAKENED.*

The interviews support unattended laundry as a source of inconvenience, but they do not establish how long completed laundry is typically left uncollected. Other factors, including limited machine capacity and equipment performance, also contribute to the problem.

### Discovery Documents

- [Ideal Customer Profile](00-foundation/icp.md)
- [Problem Hypothesis](01-discovery/problem-hypothesis.md)
- [Interview Script v1](01-discovery/interview-script-v1.md)
- [Interview Script v2](01-discovery/interview-script-v2.md)
- [Outreach Tracker](01-discovery/outreach-tracker.md)
- [Real Interview Logs](01-discovery/interview-logs/real/)
- [Affinity Map](01-discovery/synthesis/affinity-map.md)
- [Patterns Analysis](01-discovery/synthesis/patterns-analysis.md)
- [Problem Statement](01-discovery/problem-statement.md)
- [Competitive Landscape Seed](01-discovery/competitive-landscape-seed.md)

## Product Direction

Based on our discovery findings, our team selected student reporting of washing-machine and dryer problems as the first product direction.

We chose this reporting interaction because our research identified equipment problems, but we do not have reliable information about machine availability or automatic fault detection.

The central action is:

*A KIU dormitory resident submits a report about a specific washing-machine or dryer problem.*

The system collects student-reported observations rather than verified technical diagnoses.

It does not automatically determine machine availability, guarantee maintenance, or confirm whether equipment is functioning correctly.

Viewing reports submitted by other students remains a possible future experiment rather than a validated feature of our first prototype.

## North Star Metric

Our team selected the following North Star metric:

*Successfully submitted laundry equipment-problem reports per week per user.*

The current application records successful reports in Supabase and associates them with persistent participant identifiers.

The metric counts distinct successfully saved problem reports per participant within an ISO week, using the Asia/Tbilisi time zone and Monday as the beginning of the week.

This allows us to investigate reporting activity during our experiment.

The metric does not directly measure machine repairs, reduced waiting times, or improved equipment availability. More submitted reports do not necessarily mean fewer broken machines.

The North Star decision is recorded in [DECISIONS.md](DECISIONS.md) (Decision 10).

The [Analytics Event Schema](03-build/analytics/event-schema.md) defines six events, four of which have been implemented and observed in production.

The North Star event student_submitted_problem_report fires after successful database persistence.

### Analytics Reconciliation

On 28 November 2026, the team compared saved Supabase reports against production PostHog events.

| Verification window (UTC) | Supabase | PostHog | Result |
|---|---:|---:|---|
| 27 November, 20:40–21:35 | 4 | 3 | Mismatch |
| 27 November, 20:50–21:50 | 3 | 3 | Match |

The original discrepancy was consistent with the incorrect PostHog project token configuration during initial testing.

After correcting the configuration and redeploying the application, the subsequent testing window contained three saved reports and three matching PostHog submission events.

The historical missing event remains documented and was not deleted or backfilled.

- [Reconciliation evidence](https://eu.posthog.com/project/371831/insights/nCZCXBjm)
- [Detailed reconciliation documentation](03-build/analytics/event-schema.md)

### Analytics Dashboard

*Public dashboard:* [KIU Laundry — North Star Dashboard](https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg)

The dashboard contains three insights:

| Insight | Purpose |
|---|---|
| North Star — Weekly Successful Reports | Measures weekly counts of distinct successfully submitted reports |
| Journey Funnel — Report Submission | Measures progression through the reporting journey |
| Weekly Retention — Returning Reporters | Measures whether participants return to submit another report in subsequent weeks |

The journey funnel currently contains username login, reporting form opening, and successful report submission. Machine-selection and problem-selection events will be added after S4 and S5 are implemented.

The current funnel begins with an explicit username login. Returning participants with an active session may bypass that step.

Internal test accounts are excluded from student adoption analytics.

The public dashboard displays aggregated results without individual participant identifiers. The exact participant-level North Star breakdown is maintained privately.

Public dashboard access has been verified without authentication.

PostHog timezone and weekly grouping boundaries still require verification against the Asia/Tbilisi ISO-week metric definition.

## Original Clickable Prototype — Lovable

*Prototype:* [KIU Laundry Problem Reporting Prototype](https://kiulaundryproblemreportingprototype.lovable.app)

Our team created the original clickable prototype using Lovable, based on the existing prototype plan.

This version was designed to test a reporting interaction without building the full application or connecting a database.

### Prototype Screens

1. *Reporting start screen:* Students begin reporting a problem with shared laundry equipment.
2. *Problem reporting form:* Students select a building, machine type, machine identifier, and experienced problem.
3. *Submission confirmation:* The prototype displays the selected building, machine, reported problem, and submission time.

The prototype includes:

- Building selection: F, E, or G.
- Washing-machine and dryer selection.
- Simulated washing-machine identifiers W1(L)–W6(L).
- Simulated dryer identifiers D1(R)–D7(R).
- Predefined equipment-problem options.
- An option to describe a different problem.
- Validation preventing incomplete submissions.

The original clickable prototype uses simulated reports and does not save reports to the production database or send maintenance requests.

### Prototype Development and Internal Review

Nita created the original prototype using ChatGPT-assisted instructions and Lovable.

Gigi participated in discussing requirements, possible improvements, and the prototype's usability. He also checked that the published prototype could be opened on a phone.

The team refined the form to include clearer machine identifiers, building selection, and custom problem descriptions.

Internal technical checks covered navigation, form completion, validation, and the simulated confirmation screen.

These checks do not count as formal usability sessions with independent student participants.

*Prototype status:* Built and published. Formal usability testing remains pending.

### Design Documents

- [Prototype Plan](02-design/prototype-plan.md)
- [Usability Test Plan](02-design/user-testing/test-plan.md)

## Deployed Application — KIU Laundry

*Live application:* https://laundrykiu.vercel.app

*Application repository:* https://github.com/NitaJ5/Laundry

After developing the original clickable prototype, the team continued toward a functional, database-backed application.

The working application is maintained in a separate GitHub repository and deployed using Vercel.

The original Lovable prototype and the deployed application are different implementations of the same reporting concept.

### Implemented User Journey

The deployed application allows a student to:

1. Open the KIU Laundry website.
2. Register or log in using a username.
3. Select Building F, E, or G.
4. Choose a washing machine or dryer.
5. Select the relevant machine identifier.
6. Choose a predefined problem or enter a custom description.
7. Submit a problem report that is saved in Supabase.
8. View personal reporting statistics and weekly reporting history.

The application associates reports with persistent participant UUIDs.

### Technology Stack

| Layer | Implementation |
|---|---|
| Frontend | React and TypeScript |
| Application framework | TanStack Start |
| Hosting and deployment | Vercel |
| Database | Supabase PostgreSQL |
| Application operations | Server-side functions for participant sessions and reporting |
| Reporting statistics | Database-backed participant reporting history and weekly statistics |
| Analytics | PostHog — production event instrumentation, reporting funnel, weekly trends and retention |

The database-backed reporting system is implemented. Four of the six planned PostHog events have been implemented and observed in production.

### Functional Verification

The team conducted manual functional checks covering:

- Participant registration and session creation.
- Successful database report submission.
- Updating weekly and all-time reporting statistics.
- Preserving participant sessions when reopening the browser.
- Recovering reporting history using the same username.
- Blocking incomplete or invalid submissions.
- Logging out and restoring history after logging back in.
- Preventing duplicate reports from rapid repeated submissions.
- Keeping different participants' reports and statistics separate.

The nine previously recorded manual functional tests passed.

Database records and statistics were also checked independently during technical verification.

Additional production analytics verification included:

- Completing the reporting journey on a phone using mobile data.
- Observing successful submission events in PostHog.
- Matching report and participant identifiers between Supabase and PostHog.
- Verifying explicit username-login events.
- Verifying reporting-form opening events without unnecessary duplicate login events.
- Verifying that opening and reopening the statistics panel produces two events, while closing it does not produce an additional event.

The TypeScript check, production build, and all 24 existing automated tests passed during S6 development.

These results demonstrate technical functionality but do not replace formal usability testing with KIU dormitory residents.

### Analytics and Privacy

The application uses pseudonymous participant identifiers for analytics attribution.

The team configured PostHog to reduce unnecessary data collection:

- Unnecessary automatic analytics collection was disabled in the application.
- Client IP storage was disabled.
- GeoIP enrichment was paused.
- A subsequent production event was checked for populated IP and geographical properties.
- Internal test accounts are excluded from student adoption analytics.
- Individual participant identifiers are not displayed on the public dashboard.

The privacy checks apply to newly captured events and do not imply that previously collected data was removed.

### Current Limitations

- Username-only login does not securely verify a student's identity.
- Submitted problems are student reports, not independently verified equipment faults.
- Reports do not automatically create maintenance requests.
- The system does not provide real-time machine availability.
- The system does not automatically detect or diagnose machine faults.
- Machine-selection and problem-selection analytics remain pending.
- Remaining independent reviews and full journey funnel verification are required.
- PostHog timezone and weekly grouping boundaries require verification against the North Star definition.
- Successful technical reconciliation does not establish real student adoption or retention.
- Real student usability testing is still required.

The application source code is currently maintained in the separate NitaJ5/Laundry repository. The course repository's app/ directory has not yet been populated with that application, so the team must resolve the course code-location requirement.

## Usability Testing

Our prototype experiment investigates whether KIU dormitory residents can independently submit a specific laundry equipment-problem report without assistance.

The original frozen usability scenario is recorded in the [Prototype Plan](02-design/prototype-plan.md).

The team plans to test with five eligible KIU dormitory residents, including at least two participants who did not participate in discovery interviews.

Testing will record task completion, time taken, errors, assistance required, and participant feedback.

Internal technical verification and informal teammate feedback must remain separate from formal student usability results.

No formal participant sessions are claimed as completed in this README.

## Sprint 1 Development Responsibilities

The team has prepared its Sprint 1 implementation plan in [Sprint 1 Plan](03-build/sprints/sprint-1.md).

All three team members have development responsibilities:

- *Nita Javelidze (@NitaJ5):* North Star successful-submission instrumentation, username-login analytics, and reporting-form analytics (S1–S3).
- *Gigi Peikrishvili (@GigiP909):* Machine-selection and problem-selection analytics instrumentation (S4–S5).
- *Avtandili Gogoladze (@Avto30):* Deployment integration, production event verification, Supabase-to-PostHog reconciliation, dashboard configuration, and statistics-opening analytics (S6).

### Current Sprint 1 Status

| Story | Owner | Reviewer | Current status |
|---|---|---|---|
| S1 — Successful submission / North Star | NJ | AG | Implemented and independently verified in production against Supabase |
| S2 — Username login | NJ | GP | Implemented and observed in production; GP review pending |
| S3 — Reporting form opening | NJ | AG | Implemented and independently observed in production |
| S4 — Machine selection | GP | NJ | Implementation and verification pending |
| S5 — Problem selection | GP | NJ | Implementation and verification pending |
| S6 — Deployment, statistics event and reconciliation | AG | NJ | Statistics event implemented and independently tested; reconciliation passed; full journey verification and remaining gate documentation pending |

### Implementation Evidence

- [S1 — North Star instrumentation](https://github.com/NitaJ5/Laundry/commit/92888833ec5f309a6a9784df2d0fb677b2f69cec)
- [S2 — Username login analytics](https://github.com/NitaJ5/Laundry/commit/165dcfab72de658f5689ee89fea9f3c43f6f0288)
- [S3 — Reporting form analytics](https://github.com/NitaJ5/Laundry/commit/1bdc692b2921613e65a740e510c9619e709703b9)
- [S6 — Statistics-opening analytics](https://github.com/NitaJ5/Laundry/commit/04fcffe4694f3ab1ea09aa59bfed2ef51e18289d)

Each story must satisfy the Sprint 1 definition of done before being marked fully completed.

Implementation, deployment, and personal testing do not replace the required independent phone-based verification.

### Lab 6 and Lab 7 Documentation

- [MVP Boundary](03-build/mvp-boundary.md)
- [Architecture](03-build/architecture/architecture.md)
- [Sprint 1 Plan](03-build/sprints/sprint-1.md)
- [Sprint 1 GitHub Project Board](https://github.com/users/Avto30/projects/1)
- [Analytics Event Schema and Gate Reconciliation](03-build/analytics/event-schema.md)

## Lab 7 Shipping Gate

| Rung | Requirement | Current evidence |
|---|---|---|
| 1 | Production URL loads on a phone using mobile data | Verified by AG |
| 2 | Complete reporting journey works in production | Verified by AG |
| 3 | North Star event arrives in PostHog from production | Verified |
| 4 | PostHog and Supabase counts reconcile | Post-fix window matched 3 vs. 3; original mismatch documented |
| 5 | Public analytics dashboard exists and is linked in README | Dashboard created, public access verified, and link included above |

The full reporting funnel still requires S4 and S5.

Sprint 2, remaining independent reviews, and the final gate decision must also be documented.

The technical gate evidence does not establish independent student adoption or successful field testing.

## Project Decisions and Documentation

Our research findings, hypothesis verdicts, prototype direction, and product decisions are documented in [DECISIONS.md](DECISIONS.md).

The team uses interview evidence, functional verification, analytics reconciliation, and future usability findings to determine which assumptions are supported and which need further investigation.

AI-assisted work is documented in [AI Usage Log](docs/ai-usage-log.md).

## Repositories and Project Links

- *Official course deliverables:* https://github.com/Avto30/pdfse-f26-gan
- *Independent application source code:* https://github.com/NitaJ5/Laundry
- *Live product:* https://laundrykiu.vercel.app
- *Public analytics dashboard:* https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg
- *Original clickable prototype:* https://kiulaundryproblemreportingprototype.lovable.app
- *Course repository:* https://github.com/ZA-KIU-Classroom/PDFSE-F26

The team's immediate priorities are completing the remaining analytics events, independent teammate reviews, Sprint 2 planning, individual Homework 2 submissions, and outstanding course deliverables.

Formal usability testing and field-week participant research remain necessary before drawing conclusions about student adoption.
