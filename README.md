# GAN

*Project:* Campus Laundry Problems

*Investigating:* KIU dormitory residents experience difficulties using shared washing machines and dryers due to limited equipment availability, unattended laundry, and machine performance problems. These issues can lead to waiting, repeated trips to the laundry room, and disruptions to students' schedules.

*Team:* Avtandili Gogoladze (@Avto30) · Nita Javelidze (@NitaJ5) · Gigi Peikrishvili (@GigiP909)

## Project Status

| Link | Status |
|---|---|
| Discovery | Initial research and synthesis completed |
| Prototype | [KIU Laundry clickable prototype] https://kiulaundryproblemreportingprototype.lovable.app — Built; formal usability testing pending |
| Live product | Not yet developed |
| Analytics dashboard (PostHog) | Not yet implemented |
| 12-month model | Planned for Week 12 |
| Pitch deck | Planned for Week 13 |
| One-pager | Planned for Week 15 |

## Problem Discovery

Our team investigated the experiences of KIU dormitory residents using shared laundry facilities.

We conducted six real student interviews and analyzed the findings to identify recurring problems.

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

## Design

Based on our discovery findings, our team selected a student-reported laundry problem system as our first prototype direction. The initial prototype focuses on submitting a machine-problem report. Viewing reports submitted by other students may be investigated in a later experiment.

The proposed system would allow students to report specific problems they experience with washing machines and dryers, such as clothes remaining wet after drying or detergent foam remaining after washing.

The system treats submissions as unverified student reports rather than automatically diagnosing machines or claiming that equipment is working properly.

Our first prototype experiment will investigate whether dormitory residents can independently discover and complete a simple problem-reporting action.

This approach does not solve machine shortages, automatically determine availability, or guarantee maintenance. These limitations remain outside the scope of the first prototype.

### Design Documents

- [Prototype Plan](02-design/prototype-plan.md)

### Clickable Prototype

Our team created a clickable prototype using Lovable based on the existing prototype plan.

*Prototype link:* [KIU Laundry — Problem Reporting Prototype]https://kiulaundryproblemreportingprototype.lovable.app

The prototype consists of three screens:

1. *Reporting start screen:* Students can begin reporting a problem with shared laundry equipment.
2. *Problem reporting form:* Students select a building (F, E, or G), machine type, machine identifier, and the problem experienced. Students can select a predefined problem or describe another problem in their own words.
3. *Submission confirmation:* The prototype displays the selected building, machine, reported problem, and submission time.

The prototype uses simulated machine identifiers. Washing machines are labeled W1(L)–W6(L), while dryers are labeled D1(R)–D7(R). The labels indicate the machines' positions, with washing machines on the left and dryers on the right.

The prototype does not automatically determine machine availability, verify reported equipment problems, or send real maintenance requests.

### Initial Prototype Review

Nita created the prototype using ChatGPT-assisted instructions and Lovable.

Gigi participated in discussing the prototype requirements and possible improvements. He also checked that the published prototype could be opened on a phone.

Based on informal internal feedback, the team refined the form to include clearer machine identifiers, building selection, and a custom problem-description option.

Initial technical checks confirmed that the reporting journey works, incomplete submissions are blocked, and the published prototype is accessible on a phone.

*Current status:* The clickable prototype has been built and published. Formal usability testing with student participants has not yet been conducted, so the reporting interaction remains unvalidated.

## Project Decisions

Our research findings, hypothesis verdicts, and product decisions are documented in [DECISIONS.md](DECISIONS.md).

We use evidence from student interviews and prototype testing to decide whether to continue, revise, or reject our assumptions.
