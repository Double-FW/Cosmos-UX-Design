---
title: Accordion
description: A pattern for hiding and showing related content.
---

Use an accordion for related sections when making them all visible at once would make the page harder to scan. Keep essential instructions visible if users need them to complete a task.

## Presentation

Give each section a descriptive title and a visible expanded or collapsed indicator. Put the panel immediately after its control in source order. Use heading levels that fit the surrounding page.

## Implementation

### Native HTML

Build on the [native disclosure example](Cards%20-%20Reveal.md#native-html) for sections that can open independently. A shared `name` on the `details` elements can make their panels mutually exclusive in supporting browsers. If grouping is unsupported, users can still open and read sections independently. Use this option when the last open section can be closed.

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

Use the [custom disclosure behaviour](Cards%20-%20Reveal.md#custom-html) as the baseline. Place each button inside a heading at the appropriate level. During enhancement, close the previous panel when opening another if only one may be open.

If one section must always remain open, set `aria-disabled="true"` on the expanded header button and prevent it from closing its own panel. Keep that button in the tab order and remove the disabled state when another panel opens. If all panels can be closed, the expanded header remains operable. Follow the [accordion pattern](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/) for semantics and keyboard interaction.

## Checkpoints for the Tester

1. Tab to each title. Use Enter and Space to operate the controls and check that the expanded or collapsed state is visible and announced by the screen reader.
2. Check that closed content is absent visually and from screen reader navigation, and that opening a panel reveals it next in reading order.
3. For a single-open version, open another panel and check that the previous one closes. If one panel must always stay open, check that the active header is still reachable by Tab, is announced as disabled and cannot close its own panel.
4. With scripts blocked, verify that all sections can be read. With CSS disabled in a separate test, check that the controls and panels still work.

## Grouped panels

Use the shared [reveal behaviour](Cards%20-%20Reveal.md#custom-html) for each section. In a group, keep every header visible and identifiable when neighbouring panels change state.

### ACT-style checks

Rule identifier: `gel-foundations-accordions`

Run on a group of two or more panels. Apply explicit ARIA attribute checks to the custom-button variant. Native disclosures expose their state through browser semantics.

1. With two or more sections, operate each header by touch, pointer and keyboard.
2. Check that focus stays on the activated header.
3. Inspect the custom button’s expanded state after each change and verify that the next content in reading order belongs to that header.
