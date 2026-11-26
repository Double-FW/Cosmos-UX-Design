---
title: Affordances, Signifiers and Design Conventions
description: Help people recognise available actions, predict what happens and verify the result.
---

# Affordances, Signifiers and Design Conventions

Help people tell what they can do, how to do it and what will happen. A control may be easy to notice but still lead to an unexpected result if its label, appearance and behaviour suggest different actions. Use the same meaning in the visible interface and in the names, roles and states available to assistive technology.

| Term | Design decision |
| --- | --- |
| **Affordance** | What the interface lets someone do, such as reveal delivery details or open a delivery help page. |
| **Signifier** | A cue that helps someone recognise the action or state. The visible label, disclosure marker and expanded state can each convey part of that meaning. |
| **Design convention** | A learned expectation about how something works. A link leads to a destination, while a disclosure opens and closes content in place. Familiar behaviour lets people reuse what they have learned. |

## Recommended markup

- Start with the action. Use an anchor with an `href` for a destination, a native button for an action and a native disclosure for simple content that opens in place. Follow [Anchors and Links](Anchors%20and%20Links.md#link-content), [Call to Action](Call%20to%20Action.md#make-links-and-buttons-recognisable) and [Disclosure](Disclosure.md).
- Use visible text that says what the person can do or where they will go. Keep that wording in the accessible name so people can identify and operate the control through speech input. Give repeated actions enough context to make sense in a list of links or controls. See [Labels](Labels.md).
- Expose the state that matters, such as expanded, selected or unavailable. Keep it consistent with the visible cue and actual behaviour. Prefer native semantics and keep decorative icons out of the accessible name when the text already conveys their meaning. See [Icons](Icons.md#icons-and-accessible-names).

## Recommended layout

Put the cue with the thing it describes. Place an expand marker beside the label and its content next in reading order. Place conditions such as cost or eligibility close to the action that uses them. Add visible wording when colour, shape, position or an unfamiliar icon would otherwise carry the meaning alone.

Give equivalent controls a consistent treatment throughout the service. Distinguish actions, destinations and status text where their functions differ. Keep the focus indicator distinct from hover, selection and the current item. Check the cues at narrow widths, with text enlarged and in forced colours. See [Focus](Focus.md#focus-indication).

## Recommended behaviour

### Match the result to the cue

Keep keyboard, touch and pointer behaviour consistent with the control people encounter. Give gesture-only actions a visible control and a keyboard route. On activation, make the outcome clear through the new content, state or destination. Use established focus behaviour and keep the result available to revisit. Follow [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md) to check what happens across views.

Explain a material consequence before someone commits. For example, distinguish ‘Review order’ from ‘Place order and pay £28’ and put the amount and significant conditions near the final action. See [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md#show-what-the-choice-means).

### Preserve useful conventions when things change

Record the existing label, location, appearance and result of important controls before redesigning them. Keep useful conventions where the action stays the same. If its consequence changes, update the wording and presentation at the point of action. Give returning users a chance to review the change and recover from a mistaken action where possible.

## Reference implementation

### Reveal details beside a separate help destination

Use this HTML fragment where someone can read a short delivery explanation in place and visit a fuller help page. Replace the example help route with the working destination. The native disclosure exposes its state and works without JavaScript. Keep its marker visible or supply an equivalent cue that you can test.

```html
<section aria-labelledby="delivery-heading">
  <h2 id="delivery-heading">Delivery</h2>
  <details>
    <summary>Delivery times for this order</summary>
    <p>Standard delivery takes three to five working days.</p>
  </details>
  <p><a href="/help/delivery">View delivery and collection options</a></p>
</section>
```

Before activation, one control identifies the information it reveals and the other names its destination. After activation, the disclosure keeps the person on the current page and exposes the details in place. The link opens the delivery help page through ordinary browser navigation.

## Testing notes

Use a realistic task and record the starting state, method and observed result. Include first-time and returning users when a redesign changes an important action.

| Method and action | Expected result |
| --- | --- |
| Before activation, ask what each control will do and, where relevant, what it will commit the person to. Use the actual decision screen. | People can distinguish opening content, going to another location and committing to an action. Relevant consequences can be found before acting. |
| Find and operate each control using keyboard, touch and supported speech input. With a screen reader, check names, roles and states from the reading order and available control lists. | The cues describe the same action. Names identify the controls and match their visible wording. Operation follows the control’s convention and focus stays usable. |
| Open and close the disclosure, then follow the help link. Repeat at a narrow width and in forced colours. | The disclosure’s visible and programmatic states agree and focus remains distinguishable. The link reaches the named destination. |
| Give a returning user the redesigned task without explaining what changed. Observe activation, verification and recovery. | Record whether the person recognises the new outcome, can verify it and can recover if their earlier expectation leads to a mistake. |

Inspecting markup can confirm a name, role or state. Observe realistic task use to find out whether people understand the action and its consequence.

## Sources

- [Cognitive Design Principles: Standard Elements](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive%20Design%20Principles.md#1-standard-elements)
- [Cognitive Design Principles: Affordances and Signifiers](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Cognitive%20Design%20Principles.md#2-affordances-and-signifiers)
- [Learning, redesign and returning users](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Learning_Redesign_and_Returning_Users.md)
- [Familiarity, comprehension and confidence](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Familiarity_Comprehension_and_Confidence.md)
