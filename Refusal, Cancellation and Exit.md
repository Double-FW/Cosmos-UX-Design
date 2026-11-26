---
title: "Refusal, Cancellation and Exit"
description: "Help people decline optional choices, leave a task or cancel an ongoing service."
---

# Refusal, Cancellation and Exit

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Give people a way to decline an optional invitation, leave an unfinished task and cancel an ongoing service. Use wording that distinguishes these outcomes. ‘Leave application’ can mean saving a draft, while ‘Cancel subscription’ changes an ongoing arrangement.

Follow [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) for draft states, resumption and recovery after an interruption.

## Recommended markup

Use links for exit destinations and buttons for actions such as confirming cancellation. Name each action so the person can tell what it affects.

## Recommended layout

Keep exit and cancellation routes visible near the task controls. Put any explanation of lost work or access near the action.

## Recommended behaviour

### Show the route and the result

Put the exit or cancellation route where people manage the task or service. Explain what will happen, when it takes effect and what the person can still access. If leaving may lose unsaved work, explain that consequence and offer a way to return to the task. Use a confirmation step when the consequence warrants one. Explain how to go back and keep a clear keyboard exit from any dialog. See [Modal Dialogs](Modal%20Dialogs.md) and [Call to Action](Call%20to%20Action.md).

After cancellation, show what changed and when. Keep a confirmation or reference available to revisit. If cancellation fails or remains pending, say so and give a way to retry or get help. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md).

### Compare the effort

Compare starting and ending the service from equivalent starting points. Record screens, repeated decisions, waits, verification and channel changes. Keep necessary safeguards proportionate and explain them in context. If a channel change is necessary, provide an accessible route and explain who takes the request forward. See [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md).

### Respect a refusal

When someone declines an optional invitation, carry that decision forward while it applies. Keep any optional alternative offer easy to pass so it does not interrupt the exit. If circumstances change and a new decision is genuinely needed, explain the change before asking again.

## Reference implementation

### Worked example

A customer opens their subscription, chooses ‘Cancel subscription’ and reads when access will end. They can confirm or return to the subscription details. After confirmation, the subscription page shows the end date and a reference. Any offer to stay can be passed without losing the cancellation route.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="checkpoints-for-the-tester"></a>

| Method and action | Expected result |
| --- | --- |
| Start where a customer manages the service and follow the cancellation route by keyboard and screen reader. Compare it with the route for starting the service. | The cancellation route is findable and operable. Any additional effort has a clear purpose and a usable route through it. |
| Decline an optional offer, revisit the task, and cancel or go back from any confirmation dialog. | The refusal stays in effect while applicable. Going back preserves the task state and restores focus to a usable location. |
| Complete cancellation, then test a failed or pending request. | The result and effective date are clear when confirmed. A failure or pending request is distinguishable and provides the next step. |

See [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md) for related journey checks.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)

## Sources

- [Cosmos: Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md)
- [Cosmos: Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md)
