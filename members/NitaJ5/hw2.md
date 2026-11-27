
# Homework 2 · Nita Javelidze (@NitaJ5) · GAN

**Project:** KIU Laundry — Campus Laundry Problem Reporting

**Production URL:** https://laundrykiu.vercel.app

## 1. Ship report

| What I shipped | Story or rung | Commit link(s), from my account | Live at the production URL? | Walked or checked by |
|---|---|---|---|---|
| Installed PostHog SDK and created shared analytics integration | Foundation / Rung 3 | [Commit](https://github.com/NitaJ5/Laundry/commit/de4b5b5136de623200cb21bd586c97e84e6f8250) | Yes | NJ |
| Implemented North Star tracking after successful report persistence | S1 | [Commit](https://github.com/NitaJ5/Laundry/commit/92888833ec5f309a6a9784df2d0fb677b2f69cec) | Yes | NJ; AG review pending |
| Implemented successful username login tracking | S2 | [Commit](https://github.com/NitaJ5/Laundry/commit/165dcfab72de658f5689ee89fea9f3c43f6f0288) | Yes | NJ; GP review pending |
| Implemented reporting form opening tracking with duplicate protection | S3 | [Commit](https://github.com/NitaJ5/Laundry/commit/1bdc692b2921613e65a740e510c9619e709703b9) | Yes | NJ; AG review pending |
| Disabled unnecessary automatic analytics collection | Privacy improvement | [Commit](https://github.com/NitaJ5/Laundry/commit/ea9d662b6873a22c10a1c5113252e75c7ee47400) | Yes | NJ; further privacy verification pending |
| Fixed an accidental reference that caused a production page crash | Bug fix | [Commit](https://github.com/NitaJ5/Laundry/commit/5d369f8c9a6a22caf5f8f123cf6447adbe7beea1) | Yes | NJ |

**Verification evidence:**

- All 24 existing automated tests passed when executed using Node.js during S1 development.
- The production North Star event arrived in PostHog with all six expected properties.
- Two explicit logins generated two login events.
- Returning to the reporting form increased form-opening events from two to three without increasing login or submission counts.
- Independent teammate reviews and database reconciliation remain pending.

**The piece I am least sure works for a real person, and how I will know by Lab 8:**

I am least sure whether students will remember their usernames and independently access their previous reporting history. Before Lab 8, I will ask a real student to log back into the application and find their previous reports. I will observe whether they can complete this task without assistance.

**One thing I did that was not my assigned story:**

While testing PostHog, I noticed that it was automatically collecting performance data that was unnecessary for our experiment. Although this was outside my assigned S1–S3 stories, I updated the analytics configuration to reduce unnecessary automatic tracking and better protect student privacy.

Evidence: [Privacy improvement](https://github.com/NitaJ5/Laundry/commit/ea9d662b6873a22c10a1c5113252e75c7ee47400)

## 2. Pattern journal, Weeks 5 to 7

These entries were written retrospectively using my work and GitHub commits from the corresponding weeks.

### Week 5 · One metric that matters

I helped define our North Star as successfully submitted laundry equipment-problem reports per week per user. Instead of measuring website visits or form openings, I focused on an action that represents actual reporting value. I went beyond selecting the metric by documenting its analytics event schema. Focusing the prototype on one reporting interaction gave us a cheaper way to investigate our assumption without building a complete laundry management system.

Evidence:
- [North Star metric decision](https://github.com/Avto30/pdfse-f26-gan/commit/a40c4e08f6889218664ac1a9efb6f755e3a9d9b8)
- [North Star analytics event schema](https://github.com/Avto30/pdfse-f26-gan/commit/3af9e03478ffd7bb8d8580ff9eaf271227a3bae0)

### Week 6 · The smallest complete journey

I created our first clickable prototype using Lovable, focusing on the journey from opening the reporting interface to receiving submission confirmation. I refined the prototype by improving machine identifiers, adding building selection and allowing custom problem descriptions. I also helped document the MVP boundary and organize the Sprint 1 development plan. Using a clickable prototype with simulated machine identifiers allowed us to perform internal technical checks before building additional functionality.

Evidence:
- [Prototype creation and improvements](https://github.com/Avto30/pdfse-f26-gan/commit/86bef8fd8dbfeb8ef88ef94ffa9631b7f07f44d0)
- [MVP boundary](https://github.com/Avto30/pdfse-f26-gan/commit/27b12bc6d40b41a392a8fe973b79a0b5a2d7b40c)
- [Sprint 1 plan](https://github.com/Avto30/pdfse-f26-gan/commit/c931c12293ab1df7923ef78c15e617226fa759e7)

### Week 7 · Ship to learn

I integrated PostHog into our production application and implemented the North Star, username login and reporting-form opening events. During production testing, I identified an incorrect PostHog token configuration and corrected it so that events could reach our analytics project. I went beyond my assigned stories by reducing unnecessary automatic data collection and fixing a production page crash. I used an internal test account, browser developer tools and PostHog Activity to verify events immediately instead of waiting for the complete team dashboard. These checks confirmed that my events were arriving, but independent reviews and reconciliation are still required.

Evidence: S1–S3, privacy improvement and bug-fix commits in the ship report.

## 3. My DECISIONS.md lines

### Decision 8

> 8. Wk5 · Selected student reporting of washing-machine and dryer problems as our first prototype direction. Although our strongest discovery pattern concerned unattended laundry, interviews also revealed recurring equipment failures and unreliable dryers (Patterns 4 and 5). We chose to test a reporting interaction because we cannot reliably determine machine functionality or availability automatically. Our first prototype will test whether students can independently submit a specific equipment-problem report. Viewing reports, validating their accuracy, and investigating voluntary reporting behavior remain possible later experiments, not features validated by this prototype. · evidence: `01-discovery/synthesis/patterns-analysis.md`; `01-discovery/problem-statement.md`; `01-discovery/interview-logs/real/02-student-campus.md`; `01-discovery/interview-logs/real/03-student-campus.md`; `01-discovery/interview-logs/real/05-student-campus.md`; `02-design/prototype-plan.md` · owner: Team

Evidence: The [prototype plan](../../02-design/prototype-plan.md) connects our reporting interaction to the equipment problems identified during discovery.

### Decision 9

> 9. Wk6 · Completed the first clickable prototype for student reporting of washing-machine and dryer problems. Before formal usability testing, the team refined the interface to include building selection, clearer simulated machine identifiers, additional machine options, and custom problem descriptions. The core prototype remains focused on independently submitting a simulated machine-problem report. Live machine availability, automatic problem verification, and real maintenance requests remain outside its scope. Internal technical checks do not count as formal usability testing. · evidence: `02-design/prototype-plan.md`; published Lovable prototype · owner: Team

Evidence: The [prototype implementation and internal review](../../02-design/prototype-plan.md) documents my prototype creation using Lovable and subsequent interface improvements.

### Decision 10

> 10. Wk5 · North star: Successfully submitted laundry equipment-problem reports per week per user; rejected website visits (measures presence rather than completed reporting value) and report form starts (measures attempts rather than successfully submitted reports) · evidence: `README.md`; `02-design/prototype-plan.md`; `01-discovery/interview-logs/real/02-student-campus.md`; `01-discovery/interview-logs/real/03-student-campus.md`; `01-discovery/synthesis/patterns-analysis.md` · owner: AG/NJ/GP

Evidence: I documented the metric decision and its corresponding [North Star event schema](../../03-build/analytics/event-schema.md).

### Decision 11

> 11. Wk6 · MVP boundary: 5-step journey (student opens reporting and signs in to saved problem report confirmation); cut real-time machine availability, automatic fault detection, and automatic maintenance workflows · evidence: `03-build/mvp-boundary.md`, `01-discovery/interview-logs/real/02-student-campus.md`, `01-discovery/interview-logs/real/03-student-campus.md`, `01-discovery/interview-logs/real/05-student-campus.md`, `02-design/prototype-plan.md` · owner: AG/NJ/GP

Evidence: The [MVP boundary](../../03-build/mvp-boundary.md) documents the journey and the features we deliberately excluded from the initial application.

### Decision 12

> 12. Wk6 · Prototype track: B — published Lovable clickable prototype exists, but fewer than three independent usability sessions are documented; preserve the frozen Dryer D2 reporting task and complete at least three real sessions before the Week 7 lab · evidence: `02-design/prototype-plan.md`, `02-design/user-testing/test-plan.md`, `03-build/sprints/sprint-1.md` · owner: AG/NJ/GP

Evidence: The [Sprint 1 plan](../../03-build/sprints/sprint-1.md) and [usability test plan](../../02-design/user-testing/test-plan.md) distinguish internal technical verification from independent participant testing.

<!-- Add subsequent gate decisions after team verification. -->

## 4. My person this week

**The field-week person I am responsible for:** Pending Sprint 2 participant confirmation.

**How and when the link reaches them:** To be confirmed after the participant assignment is documented in Sprint 2.
