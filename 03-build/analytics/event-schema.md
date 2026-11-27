# Event Schema

*North star:* Successfully submitted laundry equipment-problem reports per week per user.

*North star event:* student_submitted_problem_report

*How the north star is computed from events:*

Count unique successfully submitted laundry equipment-problem reports for each participant during each ISO week, using the Asia/Tbilisi time zone and Monday as the beginning of the week.

Group successful submission events by participant_id, ISO year, and ISO week. Deduplicate by report_id so repeated event delivery does not count one saved report multiple times.

The metric measures successful reporting activity, not equipment repairs or verified machine faults.

## Events

PostHog instrumentation has been implemented for four of the six planned events. Production verification is ongoing. An event is marked Live only after the required independent phone-based verification and PostHog Activity check.

| Step | Event (actor_verb_object) | Fired by | Properties (never a name, phone, email, or message text) | Owner | Live |
|---|---|---|---|---|---|
| Acquisition / access | student_completed_username_login | Student, after successful username login | participant_id, login_success: true | Nita | Production observed; GP review pending |
| Activation (precursor) | student_opened_problem_form | Student, when the reporting form becomes available after authentication | participant_id | Nita | Yes |
| Activation (precursor) | student_selected_laundry_machine | Student, when a valid machine identifier is selected | participant_id, building, machine_type, machine_id | Gigi | No |
| Activation (precursor) | student_selected_problem_type | Student, when selecting a predefined problem or the custom problem option | participant_id, machine_type, selection_kind (predefined or custom) | Gigi | No |
| Activation (north star) ★ | student_submitted_problem_report | Student, after the server confirms that the report was successfully saved in Supabase | participant_id, report_id, building, machine_type, machine_id, submitted_at | Nita | Yes |
| Retention / engagement precursor | student_opened_reporting_statistics | Student, when explicitly opening the statistics panel from the reporting form | participant_id | Avto | Production observed; independent review completed; phone verification to be confirmed |

## Rules we are following

- Each event uses one consistent lowercase actor_verb_object name with underscores.
- Every custom event includes the application's persistent, pseudonymous participant_id.
- PostHog should use the same participant UUID consistently for event attribution.
- We do not intentionally send usernames, real names, phone numbers, email addresses, custom problem descriptions, or session tokens to PostHog.
- report_id is the unique database identifier of a saved report, not a participant identifier.
- Events containing equipment information must refer to the actual selected building, machine type, and machine identifier.
- The North Star event fires only after successful database persistence, never merely when the Submit button is clicked.
- Failed form validation and failed database submissions must not fire the North Star event.
- Multiple clicks, retries, or repeated event delivery must not increase the count for the same saved report.
- Viewing statistics is an engagement action, not proof of long-term retention.
- Existing Supabase records alone do not prove that PostHog events were captured. Production analytics must be independently verified and reconciled against database records.
- Internal test activity must not be interpreted as independent student adoption.
- Public dashboards must not expose individual participant identifiers.

## Reconcile plan

Before trusting the analytics dashboard, we compare PostHog events against the authoritative reports stored in Supabase.

*Event most likely to fail silently:* student_submitted_problem_report

*How it would fail:*

A student may successfully save a report to Supabase, but the PostHog event might not be delivered because of a network interruption, blocked analytics request, or missing instrumentation.

Alternatively, an analytics retry could deliver the same event twice, inflating the reported submission count.

*The check that would catch it:*

1. Select one completed ISO week using the Asia/Tbilisi time zone.
2. Count saved reports in public.laundry_reports, grouped by participant UUID.
3. Count distinct PostHog student_submitted_problem_report events by report_id, grouped by the same participant UUID and week.
4. Compare the PostHog result with the corresponding Supabase counts.
5. Investigate any missing, duplicated, or mismatched events.
6. Confirm that the North Star event is emitted only for a successfully persisted report.

For the initial technical shipping gate, a shorter, explicitly defined production testing window is used.

Supabase remains the authoritative source of saved reports.

*Who runs the check in Week 7:* Avto, with Nita assisting with database verification and independently reviewing S6. Gigi assists with application event verification.

*Current verification status:* Four of the six analytics events have been implemented and observed in production. The post-fix reconciliation returned three distinct saved reports and three matching PostHog submission events. The original discrepancy remains documented below. S4 and S5 implementation and remaining independent reviews are pending.

## Gate reconcile (Lab 7)

*Date:* 28 November 2026

*Production URL walked:* https://laundrykiu.vercel.app

*Walked by:* AG

*On a phone, mobile data:* Yes (LTE).

### Reconciliation results

| Source | Count | Window (UTC) | Exactly how it was counted |
|---|---:|---|---|
| PostHog | 3 | 27 November 2026, 20:50–21:50 | Trends insight, total count of student_submitted_problem_report. All three report IDs were distinct and matched Supabase records. |
| Supabase | 3 | 27 November 2026, 20:50–21:50 | Count of distinct saved report IDs using the SQL query below. |

*Database query:*

SELECT COUNT(DISTINCT id) AS saved_reports
FROM public.laundry_reports
WHERE created_at >= '2026-11-27 20:50:00+00'
  AND created_at < '2026-11-27 21:50:00+00';

*Match:* Yes, for the post-fix testing window.

The corresponding local testing window was 28 November 2026, 00:50–01:50 Asia/Tbilisi.

Internal test events were included in both sources for this technical reconciliation.

### Original discrepancy

The original reconciliation window (27 November 2026, 20:40–21:35 UTC) contained four saved Supabase reports but only three PostHog submission events.

| Source | Original count | Post-fix count |
|---|---:|---:|
| Supabase | 4 | 3 |
| PostHog | 3 | 3 |
| Difference | 1 | 0 |

The missing report was submitted at 20:44:11 UTC. The discrepancy was consistent with the earlier incorrect PostHog project token configuration.

The production environment variable was corrected in Vercel and the application was redeployed.

The subsequent reconciliation used a separately declared post-fix window and returned three saved reports and three PostHog events, with matching report IDs, participant IDs, and submission timestamps.

The original missing event was not deleted, fabricated, or backfilled. The historical discrepancy remains part of the verification record.

*Analytics implementation:* [North Star instrumentation commit](https://github.com/NitaJ5/Laundry/commit/92888833ec5f309a6a9784df2d0fb677b2f69cec).

*Reconciliation evidence:* [Saved PostHog insight](https://eu.posthog.com/project/371831/insights/nCZCXBjm).

### Silent failure predicted in Lab 5

*Event most likely to fail silently:* student_submitted_problem_report.

*Predicted failure:* A student may successfully save a report to Supabase, but the PostHog event might not be delivered because of a network interruption, blocked analytics request, or missing instrumentation.

*Did it happen?* Yes. One report was successfully persisted in Supabase without a corresponding PostHog event during initial configuration testing. Comparing the database records against PostHog revealed the discrepancy.

### Privacy and internal testing

AG configured and verified the following PostHog privacy and analytics settings:

- Discard client IP data was enabled.
- GeoIP enrichment was paused.
- A subsequent production submission contained no populated IP or GeoIP event properties.
- Internal test participant IDs were excluded using participant_id does not equal filters.
- Internal test filtering is enabled by default for new insights.
- The reconciliation insight retains internal test events as technical verification evidence.
- Individual participant breakdowns are maintained privately.
- The public dashboard displays aggregated results without individual participant identifiers.

*Internal filter set in PostHog:*

- User not in Internal / Test users.
- participant_id does not equal NJ's test participant UUID.
- participant_id does not equal AG's test participant UUID.

Additional internal test accounts must be excluded before their activity is included in field-week analysis.

*Test records:* NJ's and AG's internal test accounts, identified by their corresponding participant UUIDs in the private PostHog project and reconciliation evidence. Full participant identifiers are not reproduced in this document.

### Dashboard

*Dashboard:* [KIU Laundry — North Star Dashboard](https://eu.posthog.com/shared/79J4Sfi-pUs7j1g6GvNde8U5tx8amg)

AG created the three-tile analytics dashboard:

| Tile | Configuration | Status |
|---|---|---|
| North Star — Weekly Successful Reports | Weekly distinct report_id count | Configured |
| Journey Funnel — Report Submission | Explicit login, form opening, successful submission | Configured; S4 and S5 pending |
| Weekly Retention — Returning Reporters | Successful submission followed by another successful submission during subsequent weeks | Configured |

All three tiles exclude internal test accounts.

The public dashboard displays aggregate weekly counts of distinct successfully submitted reports.

The private North Star insight retains the participant-level breakdown required by the exact per-user metric.

Public dashboard access was tested without authentication.

The journey funnel will be expanded to include machine-selection and problem-selection events after Gigi completes S4 and S5.

The current funnel begins with explicit username login. Returning participants with an existing authenticated session may bypass that step.

Empty charts before independent student testing are expected and must not be interpreted as evidence of failed instrumentation or student retention.

The PostHog project timezone and weekly grouping boundaries still need to be checked against the Asia/Tbilisi ISO-week metric definition.

### S6 statistics event

AG implemented student_opened_reporting_statistics, which fires when the optional statistics panel is explicitly opened from the reporting form.

*Properties:*

- participant_id

The event does not fire when the statistics panel is closed.

*Technical verification:*

| Check | Result |
|---|---|
| TypeScript check | Passed |
| Production build | Passed |
| Automated test files | 3/3 passed |
| Existing automated tests | 24/24 passed |
| GitHub commit attribution | Avto30 |
| Production deployment | Ready |
| Production analytics event | Observed |

*Independent production verification:*

NJ independently tested the deployed statistics feature using an existing internal test participant.

| Action | Statistics events generated |
|---|---:|
| Open statistics | 1 |
| Hide statistics | 0 |
| Reopen statistics | 1 |
| Total | 2 |

Both events contained the expected participant_id.

No additional laundry report was submitted during this verification.

The existing automated tests passed, but the production walkthrough provides the direct evidence for the new analytics event.

*Implementation:* [S6 commit by Avto30](https://github.com/NitaJ5/Laundry/commit/04fcffe4694f3ab1ea09aa59bfed2ef51e18289d).

### Review and remaining work

*Checked by:* AG and NJ.

*Events and reviews still pending:*

| Event or task | Owner | Remaining work |
|---|---|---|
| student_completed_username_login | NJ | Independent GP phone review |
| student_selected_laundry_machine | GP | Implementation, deployment, and independent NJ phone review |
| student_selected_problem_type | GP | Implementation, deployment, and independent NJ phone review |
| student_opened_reporting_statistics | AG | Confirm required phone-based verification |
| Journey funnel | AG | Add S4 and S5 after GP implementation |
| Internal test filtering | AG | Exclude any additional teammate test accounts |
| Metric timezone and week boundaries | AG | Verify Asia/Tbilisi and Monday week start |
| Dashboard documentation | AG | Add public dashboard link to README.md |

The outstanding implementation and verification work is to be completed during the remaining HW2 work.

The original reconciliation discrepancy remains documented. The later production testing window successfully reconciled.

Internal technical checks do not constitute independent student usability sessions or field-week adoption evidence.
