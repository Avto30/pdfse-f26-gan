# Sprint 1

**Project:** KIU Laundry — Campus Laundry Problem Reporting  
**Team repository:** https://github.com/Avto30/pdfse-f26-gan  
**Application repository:** https://github.com/NitaJ5/Laundry  
**Production:** https://laundrykiu.vercel.app  
**Boundary:** `03-build/mvp-boundary.md`  
**Architecture:** `03-build/architecture/architecture.md`  
**Schema:** `03-build/analytics/event-schema.md`

Sprint 1 covers the Week 6 planning checkpoint through the Week 7 lab (exact date/time from the course announcement). The app already supports reporting, Supabase persistence, participant login, and personal statistics. The sprint implements and verifies analytics; it does not rebuild the existing app.

## Definition of done

A story is done when the owner implements and commits it (or retains accurate authorship in a reviewed pull request), the change is merged to the application repository's `main` branch, production is deployed, a **different teammate** independently walks the feature on a phone, and the exact event and properties are visible in PostHog Activity. The North Star story also requires the saved Supabase `report_id` and participant UUID to agree with PostHog. A committed draft or functioning form without captured events does not meet this definition.

The Week 7 shipping ladder is: (1) public URL works on mobile data; (2) full reporting journey works; (3) production North Star event is in PostHog; (4) PostHog and Supabase report counts reconcile for a declared window; (5) linked North Star dashboard exists. Complete evidence, not intention, is required.

## Real hours before the Week 7 lab

The team has agreed on a provisional Sprint 1 capacity of 16.5 person-hours: 14.5 hours for development and 2 hours for independent reviews.

| Initials | Hours free (planned) | When, specifically |
|---|---|---|
| NJ | 6.5 h (6 development + 0.5 review) | To be scheduled before the Week 7 lab |
| GP | 4 h (3.5 development + 0.5 review) | To be scheduled before the Week 7 lab |
| AG | 6 h (5 development + 1 review) | To be scheduled before the Week 7 lab |
| **Total** | **16.5 h** | Exact working days and times still to be confirmed |

These are agreed planning allocations, not hours already worked. Each teammate will record their actual work windows before the sprint begins.

### Story estimates

| Story | Owner | Estimated hours |
|---|---|---:|
| S1 — Successful submission / North Star (step 5) | NJ | 3.5 h |
| S2 — Username-login event (step 1) | NJ | 1.5 h |
| S3 — Reporting-form-open event (step 2) | NJ | 1 h |
| S4 — Machine-selection event (step 3) | GP | 1.5 h |
| S5 — Problem-selection event (step 4) | GP | 2 h |
| S6 — Deployment, optional statistics event, and reconciliation | AG | 5 h |
| **Total implementation** | | **14.5 h** |

The implementation estimates fit the team's planned development allocation. An additional 2 person-hours are reserved for independent reviews.

If work exceeds its estimate, the team will update the sprint board, revise the remaining estimates, and apply the documented slip rule.

## The board

**Project board:** [GAN — Sprint 1](https://github.com/users/Avto30/projects/1)  
**Repository:** https://github.com/Avto30/pdfse-f26-gan

The Sprint 1 board contains six GitHub Issues (S1–S6), each with a designated developer, independent reviewer, estimated hours, and acceptance criteria.

Four columns: `next`, `doing`, `review`, `done`. Maintain S1–S6 cards containing owner, distinct reviewer, acceptance criteria, estimate, commit/PR, production test and PostHog evidence. Merge reviewed work to `main` on each day changes are completed. A card moves to `done` only after the definition of done passes.

**Development and commit responsibilities:** Nita owns S1–S3; **Gigi owns, develops, and commits S4–S5**; Avto owns S6. Gigi's earlier inability to commit is no longer a constraint. Because the *application* lives in a different repository from the course repo, each contributor must still confirm they can write to `NitaJ5/Laundry`; otherwise submit a fork/branch PR retaining their actual authorship. Do not credit implementation to another teammate just because that teammate merges it.

## Stories

North Star is listed first, and deploy last, as required by the lecturer. **Start the S6 environment/PostHog setup immediately**, before waiting for all S1–S5 events, so changes can be tested in production as they merge.

### S1 · Journey step 5 — Save the problem report and capture the North Star · owner NJ · reviewer AG · 3.5 h

**As** Luka, a KIU dormitory resident, **I** submit a specific equipment problem and see saved-report confirmation **so that** the problem is documented in the campus study.

**Acceptance criteria:**
- Given a valid authenticated form, when Luka submits, then confirmation shows building, machine, selected issue and submission time; one corresponding report is saved in `public.laundry_reports`.
- After successful persistence, `student_submitted_problem_report` is captured with `participant_id`, `report_id`, `building`, `machine_type`, `machine_id`, `submitted_at`.
- The event `participant_id` matches database `user_id`; `report_id` matches database `id`.
- Invalid validation or failed database writes do **not** fire the North Star event.
- Duplicate deliveries do not inflate counts (distinct `report_id` is counted once); custom problem text, username and session tokens never appear in PostHog.
- Avto independently verifies the saved row and event on the public phone site.

**Smallest demo that proves it:** Submit one controlled production report, show the confirmation and row, then display its matching PostHog event.

**At planning:** Report save and confirmation exist; PostHog capture and reconciliation are unverified.

### S2 · Journey step 1 — Username login · owner NJ · reviewer GP · 1.5 h

**As** Luka, **I** sign in with a username **so that** later reports remain associated with my participant account.

**Acceptance criteria:**
- Given no valid session, selecting **Report a Problem** opens username sign-in; valid login opens the reporting form.
- A successful *explicit* username login fires `student_completed_username_login` with `participant_id`, `login_success: true`; invalid usernames or failed login do not.
- Restoring an already valid session is not counted as a new explicit login.
- Event properties contain no username, session cookie or session hash.
- Gigi independently tests the deployed login flow and observed event.

**Smallest demo:** Sign in from a fresh browser session, open the form and show PostHog Activity.

**At planning:** Login implemented; event unverified.

### S3 · Journey step 2 — Open report form · owner NJ · reviewer AG · 1 h

**As** Luka, **I** access the form after login or session restoration **so that** I can describe the equipment problem.

**Acceptance criteria:**
- Given valid participant authentication, the form displays building, machine type, machine ID and issue controls.
- Opening the form fires `student_opened_problem_form` with the matching `participant_id`.
- Background refetches and React rerenders do not inflate one form-open action into multiple events.
- Avto independently verifies on the production phone URL.

**Smallest demo:** Open the report form and inspect its PostHog event.

**At planning:** Form exists; event unverified.

### S4 · Journey step 3 — Select affected machine · owner GP · reviewer NJ · 1.5 h

**As** Luka, **I** choose the building, equipment type and specific machine **so that** my report identifies the affected equipment.

**Acceptance criteria:**
- Buildings F/E/G and relevant Washer/Dryer machine IDs are selectable; changing type clears incompatible earlier machine choice.
- Choosing a valid machine fires `student_selected_laundry_machine` with `participant_id`, `building`, `machine_type`, `machine_id` matching the UI; selecting an empty/invalid machine does not count as a valid choice.
- No personal identifiers or custom issue description are sent as event properties.
- Gigi authors and commits the implementation, and Nita independently reviews the diff and performs the production phone check.

**Smallest demo:** Choose **Building F → Dryer → D2(R)**; show selected machine and matching PostHog event.

**At planning:** Selection exists; Gigi's planned analytics code is not yet verified.

### S5 · Journey step 4 — Select problem type · owner GP · reviewer NJ · 2 h

**As** Luka, **I** identify the experienced symptom or describe another problem **so that** my report contains meaningful issue information.

**Acceptance criteria:**
- For a dryer, **Clothes remained wet**, **Stopped unexpectedly**, and **Other problem** can be selected.
- Selecting **Other problem** reveals the custom description; empty custom text prevents report submission.
- Selecting the problem fires `student_selected_problem_type` with `participant_id`, `machine_type`, `selection_kind` equal to `predefined` or `custom` as appropriate.
- No custom description text is sent to PostHog.
- Gigi authors and commits this event, and Nita independently reviews the code and checks the production result.

**Smallest demo:** Switch from a predefined dryer problem to the custom option, show validation, and inspect both privacy-safe PostHog events.

**At planning:** UI and validation exist; Gigi's planned event unverified.

### S6 · Deploy and verify the instrumented journey · owner AG · reviewer NJ · 5 h

**As** the team, **we** deploy and reconcile the journey **so that** completed student reports can be independently measured instead of inferred from clicks.

**Do deployment setup first**, even though this is listed as the last story.

**Acceptance criteria:**
- `https://laundrykiu.vercel.app` loads on mobile data and completes login → machine/issue selection → saved report → confirmation.
- The deployment uses the existing TanStack Start/Vercel/Supabase app and server-only environment secrets; `.env` protections are already committed to the app repository.
- All five journey events are visible **from production** in PostHog with the exact names and safe properties in the schema.
- The sixth schema event, `student_opened_reporting_statistics`, fires **only when the optional statistics panel is explicitly opened** and contains `participant_id`. This event is included in S6, not counted as an extra MVP journey step.
- For a specified time window and participant set, count distinct saved `report_id` values in PostHog and corresponding saved Supabase reports; record the actual window, numbers, exclusions, mismatches and outcome in the event-schema reconciliation section.
- Internal test participants are excluded from conclusions about independent student adoption.
- Nita independently verifies the deployed flow; Gigi may assist with the phone walk and event-property checks.
- README public-app and dashboard links are updated **when** each is available. Lecturer approval for the separate app repo or an `app/` copy in the team repo is obtained and documented; existing separation alone does not establish course approval.

**Smallest demo:** Open production on a phone, save a controlled report, show the event Activity stream and reconciled database count, and share the dashboard when actually created.

**At planning:** App deployed and reporting works; PostHog events, reconciliation, dashboard and code-location approval not verified.

## Execution order

1. **Avto** confirms production deployment and PostHog account access, and starts S6 setup.
2. **Nita** instruments North Star S1; **Avto** checks it against the database.
3. **Nita** instruments S2 and S3; **Gigi** reviews S2, **Avto** reviews S3.
4. **Gigi** writes and commits S4 and S5; **Nita** reviews and tests the two events.
5. **Avto** integrates production PostHog, adds the optional statistics-open event within S6, and reconciles the metric; **Nita** reviews the production gate.
6. All three document actual test outcomes and move board cards only when acceptance criteria pass.

## Slip rule

**Checkpoint:** 20:00 Asia/Tbilisi on the evening before the actual Week 7 lab.

If instrumentation runs late, **do not remove or weaken the completed reporting journey**. Prioritize the production `student_submitted_problem_report` event, correct attribution and database reconciliation. Defer optional statistics-panel analytics or presentation only with an explicit unfinished status and Sprint 2 owner; do not label unobserved events `Live`. Any blocked journey event remains an unresolved Week 7 requirement, not silently dropped. Do not substitute visits or form starts for the North Star. If capacity is short, revise estimates and redistribute reviews without making a story's implementer its own sole reviewer.

## Prototype track and pending user evidence

The Lovable clickable prototype exists: https://kiulaundryproblemreportingprototype.lovable.app. Its frozen task is in `02-design/user-testing/test-plan.md`. No formal external usability results have been documented in `02-design/user-testing/usability-findings.md` at planning time, so the repository supports **Track B (prototype exists, fewer than three documented independent sessions)**. This is not a claim that no one has been contacted offline. The lecturer requires at least three genuine usability sessions before Week 7; teammate functional tests cannot be relabeled as student sessions.

The existing test-plan recruitment and moderation assignments remain intact. Gigi can contribute to development **and** testing; now that he can commit, he may commit his own documentation and implementation changes.

## Sprint evidence and review

| Evidence | Source | Planning status |
|---|---|---|
| Public application | https://laundrykiu.vercel.app | Deployed; new production walk pending |
| Application source | https://github.com/NitaJ5/Laundry | Exists; Gigi development contributions planned |
| MVP boundary | `03-build/mvp-boundary.md` | Committed |
| Architecture | `03-build/architecture/architecture.md` | Committed; correct outdated `.gitignore` sentence |
| Schema | `03-build/analytics/event-schema.md` | Six planned events, owner adjustments proposed |
| Production PostHog Activity | PostHog project | Unverified |
| North Star count reconciliation | Event schema | Pending |
| Public analytics dashboard | README dashboard row | Pending |
| Usability findings | `02-design/user-testing/usability-findings.md` | Not documented |
| Hours, specific work windows, board link | This file | Team confirmation needed |

## DECISIONS.md line

The MVP boundary is Decision 11. Add Decision 12 to record the prototype track:

`Wk6 · Prototype track: B — published Lovable clickable prototype exists, but fewer than three independent usability sessions are documented; preserve the frozen Dryer D2 test and document at least three real sessions before Week 7 · evidence: 02-design/prototype-plan.md, 02-design/user-testing/test-plan.md, 03-build/sprints/sprint-1.md · owner: AG/NJ/GP`

**Planning integrity:** Assignments are agreed drafting intentions based on current team information, not claims that any unfinished event has been built. Fill actual hours and outcomes only from the team's real work.
