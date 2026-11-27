---
title: "Badge"
description: "Present a short status, category or count alongside the content it describes."
---

# Badge

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a badge to help people identify a status, category or count at a glance. Place it with the item it describes so the relationship also makes sense when someone reads the page with a screen reader. Use a link or button when the person needs to act, such as applying a [filter](Filter%20and%20Sort.md).

## Recommended markup

Use visible text for the status, category or count and associate it with the relevant item. Use a link or button if the person can act on it.

## Recommended layout

<span id="presentation"></span>

Use a short, visible label in sentence case, such as ‘On hold’, ‘New’ or ‘3 unread messages’. Keep the same wording and visual treatment for the same meaning across the service. Use badges selectively. Where a status affects what someone can do next, also state it in the relevant content or instructions.

Keep the meaning clear when colours are hard to distinguish or replaced. Provide visible wording in the badge or immediately beside it for every status, category or count. For example, show ‘Offline’ beside a dot and ‘Verified’ beside a tick. A bare number can work when the surrounding visible wording makes clear what it counts. Icons and dots that repeat the visible meaning are decorative. See [Icons](Icons.md) and [colour guidance](Colour%2C%20Contrast%20and%20Meaning.md).

Give the full label room to wrap. Use relative text sizes and padding, and let the badge grow with its content. Let groups wrap without changing the reading order. Check default and inverse colour treatments on their intended surfaces. Keep informational badges visually distinct from controls, without a pointer cursor or hover and pressed styles.

## Recommended behaviour

<span id="behaviour"></span>

A badge is non-interactive. Its value can change. Keep it out of the tab order. When a badge is part of a link or button, the parent control owns focus and activation. Give that control visible text that identifies its destination or action.

Keep the current value readable on first load without a live announcement. When a change in the current view communicates an action outcome, waiting state, progress or error, make that status message available to assistive technology without moving focus, as required by WCAG 2.2 [4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html) (Level AA). For example, after someone adds an item, announce the confirmed count with a phrase such as ‘4 items in your basket’. A routine label change, such as a product gaining a ‘New’ badge, can be discovered in the reading order. See [Dynamic Content](Dynamic%20Content.md#status-messages).

Update the displayed count when the value is confirmed. If one action produces several intermediate values, announce the final result of that action. Avoid rolling-number animation. If a badge disappears and the change matters, make the new state clear in the page content and announce it when it is a status message. Keep important information available for people who need more time to read it.

## Reference implementation

<span id="implementation"></span>

### Static badges and groups

Use plain text in a `span`. Its contents are read as text in context. A generic `span` does not take an accessible name from `aria-label` or `aria-labelledby`. Hide decorative icons and dots from assistive technology with `aria-hidden="true"` and keep them out of the tab order. Keep the text that conveys the badge’s meaning available in the reading order.

The following fragment works without scripts. Style the badge and dot in the host design system. Use a list for a meaningful group of badges. If removing the markers also removes list semantics in a supported browser, retain those semantics with `role="list"`.

```html
<ul>
  <li>
    <span class="badge">
      <span class="badge-dot" aria-hidden="true"></span>
      Offline
    </span>
  </li>
  <li><span class="badge">Maintenance scheduled</span></li>
</ul>
```

### Associating a badge with a control

Keep the badge next to the item it describes in source order. If an adjacent badge provides information that people need when the control receives focus, reference the badge’s unique ID using `aria-describedby`. In this example, ‘Cotton shirt’ identifies the destination and ‘Backorder’ describes availability.

```html
<p>
  <a href="/products/cotton-shirt" aria-describedby="shirt-availability">
    Cotton shirt
  </a>
  <span id="shirt-availability" class="badge">Backorder</span>
</p>
```

When reading the page, users can find both the link and the stock status. On focus, the link has the name ‘Cotton shirt’ and the description ‘Backorder’. In a screen reader’s list of links, the link name still identifies the product.

Use the link’s name when the badge text belongs with the destination label, as in the basket count below. Put the badge after the visible destination text so the name starts with the wording people see. This helps people using voice control, as described in [2.5.3 Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html).

### Count and status updates

Show what the number counts. If the visible context identifies the count, such as a ‘Basket’ link followed by ‘3’, use [visually hidden text](Anchors%20and%20Links.md#link-content) where needed to convey ‘3 items’. The example link’s name is ‘Basket 3 items’, including in a screen reader’s list of links. When announcing a change, use a status region that already exists before the update. If an existing message already confirms the outcome, use that message for the announcement.

This example starts with three items. It uses the shared visually hidden class linked above. Call the function with a non-negative whole number after the application confirms the final count for an action. Replace the sample route with the real basket destination. In a translated interface, use the project’s localisation system to format the complete link wording and status message, including the number and plural form.

```html
<a href="/basket">
  Basket <span id="basket-count" class="badge">3</span><span id="basket-unit" class="visually-hidden"> items</span>
</a>
<p id="basket-update" role="status" aria-atomic="true" class="visually-hidden"></p>

<script>
  function updateBasketCount(count) {
    if (!Number.isSafeInteger(count) || count < 0) {
      return;
    }

    const countText = String(count);
    const unit = count === 1 ? 'item' : 'items';
    const visibleCount = document.getElementById('basket-count');

    if (visibleCount.textContent === countText) {
      return;
    }

    visibleCount.textContent = countText;
    document.getElementById('basket-unit').textContent = ' ' + unit;
    document.getElementById('basket-update').textContent =
      countText + ' ' + unit + ' in your basket';
  }
</script>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use the project’s supported browsers and assistive technologies. Test each badge presentation in use and record the variant, environment and result.

| Method and action | Expected result |
| --- | --- |
| Read a card or list item with a screen reader, Tab to its control and, for links, open the screen reader’s list of links. | The badge and its item are clear in reading order. The badge adds no tab stop. A supplementary description is available on focus. Text included in a link’s name follows its destination label and is available in the list of links. |
| Inspect badges with icons or dots. Repeat in greyscale and with forced colours. | Visible wording still conveys the meaning if the icon, dot or background changes or disappears. Assistive technology reads the text and skips decorative graphics. |
| Navigate a group of badges in reading order. Repeat with any list markers suppressed by CSS. | The group retains its list semantics and follows the visible sequence. |
| Test a long or translated label at 200% text size. In separate checks, apply the [text spacing overrides](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html) and test vertically scrolling content at [320 CSS pixels wide](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html). | Labels wrap without clipping or overlap, groups stay readable and nearby controls remain usable. Badge content and controls fit at 320 CSS pixels without introducing horizontal scrolling. |
| Measure text and any meaningful graphic contrast for the variants and surfaces in use, including inverse treatments. Tab to any parent control in each supported theme and in forced colours. | Each applicable pair meets the thresholds in the colour guide. Keyboard focus remains visible even if the badge background disappears. |
| Load the count variant with a screen reader. Trigger updates across zero, one and multiple items, where supported. Repeat with rapid updates and with an existing confirmation for the same action. | The initial count is readable without an unsolicited announcement. Wording agrees with the count. Each meaningful action outcome is conveyed with context and without duplicate or intermediate announcements. Focus stays with the user. |
| Trigger a routine category change and, where supported, remove a badge that indicated a change in state. | The changed category is available in the reading order. If a removal matters, the new state is clear in the page content and is announced when it is a status message. |
| Block scripts and reload a page with badges. | The initial labels and values remain readable. Any enhanced update has the fallback provided by the host feature. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)

## Sources

- [Web Content Accessibility Guidelines 2.2](https://www.w3.org/TR/WCAG22/)
- [W3C guide to understanding WCAG 2.2](https://www.w3.org/WAI/WCAG22/Understanding/)
- [ARIA role, state and property definitions](https://www.w3.org/TR/wai-aria-1.2/)
- [Accessible name and description computation](https://www.w3.org/TR/accname-1.2/)
- [ARIA authoring practices](https://www.w3.org/WAI/ARIA/apg/)
- [W3C technique for announcing status messages](https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA22)
- [MDN guidance on ARIA live regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [Tag design and research](https://design-system.service.gov.uk/components/tag/)
- [Tag implementation guidance](https://designsystem.digital.gov/components/tag/)
- [Accessibility tree relationships](https://sarahmhigley.com/writing/roles-and-relationships/)
- [List styles and accessibility](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/list-style#accessibility)
- [4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)
- [2.5.3 Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html)
- [text spacing overrides](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html)
- [320 CSS pixels wide](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
