# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved PRD N-17, 2026-09-18) and `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` is superseded history and approves nothing in this intent.

## Problem

The fictional clinic receives avoidable calls from patients checking whether their results are ready. Patients have no reliable signal that a reviewed result is waiting for them, so they phone the clinic to ask about status. (PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready.")

## Proposed outcome

- **I-1 Notification gate:** a patient becomes eligible for a notification only after a doctor has reviewed and released the result. A finished laboratory upload alone does not make a result "ready". (PRD: "A doctor must review and release the result before any notification is eligible."; discovery notes, product owner: "'Ready' must not mean that the laboratory upload merely finished.")
- **I-2 Destination:** the notification directs the patient to the result in the existing authenticated portal, through the existing portal result route. (PRD: "Use the existing portal result route.")
- **I-3 Message content:** the message only says that an update is available and points to the portal. (PRD: "The notification says that an update is available and directs the patient to the existing authenticated portal.")

**How success will be observed (I-5):** compared against a measured baseline,
- the number of status-only calls ("are my results ready?"), counted separately from clinical questions, goes down;
- delivery failures are tracked.

The baseline and any numerical target will be agreed during discovery. No target is approved, so this intent sets none. (PRD: "Measure reduction in results-status calls and delivery failures. The baseline and numerical target will be agreed during discovery; none is approved yet."; discovery notes, support lead: "count status-only calls separately from clinical questions; compare against a measured baseline.")

## Affected users and systems

- **Patients:** the only approved recipients of the notification.
- **Doctors:** their review-and-release step is the trigger for eligibility.
- **Clinic support staff:** they handle the status calls this should reduce, and they measure the baseline.
- **Existing authenticated portal and its result route:** this is where the patient views the result. Engineering lead: "the existing portal path is adequate."
- **Notification delivery and operational/notification logs:** these are subject to the content and duplicate rules below.

## Out of scope

All of the following are excluded (PRD: "Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope."):

- portal redesign;
- appointment scheduling;
- clinical interpretation;
- notifying third parties;
- a new or rebuilt patient dashboard, which is a separate project. (Discovery notes, engineering lead: "A new patient dashboard is a separate project.")

The superseded request's ideas are also not approved:
- notifying on lab upload;
- putting the test result in an SMS;
- copying the referring doctor (see OPEN-2);
- rebuilding the dashboard;
- a 90% call-reduction target in one week.

## Constraints

- **I-4 No clinical detail:** no diagnosis, test name or result, clinical measurement or treatment detail may appear in the message or in notification/operational logs. (PRD: "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs."; discovery notes, privacy owner: "no sensitive content in messages or operational logs.")
- **I-6 Authenticated access only:** patients access results through the existing authenticated portal. The notification attaches no results and introduces no public result link. (PRD: "Authenticate access in the existing portal. Do not attach results or introduce a public result link.")
- **I-7 No duplicates, safe failures:** each released result version produces at most one notification. Delivery failures are handled without sending duplicate patient messages. (PRD: "Avoid duplicate notifications for the same released result version; handle delivery failures without duplicate patient messages.")
- **I-8 Patient only:** only the patient is notified. No other recipient is approved until OPEN-2 is decided.

## Open questions and owners

- **OPEN-1 Channel (SMS, email or portal push):** not decided. **Owners: product owner and privacy owner**, after the consent/delivery review. This intent does not choose a channel. (PRD: "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review."; discovery notes, privacy owner: "Consent and the actual channel are still under review.")
- **OPEN-2 Referring-doctor notification:** not approved. **Owners: clinical owner and privacy owner**, who must decide recipient authorization. Approval is not inferred from the superseded request. (PRD: "Notification to the referring doctor: clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD."; discovery notes, clinical owner: "do not infer approval for referring-doctor notifications from the old request.")

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
