---
title: Call to Action
description: Help people find, understand and activate the next step in a journey.
---

# Call to Action

Use a call to action (CTA) when people need to spot an important next step, such as starting an application, viewing an offer or completing an order. Make it clear what will happen before they activate it. A prominent control can still be hard to find if its purpose is unclear, it appears too late in reading order or its focus indicator disappears.

## Presentation

Give the main action enough prominence to distinguish it from alternatives. Put it near the information a person needs to decide. Keep the information and action in a meaningful reading and focus order at narrow widths and high zoom. Use a visible label that names the step. For example, ‘Check eligibility’ tells people more than ‘Learn more’. Keep the visible wording in the accessible name so people can refer to it by voice. See [Labels](Labels.md).

| What the person needs to do | Example label | What to make clear |
| --- | --- | --- |
| Start a journey | Start an application | What the person is starting and what they need before beginning. |
| Get an offer or benefit | Apply the discount | Eligibility, significant conditions and when the benefit takes effect. |
| Understand a product, offer or decision | Check eligibility | What the destination explains or what information the check will ask for. |
| Complete a task | Confirm order | What the action commits the person to and what confirmation to expect. |
| Respond to a time-sensitive request | Update payment details | The reason, any real deadline and what happens if the person takes no action. |

For promotions, offers and incentives, make material conditions available before the person commits. Distinguish real urgency through wording and context so the meaning stays clear without colour or animation. Use [Promotional Cards](Promotional%20Cards.md) when the page needs a short preview of the destination as well as a link.

### Make links and buttons recognisable

Use consistent visual treatments that help people predict whether a control navigates or performs an action. An underline can reinforce a link. Keep the control’s text, boundary where needed and keyboard focus visible in forced colours. A border can preserve the outline of a styled CTA when its background changes. See [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) and [Focus](Focus.md#focus-indication).

Use [Affordances, Signifiers and Design Conventions](Affordances%2C%20Signifiers%20and%20Design%20Conventions.md) to check whether a control’s appearance, name and behaviour lead people to expect the same action.

### Give pointer users room to activate the action

Under WCAG 2.2 [2.5.8 Target Size (Minimum)](https://w3c.github.io/wcag/understanding/target-size-minimum.html) (Level AA), pointer targets must be at least 24 by 24 CSS pixels unless an exception applies. Measure the actual clickable area. If relying on the spacing exception, centre a circle 24 CSS pixels in diameter on the bounding box of the undersized target. It must avoid intersecting another target or the circle of another undersized target. Inline links in sentences can qualify for the inline exception.

Consider a larger design target such as 44 by 44 CSS pixels for prominent standalone actions. Leave enough room between nearby controls for people to select the intended one. See [the touch target example](Rating%20Stars.md#touch-target-size).

## Behaviour

Use the element that matches the outcome.

- Use a link with a working `href` to take people to a new page, screen or section. Enter follows the link. Preserve ordinary link behaviour, including the option to open the destination in a new tab. See [Anchors and Links](Anchors%20and%20Links.md).
- Use a button to perform an action, such as applying a discount or opening a dialog. Enter and Space activate a native button. Use `type="submit"` to submit a form and `type="button"` for other button actions. See [Forms](Forms.md) and [Modal Dialogs](Modal%20Dialogs.md).

After an action, show the result or explain what the person can do to recover. If applying a discount fails, explain why and keep the basket available. Follow [status message guidance](Dynamic%20Content.md#status-messages) for in-place feedback.

## Implementation

This fragment shows two distinct actions. The link opens the offer details and the button submits a form to apply an offer. The service supplies both routes and reports the result of submission.

```html
<p><a href="/offers/delivery">View the delivery offer</a></p>

<form action="/basket/discount" method="post">
  <button type="submit">Apply delivery discount</button>
</form>
```

## Checkpoints for the tester

| Method and action | Expected result |
| --- | --- |
| Find the main action visually and then through keyboard or screen reader navigation. Read its label and the preceding context. | The action is easy to locate and its wording distinguishes it from nearby choices. Conditions, deadlines and consequences needed for a decision are available before activation. |
| Inspect the role and accessible name. Use Enter on a link and Enter and Space on a button. | The visible wording is included in the name. The role and keys match the behaviour, and activation reaches the stated destination or performs the stated action. |
| Increase text size and test a narrow viewport separately. Turn on forced colours and Tab to the CTA. | The label, any boundary needed to identify the control and its focus indicator remain visible. The action stays in a meaningful order relative to the information it needs. |
| Measure the actual pointer target and, where needed, check adjacent targets against the spacing exception. Try the supported touch and pointer inputs. | The target meets the applicable criterion and people can activate it without accidentally selecting a neighbouring control. |
| Complete an action, then test an ineligible or failed attempt where relevant. | The result matches the promise made by the label. Failure explains what happened and how to continue. |

## Sources

- [Understanding Target Size (Minimum)](https://w3c.github.io/wcag/understanding/target-size-minimum.html)
- [Survey of users with low vision](https://webaim.org/projects/lowvisionsurvey2/)
