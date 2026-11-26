---
title: "Support, Escalation and Handoffs"
description: "Help people reach support, know who owns the next step and recover if a transfer fails."
---

# Support, Escalation and Handoffs

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Give people a way to ask for help without repeating the barrier that stopped them. A link to support is only the first step. Check whether the person can reach a usable channel, explain the problem and get a response.

## Recommended markup

Use descriptive links for support destinations and labelled buttons for in-place actions. Put the request reference and next step in text.

## Recommended layout

### Make support findable and usable

Provide a clear support entry in a consistent place and a relevant link where a task can fail. Explain what each channel can help with, what information is needed and when people can expect a response. Where a usual channel is inaccessible or unavailable, provide an alternative people can use for the same issue. For example, provide a written route for someone who cannot use a phone. See [Global Navigation](Global%20Navigation.md) and [Forms](Forms.md).

## Recommended behaviour

### Carry the task across a handoff

Before transferring someone to a different team, system or channel, say who will handle it, what will happen next and how they can check progress. Carry forward relevant information already provided where appropriate. Explain what will be shared and give the person a reference they can use if they need to follow up.

Track whether the receiving team or service took ownership. If a transfer fails, keep the original request available and show a way to retry or continue. For an asynchronous handoff, keep the status and next step available in the task or in a [notification centre](Notifications%20and%20Status%20Messages.md#notification-centre).

### Provide an escalation path

Show how to follow up when the issue remains unresolved. Let the person use their existing reference and provide a route that remains usable if the original channel or account is unavailable. Escalation works when a named team can act on the request and the person can tell where responsibility sits.

## Reference implementation

### Worked example

A customer cannot complete an identity check. They use a written support form and receive a reference. Support transfers the request to the verification team and explains when the customer can expect an update. If the transfer fails, the support record stays available and the customer can follow up with the same reference.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="checkpoints-for-the-tester"></a>

| Method and action | Expected result |
| --- | --- |
| Start from a failed task and try to reach support with the available keyboard and screen reader routes. Repeat with the usual channel unavailable. | An accessible route can handle the same problem. The person can find any requirements and know what to expect. |
| Follow a request across a team or channel transfer. | The person knows who owns the next step and can use an existing reference. Relevant information carries forward where appropriate. |
| Simulate a failed transfer or no response within the stated time. Follow the retry or escalation route. | The original request remains available. The person can tell who to contact and can continue without starting the original task again. |

See [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md) for related journey checks.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)

## Sources

- [Cosmos: Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md)
- [Cosmos: Notification centre](Notifications%20and%20Status%20Messages.md#notification-centre)
