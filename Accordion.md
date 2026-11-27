---
title: "Accordion"
description: "Help people find and expand relevant sections on the same page."
---

# Accordion

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use an accordion to help people scan two or more related sections and open the ones they need. Collapsing sections can reduce scrolling, but it can also hide information people need to finish a task. Keep essential instructions visible. Use a single [Disclosure](Disclosure.md) for one section. For cross-cutting guidance on hidden states, focus and accessibility-tree exposure, see [Visually Hidden Content](Visually%20Hidden%20Content.md) and [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md).

## Recommended markup

Use native `details` and `summary`, or a named button in a heading with an associated panel.

## Recommended layout

<span id="presentation"></span>

Give each control a visible label that describes the panel it opens, such as ‘Ingredients’ or ‘Contact information’. Keep the control visible and recognisable as interactive in both states. Show the state with a native marker or a visible change in shape, such as a plus changing to a minus. Keep the associated panel immediately after its control or enclosing heading in source order.

Use a visible heading to introduce the group. Where a named landmark helps people find the group, use a `section` labelled by that heading with `aria-labelledby`. Use heading levels that fit the page for substantial sections. A list can express the relationship between short items, such as questions with brief answers.

Let long labels wrap without overlapping or squeezing out the indicator. Check that the control boundary, state and keyboard focus remain visible in forced colours. If the design places one control per row at narrow widths, preserve the accordion interaction. A [tab interface](Tabs.md) needs its own keyboard and focus behaviour even when it looks like a stack of accordion controls.

## Recommended behaviour

<span id="behaviour"></span>

Tab moves between controls and any interactive content in open panels in document order. Enter and Space operate the focused control. Keep keyboard focus on the control the person activates. Opening a panel makes it available visually, to assistive technology and to keyboard users. Closing it removes it from all three.

### Grouped panels

Prefer independent panels when people may compare information across sections. In a single-open group, opening another panel closes the previous one. Decide separately whether people may close the last open panel.

For an independent group with many items, consider offering ‘Expand all’ and ‘Collapse all’. Update the same panel visibility and control states as when people operate individual controls. Keep focus on the bulk action when someone activates it.

### Links to a section

If a URL fragment targets content within a collapsed panel, reveal the containing panel on direct entry and when the fragment changes. For an enhanced accordion that manages fragment navigation, let people continue reading at the target. When moving focus to a non-interactive target is needed, use `tabindex="-1"` so it stays out of the ordinary tab order. Preserve the normal tab stop of a target that is already interactive.

## Reference implementation

<span id="implementation"></span>

### Native HTML

Build on the [native disclosure example](Disclosure.md#native-html) when the browser behaviour meets the need. Use an ungrouped `details` element for each independent panel. A shared `name` makes the panels mutually exclusive in supporting browsers. Use this variant when people may close the last open panel. If the browser lacks support for grouping, people can still read and operate the individual disclosures.

This HTML example starts with both panels closed and needs no script or stylesheet. Remove the shared `name` attributes for independent panels.

```html
<section aria-labelledby="delivery-accordion-title">
  <h2 id="delivery-accordion-title">Ordering and delivery</h2>
  <details name="ordering-information">
    <summary>When will my order arrive?</summary>
    <p>Standard delivery takes three to five working days.</p>
  </details>
  <details name="ordering-information">
    <summary>Can I change my delivery address?</summary>
    <p>You can change your address before submitting the order.</p>
  </details>
</section>
```

### Custom HTML

Build on the [custom disclosure example](Disclosure.md#custom-html) when the pattern needs custom behaviour. Start with readable content and introduce working controls before hiding panels. For a section with a heading, put a native `button type="button"` inside the heading to keep both semantics. Give each button an `aria-controls` value matching its panel’s unique ID. Set `aria-expanded="true"` only while that panel is visible and use `hidden` on collapsed panels. A panel can be a named region when a landmark helps people navigate its contents. In that case, use `role="region"` and label it with `aria-labelledby` pointing to its button.

For a single-open group, update both the newly opened panel and the one that closes. If one panel must always stay open, put `aria-disabled="true"` on the open panel’s button and have the script ignore attempts to close that panel through its button. Keep the button in the tab order. Remove `aria-disabled` from the previously open panel’s button when another opens. If all panels can close, leave the open panel’s button operable. See the [ARIA Authoring Practices accordion pattern](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/).

## Testing Approach and ACT Scripts

The ACT-style checks in this section are pattern-specific checks maintained in this guide. The proposed W3C rules are listed separately.

<span id="checkpoints-for-the-tester"></span>

### ACT-style checks

Rule identifier: `gel-foundations-accordions`

Run these checks on a group of two or more items. Include a link or other interactive element in a test panel so you can check its tab order. For the custom-button variant, inspect the explicit ARIA attributes. For the native variant, check the browser’s exposed state. Use the project’s supported browsers, input devices and assistive technologies. Record the environment, result and any reason a check did not apply or was not run.

| Method and action | Expected result |
| --- | --- |
| Inspect the document structure. Find each control and its panel. | The group has at least two control–panel pairs. Each panel follows its control or enclosing heading in source order. |
| Tab to each header. Press Enter to open a closed panel and Space to close an open panel where closing is allowed. Repeat the allowed transitions with a mouse and touch. | Each input opens or closes the correct panel. During keyboard activation, focus stays on the header. |
| Inspect each header when open and closed, including in forced colours. Read the label and listen to its accessible name and state with a screen reader. | The control stays visible, looks interactive and has a label that identifies its panel. A visible marker communicates the state. The screen reader exposes the control, its name and state. |
| Inspect each custom button and panel before and after activation. | The button references the correct panel. `aria-expanded` is `true` when open and `false` when closed. Every affected button updates when the visible panel changes. |
| Start with a collapsed panel. Read past the control with a screen reader and Tab past it. Return to the control, open the panel and repeat. Close it again where allowed. | The closed content is invisible, absent from screen reader navigation and skipped by Tab. Once open, it follows its control in reading order and its interactive elements are reachable. After closing, the content leaves reading and focus order again. |

### Variant and fallback checks

1. Open two panels in sequence. In an independent group, both stay open. In a single-open group, the first closes. If the last panel may close, activate its header again and check that the group can have no open panels.
2. If one panel must stay open, Tab to its button and listen for the expanded and disabled states. Activate it and confirm that the panel stays open. Open another panel and verify that the disabled state moves to its button while both controls remain reachable.
3. If bulk controls are provided, activate Expand all and then Collapse all. Check every panel and control state after each action. Focus stays on the bulk control used.
4. Where the guide supports links into panels, open a direct URL targeting a collapsed panel, then follow an in-page link to a different collapsed panel. Check that the target becomes visible and that reading and keyboard navigation can continue from it.
5. Block JavaScript and reload. Check that native panels can be opened and that custom panels remain readable as baseline content. In a separate run, disable CSS with scripts enabled. Check that the labels and content remain readable, the controls still operate and collapsed panels stay hidden.
6. Increase text size and check that labels wrap and remain legible. In a separate check, narrow the viewport and confirm the labels, indicators and controls fit without overlap and the panel still follows its own control.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Summary element has non-empty accessible name (2t702h)](https://www.w3.org/WAI/standards-guidelines/act/rules/2t702h/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)

## Sources

- [ARIA Authoring Practices accordion pattern](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/)
- [Tabs in ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/#tabpanel)
- [Second rule of ARIA use](https://www.w3.org/TR/using-aria/#second)
- [MDN documentation for CSS `@supports`](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)
- [ARIA 1.0 definition of `aria-expanded`](https://www.w3.org/TR/wai-aria-1.0/states_and_properties#aria-expanded)
- [Discussion of `aria-controls` support](http://www.heydonworks.com/article/aria-controls-is-poop)
- [Inclusive Design Principles on giving users control](https://inclusivedesignprinciples.org/#give-control)
- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
