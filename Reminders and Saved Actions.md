---
title: "Reminders and Saved Actions"
description: "Help people remember a future action and find it even when a reminder is missed."
---

# Reminders and Saved Actions

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people save an action they intend to do later and find it again when they are ready. Someone may miss a reminder because their device suppresses it, their contact details change or delivery fails. Give them a stable route to the action inside the service.

### Scope

Use this guide for optional reminders tied to a saved task or a meaningful future event. Use [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) for restoring work in progress. Use [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md) for message delivery in the interface.

## Recommended markup

Use labelled controls for timing and channel. Name the saved task and provide a descriptive link back to it.

## Recommended layout

<span id="presentation"></span>

- Name the task and next action, such as ‘Continue your booking’ and ‘Choose a collection time’. Keep those details together in the saved item.
- Distinguish the reminder time from a deadline or expiry. Show the date, time and time zone where relevant. Explain the consequence of a real deadline in plain text.
- Make scheduling optional. Offer a simple default time where it helps, and show the timing and delivery route before the person agrees. Use [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md#defaults-and-preferences) for optional settings.
- Give people a consistent route such as ‘Saved bookings’. Keep the task, its status and its management controls available through headings, descriptive labels and ordinary keyboard navigation.

## Recommended behaviour

<span id="behaviour"></span>

### Save the action and schedule the reminder

Let people save the task without scheduling a reminder. Confirm the save only after the promised storage accepts it. Report scheduling separately. A queued reminder tells the person what the service plans to send. It provides no evidence that the person saw it or completed the task.

Keep the saved item available if scheduling or delivery fails. Where the service detects a failure, explain which part failed and offer a retry or an alternative route. Keep the person’s focus usable and show the result near the action. See [persistent inline feedback](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

### Let people manage or stop reminders

Show the timing, channel and current status with the saved item. Let people change or cancel a reminder without deleting the task. If you offer snoozing, show the new time and keep the actual deadline visible. Stop future reminders when the action is completed, cancelled, deleted or no longer relevant. Explain what an existing email or calendar copy can retain.

Keep external previews and lock-screen messages free of sensitive details. Take the person to the relevant task after any required sign-in. If it is complete or expired, explain that state and give a useful next step where available.

## Reference implementation

<span id="implementation"></span>

Track the task state separately from the reminder state. Bind each reminder to the intended task and account. Define what time zone governs the schedule, how time or deadline changes affect pending reminders and when a retry is useful. Deduplicate retries and recheck the task and reminder state before sending so obsolete queued work is suppressed.

### Example: save a collection booking

A person saves an unfinished booking. The draft expires on Friday at 5 pm. The service offers an optional email reminder and keeps the booking in ‘Saved bookings’.

| Event | What the person sees and can do |
| --- | --- |
| The booking is saved. | ‘Booking saved. Next: choose a collection time.’ The booking remains in the saved list if the person skips the reminder. |
| The reminder is scheduled. | ‘Reminder scheduled for Thursday at 10 am.’ The Friday expiry is shown separately. The person can change or cancel the reminder. |
| The service detects a delivery failure. | ‘We could not send your reminder. Your booking is still saved.’ The person can continue from the saved list or update the delivery route. |
| The booking is completed on another device. | The service cancels pending reminders. A previously sent reminder takes the person to the current booking status after any required sign-in. |

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use fictitious data and a controlled delivery fixture. Record the supported browser, assistive technology and delivery channel where applicable.

| Method and action | Expected result |
| --- | --- |
| Save without a reminder. Return through the service’s saved list. Repeat after scheduling a reminder and missing it. | Both routes expose the task, next action and any deadline, and let the person continue. |
| Use keyboard and a supported screen reader to schedule, change and cancel a reminder. Try snooze if available. | Labels are clear and timing, channel, result and expiry are distinguishable. Cancelling a reminder leaves the saved task intact. |
| Fail saving, scheduling and delivery in separate runs. Retry a queued request. | Feedback identifies the failed operation. A saved task remains available when scheduling or delivery fails. A retry does not create a duplicate reminder. |
| Change the deadline or cross a supported time-zone or daylight-saving boundary. Complete or delete the task before a queued reminder is due. | The displayed time matches the scheduling rule. Obsolete reminders are suppressed. |
| Follow an old reminder after expiry, completion or sign-out. Check previews on a shared or locked device. | The current state is clear. Private task details remain protected, and any account check occurs before those details are shown. |
| In a representative task study, ask someone to save an action and return after a delay, once with a reminder and once through the saved list. | Record whether they can find the task, identify the next action and complete or decline it. Record delivery separately from notice and completion. |

Use the source evaluations for [creating and acting on a cue](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Prospective_Memory_and_Reliable_Memory_Aids.md#mem-pm-01--create-find-and-act-on-a-cue) and [recovering from reminder failure](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Prospective_Memory_and_Reliable_Memory_Aids.md#mem-pm-02--survive-memory-aid-failure-and-deletion). Research on future intentions informs what to evaluate. Test reminder timing and usefulness in the service context.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)

## Sources

- [Prospective memory and reliable memory aids](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Prospective_Memory_and_Reliable_Memory_Aids.md)
- [Cognitive science evidence register: delayed intentions, reminders and offloading](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive_Science_Evidence_Register.md#e2)
- [creating and acting on a cue](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Prospective_Memory_and_Reliable_Memory_Aids.md#mem-pm-01--create-find-and-act-on-a-cue)
- [recovering from reminder failure](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Prospective_Memory_and_Reliable_Memory_Aids.md#mem-pm-02--survive-memory-aid-failure-and-deletion)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
