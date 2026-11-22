
# GAN

**Project:** Campus Laundry Problems / KIU Laundry

**Investigating:** KIU dormitory residents experience difficulties using shared washing machines and dryers due to limited equipment availability, unattended laundry, and machine performance problems. These issues can lead to waiting, repeated trips to the laundry room, and disruptions to students' schedules.

**Team:** Avtandili Gogoladze (@Avto30) · Nita Javelidze (@NitaJ5) · Gigi Peikrishvili (@GigiP909)

## Project Status

| Deliverable | Status |
|---|---|
| Discovery | Initial research and synthesis completed; six real interview logs documented |
| Prototype | [KIU Laundry clickable prototype](https://kiulaundryproblemreportingprototype.lovable.app) — Built; formal usability testing pending |
| Live application | [KIU Laundry on Vercel](https://laundrykiu.vercel.app) — Developed and deployed |
| Application source code | [NitaJ5/Laundry](https://github.com/NitaJ5/Laundry) — Separate application repository |
| North Star metric | Selected and documented in `DECISIONS.md` (Decision 10) and `03-build/analytics/event-schema.md`; PostHog instrumentation pending |
| Analytics dashboard (PostHog) | Not yet implemented |
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

**Hypothesis verdict: WEAKENED.**

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

**A KIU dormitory resident submits a report about a specific washing-machine or dryer problem.**

The system collects student-reported observations rather than verified technical diagnoses.

It does not automatically determine machine availability, guarantee maintenance, or confirm whether equipment is functioning correctly.

Viewing reports submitted by other students remains a possible future experiment rather than a validated feature of our first prototype.

## North Star Metric

Our team selected the following North Star metric:

**Successfully submitted laundry equipment-problem reports per week per user.**

The current application records successful reports in Supabase and associates them with persistent participant identifiers.

The metric counts successfully saved problem reports per participant within an ISO week, using the Asia/Tbilisi time zone.

This allows us to investigate reporting activity during our experiment.

The metric does not directly measure machine repairs, reduced waiting times, or improved equipment availability. More submitted reports do not necessarily mean fewer broken machines.

The North Star decision is recorded in `DECISIONS.md` (Decision 10), and the planned analytics events are documented in `03-build/analytics/event-schema.md`. The schema defines six events, including the North Star event `student_submitted_problem_report`. PostHog implementation and reconciliation with Supabase remain outstanding.

## Original Clickable Prototype — Lovable

**Prototype:** [KIU Laundry Problem Reporting Prototype](https://kiulaundryproblemreportingprototype.lovable.app)

Our team created the original clickable prototype using Lovable, based on the existing prototype plan.

This version was designed to test a reporting interaction without building the full application or connecting a database.

### Prototype Screens

1. **Reporting start screen:** Students begin reporting a problem with shared laundry equipment.
2. **Problem reporting form:** Students select a building, machine type, machine identifier, and experienced problem.
3. **Submission confirmation:** The prototype displays the selected building, machine, reported problem, and submission time.

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

**Prototype status:** Built and published. Formal usability testing remains pending.

### Design Documents

- [Prototype Plan](02-design/prototype-plan.md)

## Deployed Application — KIU Laundry

**Live application:** https://laundrykiu.vercel.app

**Application repository:** https://github.com/NitaJ5/Laundry

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

The database-backed reporting system is implemented. PostHog event instrumentation is not yet complete.

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

The nine recorded manual functional tests passed.

Database records and statistics were also checked independently during technical verification.

These results demonstrate technical functionality but do not replace formal usability testing with KIU dormitory residents.

### Current Limitations

- Username-only login does not securely verify a student's identity.
- Submitted problems are student reports, not independently verified equipment faults.
- Reports do not automatically create maintenance requests.
- The system does not provide real-time machine availability.
- The system does not automatically detect or diagnose machine faults.
- PostHog instrumentation and analytics reconciliation are still pending.
- Real student usability testing is still required.

The application source code is currently maintained in the separate `NitaJ5/Laundry` repository. The course repository's `app/` directory has not yet been populated with that application, so the team must resolve the course code-location requirement.

## Usability Testing

Our prototype experiment investigates whether KIU dormitory residents can independently submit a specific laundry equipment-problem report without assistance.

The original frozen usability scenario is recorded in [Prototype Plan](02-design/prototype-plan.md).

The team plans to test with five eligible KIU dormitory residents, including at least two participants who did not participate in discovery interviews.

Testing will record task completion, time taken, errors, assistance required, and participant feedback.

Internal technical verification and informal teammate feedback must remain separate from formal student usability results.

No formal participant sessions are claimed as completed in this README.


## Sprint 1 Development Responsibilities

The team has prepared its Sprint 1 implementation plan in [Sprint 1 Plan](03-build/sprints/sprint-1.md).

All three team members have development responsibilities:

- **Nita Javelidze (@NitaJ5):** North Star successful-submission instrumentation, username-login analytics, and reporting-form analytics (S1–S3).
- **Gigi Peikrishvili (@GigiP909):** Machine-selection and problem-selection analytics instrumentation (S4–S5). Gigi can now commit his development work using his own GitHub account.
- **Avtandili Gogoladze (@Avto30):** Deployment integration, production event verification, and Supabase-to-PostHog reconciliation (S6).

Each story has an independent reviewer. These are planned development assignments, not claims that the analytics implementation is already complete.

The team will verify the implementation through GitHub commits, deployed functionality, and PostHog event evidence before marking stories as completed.

### Lab 6 Documentation

- [MVP Boundary](03-build/mvp-boundary.md)
- [Architecture](03-build/architecture/architecture.md)
- [Sprint 1 Plan](03-build/sprints/sprint-1.md)
- [Analytics Event Schema](03-build/analytics/event-schema.md)


## Project Decisions and Documentation

Our research findings, hypothesis verdicts, prototype direction, and product decisions are documented in [DECISIONS.md](DECISIONS.md).

The team uses interview evidence, functional verification, and future usability findings to determine which assumptions are supported and which need further investigation.

AI-assisted work is documented in [AI Usage Log](docs/ai-usage-log.md).

## Repositories and Project Links

- **Official course deliverables:** https://github.com/Avto30/pdfse-f26-gan
- **Independent application source code:** https://github.com/NitaJ5/Laundry
- **Live application:** https://laundrykiu.vercel.app
- **Original clickable prototype:** https://kiulaundryproblemreportingprototype.lovable.app
- **Course repository:** https://github.com/ZA-KIU-Classroom/PDFSE-F26

The team will continue documenting the existing implementation, completing formal usability testing, preparing the Lab 5 and Lab 6 deliverables, and integrating the required analytics instrumentation.
