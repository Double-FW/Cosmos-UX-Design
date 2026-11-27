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
- [Accessibility Requirements](#accessibility-requirements)
- [ACT Test Cases](#act-test-cases)
- [Sources](#sources)

## Introduction

A tooltip is a brief, non-interactive text description associated with a trigger. It can appear when a person points to the trigger or moves keyboard focus to it. It does not take focus and cannot contain controls, links, forms or other content that a person needs to operate.

Use it only for supplemental information. Do not use a tooltip as the only label or accessible name for an interactive control, or to deliver instructions needed to complete a task. Prefer a visible label. If a control has to remain compact, its accessible name must still be available when the tooltip is closed; the tooltip may repeat the label or add a short description. This supports people using screen readers and voice control.

Use a different pattern when people need to interact with the content or deliberately open and close it. For example, use an [Information Panel](Information%20Panels.md) or [Disclosure](Disclosure.md) for supplementary content that needs an explicit action, and a [Modal Dialog](Modal%20Dialogs.md) when the task requires a modal interaction. Keep essential help available in the page or through a touch-accessible control. A tooltip alone is not a reliable touch interface.

## Recommended markup

- Use a native, keyboard-focusable trigger, such as a button or link, when the tooltip is revealed on focus. Do not make a non-interactive `div` or `span` behave like a trigger unless the host design has a defined, operable pattern for it.
- Give the trigger its own accessible name independently of the tooltip. Keep visible wording in that name where a visible label exists; see [Labels](Labels.md).
- Associate the tooltip text with its trigger using `aria-describedby`, and identify the tooltip container with `role="tooltip"`, following the informative [WAI-ARIA APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/). Keep the trigger's accessible name independent of this description. Test the relationship with the target browser and assistive technology combinations.
- Keep the tooltip outside the trigger's focus order. Do not put focusable content inside it. If users must focus or activate content inside the popup, it is not a tooltip; select a popover, dialog or other suitable pattern.
- Do not rely on the HTML `title` attribute as the only way to convey information. Browser and assistive technology behaviour varies, and a title-only message may not be available in the ways the task requires.
- Verify the relationship and announcement with the actual browser and assistive technology combinations supported by the product. The APG tooltip pattern is marked as work in progress and does not have task force consensus; treat it as implementation guidance, not a normative requirement.

## Recommended layout

- Place the tooltip close to its trigger so their relationship is clear, while keeping the text legible and avoiding the content it describes.
- Define placement rules for responsive layouts, zoom and text enlargement. Reposition when necessary so the tooltip remains on screen and is not clipped or obscured by containers.
- Specify a visible cue for a trigger when the presence of extra information would otherwise be hard to discover. Do not make the cue the only way to understand the trigger's action.
- Set readable text size, line spacing, foreground and background colours. Meet [WCAG 2.2 SC 1.4.3 Contrast (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html): text contrast is at least 4.5:1, or 3:1 for large text as defined by the criterion. Check the tooltip in every supported theme and in forced-colours display modes. Ensure the content and any essential visual boundary remain distinguishable.
- Keep any appearance or dismissal transition brief. Honour reduced-motion preferences by removing or reducing non-essential movement; the tooltip must work without animation.

## Recommended behaviour

1. Keep the tooltip closed until the person points to or focuses its trigger. Do not open it automatically or use a timer to make it disappear.
2. Show it on keyboard focus as well as pointer hover. Showing it must not move focus from the trigger.
3. For pointer users, keep it open while the pointer is over either the trigger or tooltip. Close it when the pointer leaves both. This allows users with magnification or a larger pointer to move onto and read the text.
4. For keyboard users, close it when focus leaves the trigger. Provide a keyboard dismissal mechanism such as Escape. After Escape dismisses it, do not immediately reopen it while the same focus or pointer condition remains unchanged.
5. Do not require a second tap, long press or non-standard gesture to use the trigger on a touchscreen. If the additional text matters to touch users, make it visible or provide an explicit, touch-accessible disclosure or information control.
6. Keep the message concise and non-interactive. If it needs more than a brief explanation, contains an action, or reports a changing status, use the relevant pattern, such as [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md).
7. Provide a usable baseline when scripts or enhancements are unavailable. Essential names, instructions and task information must remain available without the tooltip.
8. Show no more than one tooltip at a time in the same interface. When a different trigger opens its tooltip, close the previously visible tooltip.
9. Keep the tooltip non-focusable and free of interactive descendants so keyboard focus cannot become trapped. Tab must continue to move between controls normally.
10. If a touch interaction displays a tooltip, tapping outside it or activating another element closes it. Do not use a long press as the only way to expose information needed for the task.

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

The checks below include local pattern checks and the supplied test cases. The source calls its cases ACT test cases; they are not W3C ACT Rules with published rule identifiers. Record the tested variant, environment, method, expected and observed outcome, and evidence. Mark unrun checks as **not run**.

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


## Accessibility Requirements

The Tooltip component is evaluated against the following WCAG 2.2 success criteria, categorized under the Perceivable, Operable, and Robust principles:

### Perceivable

- 1.4.13 Content on Hover or Focus: Ensures that content displayed on hover or focus is dismissible, hoverable, and persistent.

### Operable

- 2.1.1 Keyboard: The tooltip must be operable through a keyboard interface without requiring specific timings for individual keystrokes.
- 2.1.2 No Keyboard Trap: Keyboard users must be able to navigate to and from the tooltip without getting trapped.

### Robust

- 4.1.2 Name, Role, Value: The tooltip must be properly labeled with ARIA attributes to ensure that assistive technologies can interpret and present the information correctly.

## ACT Test Cases

### Test Case 1: Screen Reader Accessibility and Hidden State Verification

#### Objective

To verify that tooltips are not accessible or announced by screen readers in their default (hidden) state and are properly announced when active.

#### Test Steps

1. Using a screen reader, navigate through the page without interacting with the trigger element to ensure the tooltip content is not announced in its default state.
2. Focus on the trigger element using the keyboard or the screen reader’s virtual cursor.
3. Confirm that the screen reader announces the tooltip’s presence and content.

#### Expected Results

- In its default state, the tooltip content is not accessible or announced by the screen reader.
- When the trigger element is focused, the tooltip becomes accessible and its content is announced.

### Test Case 2: Visibility and Interaction with Mouse

#### Objective

To ensure the tooltip becomes visible with mouse hover and behaves correctly with cursor movements.

#### Test Steps

1. Hover the mouse over the trigger element and verify that the tooltip becomes visible.
2. Move the cursor from the trigger element to the tooltip and back without the tooltip hiding.
3. Move the cursor away from both the trigger element and the tooltip, ensuring the tooltip hides.

#### Expected Results

- The tooltip shows and hides according to mouse cursor movement as specified.

### Test Case 3: Keyboard Interaction and Escape Key Functionality

#### Objective

To confirm that tooltips are accessible via keyboard and can be dismissed using the escape key.

#### Test Steps

1. Tab to focus the trigger element and verify that the tooltip becomes visible.
2. While the tooltip is visible, press the escape key and check that it hides.
3. Tab away from the trigger element to another focusable element and ensure the tooltip hides.

#### Expected Results

- The tooltip behaves as expected with keyboard interactions, including showing, hiding with the escape key, and hiding when moving focus away.

### Test Case 4: Single Tooltip Visibility

#### Objective

To ensure that only one tooltip is displayed at a time.

#### Test Steps

1. Activate a tooltip by hovering or focusing on a trigger element.
2. Without dismissing the first tooltip, activate another tooltip by hovering or focusing on a different trigger element.
3. Verify that the first tooltip hides when the second tooltip becomes visible.

#### Expected Results

- Only one tooltip is visible at a time, ensuring clear and uncluttered presentation of supplementary information.

### Test Case 5: Touchscreen Interaction

#### Objective

To ensure tooltips do not disrupt usability for touchscreen users.

#### Test Steps

1. On a touchscreen device, tap on an element with an attached tooltip to verify the primary action of the element is executed without displaying the tooltip.
2. If a tooltip is displayed (e.g., after a long press), tapping outside the tooltip or on another element hides the tooltip.

#### Expected Results

- Tooltips do not interfere with the expected behavior of interactive elements on touchscreen devices.

### Test Case 6: Content Accessibility and Visibility Enhancements

#### Objective

To check that the tooltip content is accessible, with adequate contrast, and remains visible under various conditions such as scrolling and high contrast mode.

#### Test Steps

1. Verify the tooltip content meets the minimum contrast ratio requirements against its background.
2. Test the tooltip’s visibility and readability during scrolling, zooming in, resizing the window, or rotating the device.
3. Enable high contrast mode and ensure the tooltip content remains visible and readable.

#### Expected Results

- The tooltip’s content maintains accessibility standards in different scenarios, including sufficient contrast ratio, readability during dynamic view changes, and visibility in high contrast mode.


## Sources

- [A11yQuest: Tooltip pattern guide](https://a11yquest.com/pro/guides/patterns/tooltip/) — source material for design constraints and practical checks.
- [WAI-ARIA Authoring Practices Guide: Tooltip Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/) — informative pattern status, roles, relationship and interaction notes.
- [WCAG 2.2: Understanding Success Criterion 1.4.13, Content on Hover or Focus](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html) — normative success criterion and explanatory guidance.
- [WCAG 2.2: Understanding Success Criterion 1.4.3, Contrast (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html) — text contrast requirements.
- [WCAG 2.2: Success Criterion 2.1.1, Keyboard](https://www.w3.org/TR/WCAG22/#keyboard) and [2.1.2, No Keyboard Trap](https://www.w3.org/TR/WCAG22/#no-keyboard-trap) — keyboard operation and focus movement.
- [WCAG 2.2: Understanding Success Criterion 4.1.2, Name, Role, Value](https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html) — programmatic names, roles, states and values.
- [HTML Living Standard: The `title` attribute](https://html.spec.whatwg.org/multipage/dom.html#the-title-attribute) — cautions about availability of title text to users of different input methods.
