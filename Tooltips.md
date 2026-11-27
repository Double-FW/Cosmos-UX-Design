---
title: Tooltips
description: Provide brief, non-interactive supplementary information without hiding labels or essential task guidance.
---

# Tooltips

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

A tooltip is a brief, non-interactive text description associated with a trigger. It can appear when a person points to the trigger or moves keyboard focus to it. It does not take focus and cannot contain controls, links, forms or other content that a person needs to operate.

Use it only for supplemental information. Do not use a tooltip as the only label or accessible name for an interactive control, or to deliver instructions needed to complete a task. Prefer a visible label. If a control has to remain compact, its accessible name must still be available when the tooltip is closed; the tooltip may repeat the label or add a short description. This supports people using screen readers and voice control.

Use a different pattern when people need to interact with the content or deliberately open and close it. For example, use an [Information Panel](Information%20Panels.md) or [Disclosure](Disclosure.md) for supplementary content that needs an explicit action, and a [Modal Dialog](Modal%20Dialogs.md) when the task requires a modal interaction. Keep essential help available in the page or through a touch-accessible control. A tooltip alone is not a reliable touch interface.

## Recommended markup

- Use a native, keyboard-focusable trigger, such as a button or link, when the tooltip is revealed on focus. Do not make a non-interactive `div` or `span` behave like a trigger unless the host design has a defined, operable pattern for it.
- Give the trigger its own accessible name independently of the tooltip. Keep visible wording in that name where a visible label exists; see [Labels](Labels.md).
- Associate the tooltip text with the trigger as an accessible description. In web implementations, `aria-describedby` can express this relationship. The tooltip container may use `role="tooltip"`, following the informative [WAI-ARIA APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/).
- Keep the tooltip outside the trigger's focus order. Do not put focusable content inside it. If users must focus or activate content inside the popup, it is not a tooltip; select a popover, dialog or other suitable pattern.
- Do not rely on the HTML `title` attribute as the only way to convey information. Browser and assistive technology behaviour varies, and a title-only message may not be available in the ways the task requires.
- Verify the relationship and announcement with the actual browser and assistive technology combinations supported by the product. The APG tooltip pattern is marked as work in progress and does not have task force consensus; treat it as implementation guidance, not a normative requirement.

## Recommended layout

- Place the tooltip close to its trigger so their relationship is clear, while keeping the text legible and avoiding the content it describes.
- Define placement rules for responsive layouts, zoom and text enlargement. Reposition when necessary so the tooltip remains on screen and is not clipped or obscured by containers.
- Specify a visible cue for a trigger when the presence of extra information would otherwise be hard to discover. Do not make the cue the only way to understand the trigger's action.
- Set readable text size, line spacing, foreground and background colours. Check the tooltip in every supported theme and in forced-colours display modes. Ensure the content and any essential visual boundary remain distinguishable.
- Keep any appearance or dismissal transition brief. Honour reduced-motion preferences by removing or reducing non-essential movement; the tooltip must work without animation.

## Recommended behaviour

1. Keep the tooltip closed until the person points to or focuses its trigger. Do not open it automatically or use a timer to make it disappear.
2. Show it on keyboard focus as well as pointer hover. Showing it must not move focus from the trigger.
3. For pointer users, keep it open while the pointer is over either the trigger or tooltip. Close it when the pointer leaves both. This allows users with magnification or a larger pointer to move onto and read the text.
4. For keyboard users, close it when focus leaves the trigger. Provide a keyboard dismissal mechanism such as Escape. After Escape dismisses it, do not immediately reopen it while the same focus or pointer condition remains unchanged.
5. Do not require a second tap, long press or non-standard gesture to use the trigger on a touchscreen. If the additional text matters to touch users, make it visible or provide an explicit, touch-accessible disclosure or information control.
6. Keep the message concise and non-interactive. If it needs more than a brief explanation, contains an action, or reports a changing status, use the relevant pattern, such as [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md).
7. Provide a usable baseline when scripts or enhancements are unavailable. Essential names, instructions and task information must remain available without the tooltip.

When content appears on pointer hover or keyboard focus, assess [WCAG 2.2 Success Criterion 1.4.13, Content on Hover or Focus](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html). Where it applies, the additional content must be dismissible without moving the pointer or focus (unless it does not obscure or replace other content), hoverable, and persistent until the trigger condition is removed or it is dismissed.

## Reference implementation

A complete tooltip needs coordinated visibility, pointer and focus handling, Escape dismissal, positioning and accessible-description behaviour. These details vary by host and supported browser/assistive technology combinations, so this guide does not present a short HTML fragment as a complete implementation.

For implementation handover, specify:

- the trigger and the supplemental text;
- the trigger's accessible name when the tooltip is closed;
- how the accessible description is associated and exposed;
- the conditions for opening, persistence, dismissal and closing;
- the touch-accessible route for any information that matters to the task;
- positioning, responsive behaviour, visual states, forced colours and reduced motion;
- supported host, browser and assistive technology combinations and the checks required for them.

Do not assume that setting an element visually hidden or applying a single HTML attribute produces the correct accessibility-tree behaviour in every implementation. Verify the closed and open states with assistive technology.

## Testing Approach and ACT Scripts

The checks below are local pattern checks, not a published W3C ACT rule. Record the tested variant, environment, method, expected and observed outcome, and evidence. Mark unrun checks as **not run**.

| Method and action | Expected result |
| --- | --- |
| Load the page without interacting with the trigger. Inspect visually and navigate with a screen reader. | The tooltip is not displayed as unrelated page content. Essential labels and instructions remain available. |
| Focus the trigger with a keyboard. Inspect focus and listen with a screen reader. | Focus remains on the trigger. Its name is available, and the supplemental description is announced in the supported implementation. |
| Move the pointer onto the trigger, then from the trigger onto the tooltip and back. | The tooltip appears and remains available while the pointer is over either area. |
| Move the pointer away from both the trigger and tooltip. | The tooltip closes. |
| With the trigger focused and tooltip open, press Escape. | The tooltip closes without moving focus. It does not immediately reopen until the interaction state changes. |
| Move keyboard focus away from the trigger. | The tooltip closes. It does not add a tab stop or interrupt the normal focus order. |
| Use the trigger on a touchscreen and with voice control where supported. | The control performs its expected action without an extra tap or non-standard gesture. Any useful supplemental information has an accessible route. |
| Zoom, enlarge text, resize or rotate the viewport, and scroll while the tooltip is open. | The tooltip remains associated with its trigger and can be read without clipping or obscuring relevant content. |
| Check contrast, text size, line spacing, forced colours and reduced motion. | Text and necessary visual cues remain distinguishable; the tooltip works without non-essential animation. |
| Disable scripts or the enhancement where feasible. | The control's name and all essential task information remain available. |

## Sources

- [A11yQuest: Tooltip pattern guide](https://a11yquest.com/pro/guides/patterns/tooltip/) — source material for design constraints and practical checks.
- [WAI-ARIA Authoring Practices Guide: Tooltip Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/) — informative pattern status, roles, relationship and interaction notes.
- [WCAG 2.2: Understanding Success Criterion 1.4.13, Content on Hover or Focus](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html) — normative success criterion and explanatory guidance.
- [HTML Living Standard: The `title` attribute](https://html.spec.whatwg.org/multipage/dom.html#the-title-attribute) — cautions about availability of title text to users of different input methods.
