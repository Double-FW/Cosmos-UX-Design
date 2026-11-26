---
title: Making Important Information Stand Out
description: Help people notice and act on material changes without competing visual cues or screen reader announcements.
---

# Making Important Information Stand Out

Help people notice what matters for their current task and find it again when they need to act. When several banners, badges or announcements compete for attention, someone may miss a change or lose their place in the task.

## Scope

Use this guide when choosing what to emphasise across a page or app view. Use [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md) to choose and implement the message pattern. Use [Forms](Forms.md#error-messages) for validation and field errors.

## Guidance

### Decide what matters now

List the messages and cues that can appear together. For each one, identify what changed, who needs to know, what they can do and what happens if they wait. Give the strongest emphasis to a change that materially affects the current task. Make concurrent issues easy to distinguish and group related messages where that helps people act on them. Keep time limits and consequences accurate. See [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md) when reviewing urgency cues.

### Use the pop-out effect

A cue can stand out when it differs from its surroundings. Start by placing a clear message near the content it affects. Use space, type or a distinct border to help people find it. Write what changed and what to do next in visible text so people can understand it without interpreting a colour or icon.

Keep familiar controls and priority styles stable. Remove or tone down competing emphasis before adding more. Reserve the strongest treatment for meaningful changes and make it distinct from the keyboard focus indicator. Prefer a static cue. If you add motion, follow [reduced motion guidance](Motion%20and%20Animation.md#reduced-motion-preferences). See [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) for alternate cues and forced colours.

### Choose what needs announcing

Decide whether to announce a change separately from how strongly it is styled. Keep important information in the reading order near its context, with a heading or label where it helps people find it again.

| Situation | Delivery |
| --- | --- |
| The person can find the information through ordinary reading or navigation. | Keep it visible and use ordinary text, headings and labels. |
| A meaningful change happens away from focus and the person could miss it. | Use a polite status message when appropriate. Keep focus on the current task and keep any action available beside the affected content. |
| The person needs to act immediately because waiting has a serious consequence. | Consider an assertive announcement. Keep the consequence and action available after the announcement. If a decision must block the current task, follow [Modal Dialogs](Modal%20Dialogs.md). |

Group routine updates where a summary helps. Deduplicate retries and repeated events. If the interaction moves focus to a message and that focus change already communicates it, avoid a second live announcement of the same content. Keep genuine changes in consequence eligible for a fresh update. Follow [Dynamic Content](Dynamic%20Content.md#announcing-status-updates) for implementation.

## Implementation

### Example: a collection slot is no longer available

A person is reviewing a booking while stock updates arrive in the background. Keep routine refreshes quiet. When their selected slot becomes unavailable, put **“Your collection time is no longer available”** beside the selected time and provide a **“Choose another time”** action. Keep the message available until it is resolved or superseded.

If the person is still reading elsewhere in the view, use the existing polite status region to announce the change when it is confirmed. Keep focus in place. If the interface later explains the same issue after a submit attempt, follow the form’s error handling and check that the error is conveyed without overlapping announcements.

## Checks

Use a representative page with a current task, routine updates and a consequential change. Record the starting state, method, supported browser and assistive technology where applicable, and the observed result.

| Method and action | Expected result |
| --- | --- |
| Review a busy state with the main task, secondary promotions, badges and a consequential message visible together. Repeat in forced colours and with reduced motion. | The message can be located from its placement and text. Its priority remains understandable, and it is distinct from keyboard focus. |
| With a screen reader running, trigger a meaningful background change, routine updates and a retry of the same event. Continue reading or editing while they occur. | The update policy is followed. The meaningful update is communicated without competing duplicate announcements or unexpected focus movement. |
| Trigger a form error that moves focus to an error summary. Then revisit the original task and navigate to the affected choice. | The focused summary communicates the issue without an identical live alert. The affected choice and its recovery action remain findable in the task. |
| In a repeated-use study, include familiar routine messages and a later consequential change. Give participants the task without telling them to look for the message. | Record whether people notice the change, understand it, take the intended action and resume their task. Record missed cues and interruptions separately. |

A technical check can confirm markup, focus and event delivery. Use the [repeated-exposure evaluation](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Warnings_Habituation_and_Notification_Management.md#mem-nt-02--evaluate-repeated-exposure-and-meaningful-change) to assess whether people still notice and understand the cue after repeated use. The source evidence concerns repeated visual warning images. It leaves spoken-alert outcomes and the effectiveness of a specific design to be evaluated in context.

## Sources

- [Cognitive Design Principles: Variable Stimuli and the Pop-Out Effect](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive%20Design%20Principles.md#8-variable-stimuli-and-the-pop-out-effect)
- [Warnings, habituation and notification management](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Warnings_Habituation_and_Notification_Management.md)
- [Cognitive science evidence register: repeated warnings and habituation](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive_Science_Evidence_Register.md#e6)
