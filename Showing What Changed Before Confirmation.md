---
title: Showing What Changed Before Confirmation
description: Help people review material changes and confirm the details they intend.
---

# Showing What Changed Before Confirmation

Show people what changed when a price, destination, selected option or condition changes before they commit. Put the current detail and its consequence together so the person can decide whether to continue without remembering an earlier screen.

## Scope

Use this guide when information changes after someone has seen or chosen it, including after returning to a draft. Use [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md#show-what-the-choice-means) for the initial decision and [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) to restore the draft.

## Presentation

- Name the affected item and its context. For example, say ‘Delivery address for this order’ where the account also has a billing address. When two items have similar names, show a useful distinguishing detail that people can find with assistive technology.
- Put the current value near the action that uses it. Show the earlier value alongside it when comparison helps, with plain labels such as ‘Previously shown’ and ‘Current total’. Explain the effect, such as a different amount payable or a later appointment.
- Use visible text to identify each material change. Keep related changes together under a descriptive heading. Follow [Making Important Information Stand Out](Making%20Important%20Information%20Stand%20Out.md) for emphasis. Use a specific correction action such as ‘Change delivery option’.

## Behaviour

### Decide what needs another review

Treat a change as material when it could reasonably affect the decision, such as a change in cost, recipient, delivery, renewal terms or availability. Decide what matters for each task and specify which changes require fresh confirmation. Routine background updates can remain quiet when they leave the decision intact.

Before completing the action, show the current details and give the person a chance to review, change or leave. Retain valid choices and entries. If the selected option is unavailable, explain that and ask the person to choose again before proceeding. Follow [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md) for the route out.

### Make the review usable

For an in-place update, preserve focus where it remains useful. If an attempted confirmation opens a new review view, orient the person at its heading. Keep the changed details available for rereading in either case. Use [persistent inline feedback](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback) when someone could otherwise miss a change. If moving focus already communicates the message, avoid a duplicate live announcement.

Use a confirmation action that describes the actual commitment, such as ‘Pay £28’. After the action, show the accepted outcome or explain that it is pending or failed. Follow [Call to Action](Call%20to%20Action.md) for action labels and [Forms](Forms.md#correcting-a-field) for form recovery.

## Implementation

Record the identity and material values shown at review. When processing confirmation, check those values against the current authoritative state as part of the same operation that accepts the action. If they have changed materially, stop the pending commitment and show a fresh review. Carry the newly reviewed values forward only after another explicit confirmation. Handle repeat submissions so a retry cannot create a second commitment.

Use the same current values in the review, confirmation action and accepted outcome. If the service cannot determine the outcome after a connection failure, show that uncertainty and offer a way to check before asking the person to submit again.

### Example: delivery cost changes before payment

A person reviews an order at £24, including delivery. The delivery charge changes before payment. The person’s delivery address and other valid choices stay available.

| Step | What the person sees and can do |
| --- | --- |
| They first review the order. | ‘Order total £24’ and ‘Pay £24’. |
| They choose ‘Pay £24’ after the charge changes. | The service leaves the order unpaid. The review says ‘Delivery charge increased by £4’, ‘Previously shown £24’ and ‘Current total £28’. They can change delivery, leave or choose ‘Pay £28’. |
| They choose ‘Pay £28’ and the service accepts it. | The confirmation shows that £28 was paid. If the total changes again before acceptance, the service returns to review. |

## Checkpoints for the tester

Use controlled fixtures and fictitious data. Record the values shown at review, the action requested and the accepted outcome, where there is one.

| Method and action | Expected result |
| --- | --- |
| Review a booking or order. Change a material value before activation, then repeat with the change occurring after activation and before acceptance. | Both paths prevent commitment to unseen values. The review identifies the affected item, current values, consequence and next action. |
| Compare two similar recipients or destinations. Change one distinguishing detail after review. | The affected entity can be identified without recalling an earlier view. The change is clear in text and the correction action goes to the right place. |
| Use keyboard and a supported screen reader to review an in-place change and a change that opens a new review view. Go back to edit. | Focus stays usable or reaches the review heading as appropriate. The change can be reread without competing announcements. Valid selections and answers remain available. |
| Change a routine detail that leaves the decision intact, then remove a selected option. | The routine update does not force another decision. The unavailable option is explained and the person can choose a valid replacement. |
| Accept an updated review, then repeat with a delayed response, a retry and a further material change. | The accepted outcome matches the reviewed values. A retry does not duplicate the action. A further material change requires another review before commitment. |

Use the [current-context evaluation](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interference_Source_Memory_and_Current_Context.md#mem-ct-01--distinguish-similar-current-contexts) and [stale-information evaluation](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interference_Source_Memory_and_Current_Context.md#mem-ct-02--detect-stale-or-superseded-information) to assess whether people can explain what will happen and choose the intended outcome. The research informs what to investigate. Validate understanding in the service context.

## Sources

- [Interference, source memory and current context](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Interference_Source_Memory_and_Current_Context.md)
- [Familiarity, comprehension and confidence](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Familiarity_Comprehension_and_Confidence.md)
- [Cognitive science evidence register: interference and item or association recognition](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive_Science_Evidence_Register.md#e4)
