---
title: Filter and Sort
description: "Choosing which results to show."
---

# Filter and Sort

Use labelled controls or links for the available filters and sort orders. Identify the current selection. At narrow widths, put overflow choices behind a disclosure and keep them reachable when scripts fail. Place revealed choices directly after their control. Explain the scope of the filter and where to find updated results.

## Implementation

For navigation-based filters, use real links with destinations that retain the current sort. Expose the current filter and sort in their respective lists. When moving choices into an overflow panel, keep each destination available exactly once to keyboard and screen reader users.

## Checkpoints for the tester

1. Choose a filter and sort order, then inspect the selected state and results.
2. At a narrow width open the overflow choices and press Tab.
3. The first newly available choice should be reachable.
4. Close the panel and verify hidden choices leave the focus order.

## Related references

- [ARIA landmark example](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/navigation.html)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
- [CSS Flexible Box Layout Module](https://caniuse.com/#feat=flexbox)
- [Perceived Affordances](http://johnnyholland.org/2010/04/perceived-affordances-and-designing-for-task-flow/)
- [`IntersectionObserver` API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [Managing focus for accessibility (video)](https://dev.to/robdodson/managing-focus-64l)
- [ARIA-controls is Poop](https://www.heydonworks.com/article/aria-controls-is-poop)
- [WCAG2.1 3.2.2 On Input](https://www.w3.org/TR/WCAG21/#on-input)
