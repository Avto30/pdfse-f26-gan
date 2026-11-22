
# Event Schema

**North star:** Successfully submitted laundry equipment-problem reports per week per user.

**North star event:** `student_submitted_problem_report`

**How the north star is computed from events:**

Count unique successfully submitted laundry equipment-problem reports for each participant during each ISO week, using the Asia/Tbilisi time zone and Monday as the beginning of the week.

Group successful submission events by `participant_id`, ISO year, and ISO week. Deduplicate by `report_id` so repeated event delivery does not count one saved report multiple times.

The metric measures successful reporting activity, not equipment repairs or verified machine faults.

## Events

The following events are planned for PostHog instrumentation. The application already supports the underlying user actions, but these PostHog events have not yet been implemented or verified.

| Step | Event (actor_verb_object) | Fired by | Properties (never a name, phone, email, or message text) | Owner |
|---|---|---|---|---|
| Acquisition / access | `student_completed_username_login` | Student, after successful username login | `participant_id`, `login_success: true` | Gigi |
| Activation (precursor) | `student_opened_problem_form` | Student, when the reporting form becomes available after authentication | `participant_id` | Gigi |
| Activation (precursor) | `student_selected_laundry_machine` | Student, when a nonempty machine identifier is selected | `participant_id`, `building`, `machine_type`, `machine_id` | Gigi |
| Activation (precursor) | `student_selected_problem_type` | Student, when selecting a predefined problem or the custom problem option | `participant_id`, `machine_type`, `selection_kind` (predefined or custom; no custom description text) | Gigi |
| Activation (north star) ★ | `student_submitted_problem_report` | Student, after the server confirms that the report was successfully saved in Supabase | `participant_id`, `report_id`, `building`, `machine_type`, `machine_id`, `submitted_at` | Gigi |
| Retention / engagement precursor | `student_opened_reporting_statistics` | Student, when they explicitly open the statistics panel from the reporting form | `participant_id` | Gigi |


## Rules we are following

- Each event uses one consistent lowercase `actor_verb_object` name with underscores.
- Every event includes the application's persistent, pseudonymous `participant_id`.
- PostHog should use the same participant UUID consistently for event attribution.
- We do not send usernames, real names, phone numbers, email addresses, custom problem descriptions, or session tokens to PostHog.
- `report_id` is the unique database identifier of a saved report, not a participant identifier.
- All report events must refer to the actual selected building, machine type, and machine identifier.
- The North Star event fires only after successful database persistence, never merely when the Submit button is clicked.
- Failed form validation and failed database submissions must not fire the North Star event.
- Multiple clicks, retries, or repeated event delivery must not increase the count for the same saved report.
- Viewing statistics is an engagement action, not proof of long-term retention.
- These are planned analytics events. Existing Supabase records do not prove that PostHog events were captured.

## Reconcile plan

Before trusting the analytics dashboard, we will compare PostHog events against the authoritative reports stored in Supabase.

**Event most likely to fail silently:** `student_submitted_problem_report`

**How it would fail:**

A student may successfully save a report to Supabase, but the PostHog event might not be delivered because of a network interruption, blocked analytics request, or missing instrumentation.

Alternatively, an analytics retry could deliver the same event twice, inflating the reported submission count.

**The check that would catch it:**

1. Select one completed ISO week using the Asia/Tbilisi time zone.
2. Count saved reports in `public.laundry_reports`, grouped by participant UUID.
3. Count distinct PostHog `student_submitted_problem_report` events by `report_id`, grouped by the same participant UUID and week.
4. Compare the PostHog result with the corresponding Supabase counts.
5. Investigate any missing, duplicated, or mismatched events.
6. Confirm that the North Star event is emitted only for a successfully persisted report.

For reporting accuracy, Supabase remains the source of truth until event counts have been reconciled.

**Who runs the check in Week 7:** Avto, with Gigi reviewing application event capture and Gigi documenting the outcome.

**Current verification status:** The application stores real reports and supports database-backed reporting statistics. PostHog instrumentation and database-to-analytics reconciliation are not yet complete.
