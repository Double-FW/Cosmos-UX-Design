---
title: "Saving and Resuming a Task"
description: "Help people pause a task, recover their work and find the next action when they return."
---

# Saving and Resuming a Task

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Let people pause a task they cannot complete in one sitting and return to the right place. Show what has been saved and what remains to do. After an interruption, someone may remember their goal while needing a reminder of their last step, decisions or next action.

### Scope

Use this guide for unfinished tasks such as applications, bookings and multi-step forms. Use [Forms](Forms.md) for field behaviour, [Client-side Routing](Client-side%20Routing.md) for route and focus handling, and [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md) when leaving or discarding work changes what the person can recover.

Use [Reminders and Saved Actions](Reminders%20and%20Saved%20Actions.md) when people want an optional cue to return to a saved task.

## Recommended markup

Use a labelled save button and a descriptive return link. Keep the saved state and next step available in text.

## Recommended layout

<span id="presentation"></span>

- Put a clearly labelled save action near the task controls. Use ‘Save and leave’ only if the action saves the draft and then leaves the task. Use ‘Continue application’ or another task-specific label for the return action.
- Show the save state near the task. Distinguish changes still being edited, a save in progress and a confirmed save. If a draft is stored only on this device, say so where it affects how someone can return. Keep the save result easy to find after any announcement.
- On return, show the task name, a short summary of progress, anything that changed and the next action. Let people review and edit saved answers. Give them a stable route to continue, such as a draft list in their account if the service offers one.

## Recommended behaviour

<span id="behaviour"></span>

### Save and leave

Save the completed inputs and enough context to know what remains. Keep entered values available while saving and after a failed request for as long as the current task can safely retain them. Confirm a save only after the storage location promised to the person has accepted it. Leave the task only after the save succeeds, unless the person explicitly chooses to leave with unsaved changes.

Keep focus in place during autosave. Make errors and changes that need action available as persistent feedback, with a retry or another safe way forward. Avoid announcing each autosave. Follow [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback) for feedback.

### Return and reauthenticate

Restore the last confirmed draft and choose a return point that makes the next action clear. If sign-in has expired, explain why it is needed and which saved progress remains available. After sign-in, return people to the intended task if they still have access. If the task is unavailable, explain the outcome and offer a useful destination.

When returning to a new view, use a meaningful heading to orient the person. Preserve focus for in-place updates if it is still useful. Follow [Client-side Routing](Client-side%20Routing.md#first-load-and-route-changes) for the focus change.

### Recover changes and expiry

Distinguish session expiry from draft expiry. Explain what remains, what was lost and what the person can do next. Validate saved choices against current availability before asking for confirmation. If a previously selected option is gone, preserve other valid answers and ask the person to choose again.

If another tab or device has changed the draft, identify the newer state and offer a way to review or reconcile the difference. Keep recoverable work where the service can safely retain it. Require an explicit choice before replacing a newer draft with older conflicting work.

## Reference implementation

<span id="implementation"></span>

Define what the service saves, who can reopen it, where it can be reopened, and what expires. Track the saved step and the draft version so retries and older copies cannot silently replace newer work. Keep a draft scoped to its task and authorised user. For shared devices or sensitive entries, decide what may be retained and explain any limits before the person relies on saving.

### Example: resuming an application

A person saves an application after entering their address and choosing an appointment. Their session expires before they return, and the appointment becomes unavailable.

| Starting state and action | Expected behaviour |
| --- | --- |
| The person selects ‘Save and leave’. | After confirmation, open the draft list and show ‘Application saved’ with a link to continue. If saving fails, keep the form open, retain the entered values and offer Retry. |
| The person follows the continuation link after their session expires. | Explain that sign-in is needed to continue. After successful sign-in, open the intended application and introduce the view with its heading. |
| The saved appointment is unavailable. | Keep the saved address and other valid answers. Show ‘Your appointment is no longer available’ alongside a ‘Choose another appointment’ action. Let the person review their current choices before submitting. |

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use fictitious task data and record the starting state, platform, method and result. Include keyboard and screen reader use for status feedback and return navigation.

| Method and action | Expected result |
| --- | --- |
| Enter several answers, select ‘Save and leave’, then return through the advertised route. Where cross-device access is supported, repeat on another device. | The save confirmation reflects the storage outcome. The right task, valid answers and next action are available through each advertised route. |
| Delay or fail a save, try leaving, then retry. Separately, trigger several autosaves while editing with a screen reader. | While the save is pending or has failed, the interface does not claim success or leave unexpectedly. Retained input is usable for retry. Autosave does not move focus or trigger repeated announcements. |
| Start from an expired session, follow the return route and sign in again. In a separate run use an expired or unavailable draft. | A valid draft reopens in context after sign-in. An unavailable draft gets a clear explanation and a route to recover, start again or get help where available. |
| Resume with an unavailable choice, then repeat after changing the same draft in a second tab or device. | Other valid work survives. The invalid choice has a clear correction route, and conflicting updates do not silently replace newer work. |
| In a representative task, pause and return after a delay. Ask the person to continue without supplying the next step. | Record whether they recognise the task, understand what was retained or changed and can resume or recover. Record where assistance was needed. |

Use the source evaluations to plan [task resumption](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interruption_Recovery_and_Task_Continuity.md#mem-rc-01--resume-a-paused-task) and [recovery from stale or expired state](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interruption_Recovery_and_Task_Continuity.md#mem-rc-02--recover-stale-or-expired-state). The evidence informs what to evaluate. Choose and test a return point in the actual service context.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](qa/w3c-act/e086e5.md)
- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)

## Sources

- [Interruption recovery and task continuity](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interruption_Recovery_and_Task_Continuity.md)
- [Cognitive science evidence register: preparation and task resumption](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive_Science_Evidence_Register.md#e7)
- [task resumption](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interruption_Recovery_and_Task_Continuity.md#mem-rc-01--resume-a-paused-task)
- [recovery from stale or expired state](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interruption_Recovery_and_Task_Continuity.md#mem-rc-02--recover-stale-or-expired-state)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
