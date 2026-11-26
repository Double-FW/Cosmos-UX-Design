---
title: Badge
description: Present a short status, category or count alongside the content it describes.
---

# Badge

Use a badge to help people identify a status, category or count at a glance. Place it with the item it describes so the relationship also makes sense when someone reads the page with a screen reader. Use a link or button when the person needs to act, such as applying a [filter](Filter%20and%20Sort.md).

## Presentation

Use a short, visible label in sentence case, such as ‘On hold’, ‘New’ or ‘3 unread messages’. Keep the same wording and visual treatment for the same meaning across the service. Use badges selectively. Where a status affects what someone can do next, also state it in the relevant content or instructions.

Keep the meaning clear when colours are hard to distinguish or replaced. An icon or dot can reinforce a text label. For example, pair a dot with ‘Offline’. If an icon adds information, prefer including it in the visible text. Reserve icon-only badges for symbols users already recognise in context and provide the same meaning in words for assistive technology. See [Icons](Icons.md) and [colour guidance](Colour%2C%20Contrast%20and%20Meaning.md).

Give the full label room to wrap. Use relative text sizes and padding, and let the badge grow with its content. Let groups wrap without changing the reading order. Check default and inverse colour treatments on their intended surfaces. Keep informational badges visually distinct from controls, without a pointer cursor or hover and pressed styles.

## Behaviour

A badge is static information. Keep it out of the tab order. When a badge is part of a link or button, the parent control owns focus and activation. Give that control visible text that identifies its destination or action.

Keep the current value available on first load. When a change communicates a status message within the current view, announce it without moving focus. For a routine count update, use a polite status region and update a complete phrase such as ‘4 items in your basket’. Under WCAG 2.2 [4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html) (Level AA), an applicable status message must be available to assistive technology without receiving focus. See [Dynamic Content](Dynamic%20Content.md#status-messages).

Update counts when the underlying value changes. Avoid rolling-number animation and repeated announcements for intermediate values. If a badge disappears and the change matters, explain the new state through the page content or an appropriate status message. Keep important information available for people who need more time to read it.

## Implementation

### Static badges and groups

Use plain text in a `span`. Its contents are read as text in context. A generic `span` does not take an accessible name from `aria-label` or `aria-labelledby`. Hide decorative icons and dots from assistive technology. For an icon-only badge, use a visually hidden text equivalent or a named SVG with `role="img"`. Put that role on the SVG itself. Use [the visually hidden text technique](Anchors%20and%20Links.md#link-content) wherever these examples need extra wording.

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

Keep the badge next to the item it describes in source order. If a badge outside a link gives supplementary information that people need on focus, reference its unique ID using `aria-describedby`. Leave the badge available in reading order. If the text needs to appear in the accessible name, such as in a list of links, place it inside the link after the visible destination label. In that case the link already includes the badge text, so a second description is unnecessary.

### Count and status updates

Show what the number counts. If the visible context identifies the count, such as a ‘Basket’ link followed by ‘3’, use additional accessible text where needed to convey ‘3 items’. Keep the number in the control’s text alternative. When announcing a change, use a separate status region that already exists before the update. If the same change is announced elsewhere, use the existing status message.

This example shows a basket link with an initial count of three. It uses the shared visually hidden class linked above. The application calls the function after the confirmed count changes. Replace the sample route with the real basket destination.

```html
<a href="/basket">
  Basket <span id="basket-count">3</span><span id="basket-unit" class="visually-hidden"> items</span>
</a>
<p id="basket-update" role="status" class="visually-hidden"></p>

<script>
  function updateBasketCount(count) {
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

## Checkpoints for the tester

Use the project’s supported browsers and assistive technologies. Test each badge presentation in use and record the variant, environment and result.

| Method and action | Expected result |
| --- | --- |
| Read a card or list item with a screen reader. Where a badge accompanies a link or button, Tab to the control and inspect its name and description. | The badge meaning and the item it describes are clear. Static badges add no tab stop. Information inside a control’s name or description is available in the intended navigation mode. |
| Inspect a text-and-icon badge, an icon-only badge and a dot indicator where provided. Repeat in grayscale and with forced colours. | Text or a distinguishable symbol conveys the meaning without hue. Assistive technology gets an equivalent for meaningful icons and skips decorative graphics. |
| Navigate a group of badges in reading order. Repeat with any list markers suppressed by CSS. | The group retains its list semantics and follows the visible sequence. |
| Increase text size to 200%, apply the [text spacing overrides](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html), and test a long or translated label. In a separate check, narrow the viewport. | Labels wrap without clipping or overlap, groups stay readable and nearby controls remain usable. |
| Measure text and meaningful graphic contrast for the variants in use. Tab to any parent control in the default theme and forced colours. | Each applicable pair meets the thresholds in the colour guide. Keyboard focus remains visible on the control. |
| Load the count variant with a screen reader. Trigger updates across zero, one and multiple items, where supported. | The initial count is available without an unsolicited live announcement. Each relevant change is announced with its context, singular and plural wording agree with the count, and focus stays with the user. |
| Block scripts and reload a page with badges. | The initial labels and values remain readable. Any enhanced update has the fallback provided by the host feature. |

## Sources

- [Web Content Accessibility Guidelines 2.2](https://www.w3.org/TR/WCAG22/)
- [Understanding the accessibility criteria](https://www.w3.org/WAI/WCAG22/Understanding/)
- [ARIA role, state and property definitions](https://www.w3.org/TR/wai-aria-1.2/)
- [Accessible name and description computation](https://www.w3.org/TR/accname-1.2/)
- [ARIA authoring practices](https://www.w3.org/WAI/ARIA/apg/)
- [Live region guidance](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [Tag design and research](https://design-system.service.gov.uk/components/tag/)
- [Tag implementation guidance](https://designsystem.digital.gov/components/tag/)
- [Accessibility tree relationships](https://sarahmhigley.com/writing/roles-and-relationships/)
- [List styles and accessibility](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/list-style#accessibility)
