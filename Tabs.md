---
title: Tabs
description: "Switching between related panels."
---

# Tabs

Begin with in-page links to labelled sections. Enhance only when the complete tab interaction is available. Use one labelled tablist and one named tab for each panel. Give each panel the tabpanel role and label it from its tab. Keep panels after the tablist in source order. Distinguish focus from selection, including in forced colours.

## Implementation

Use fragment links as the baseline so that each panel has its own address. Add tab roles only after the keyboard handlers are available. Keep one tab in the ordinary tab sequence. Use Left and Right Arrow to move among horizontal tabs. In an explicit-activation design, Enter or Space selects the focused tab. Set `aria-controls` on each tab to the corresponding panel ID and `aria-labelledby` on the panel to the tab ID. When a fragment identifies content inside a hidden panel, reveal the panel before navigating to the target.

### Markup example

This baseline works before the tab controls are enhanced.

```html
<nav aria-label="Journey information">
  <a href="#arrival">Arrival</a>
  <a href="#departure">Departure</a>
</nav>
<section id="arrival"><h2>Arrival</h2><p>Use the north entrance.</p></section>
<section id="departure"><h2>Departure</h2><p>Leave through the east exit.</p></section>
```

## ACT-style checks

Rule identifier: `gel-components-tabs`

Run on an enhanced tab set with at least two panels. Check the baseline separately with enhancement blocked.

1. Activate each tab by keyboard, touch and pointer.
2. Exactly one tab should report `aria-selected="true"` and only its panel should be visible, readable and operable.
3. For the explicit-activation variant that moves focus into content, focus the selected panel programmatically with `tabindex="-1"`.
4. Check arrow-key navigation among tabs, direct fragment links, Back and Forward, and the baseline with scripts blocked.

## Related references

- [Tabs: ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices-1.1/#tabpanel)
- [Tabs, Used Right](https://www.nngroup.com/articles/tabs-used-right/)
- [Danger! ARIA Tabs](https://simplyaccessible.com/article/danger-aria-tabs/)
- [In-Page Links and Input Focus](http://accessibleculture.org/articles/2010/05/in-page-links/)
- [Removing Semantics Using The Presentation Role](https://www.accessibility-developer-guide.com/examples/sensible-aria-usage/presentation/)
- [Reading Commands and Cursors](https://doccenter.freedomscientific.com/doccenter/doccenter/rs11f929e9c511/2012-04-24_teachersandtrainers-l7/02_jawsandmagicreadingcommandsandcursors.htm)
- [`-ms-high-contrast` @media query](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/-ms-high-contrast)
- [WCAG2.1 2.4.3 Focus Order](https://www.w3.org/TR/WCAG21/#focus-order)
