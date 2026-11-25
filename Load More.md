---
title: Load More
description: "Reading additional results."
---

# Load More

Provide working pagination as the baseline. Mark the current page and omit unavailable previous or next actions. When enhancement is available, append results on request. Announce loading and identify the range of newly loaded results. Place a programmatically focusable marker before the new batch so readers can continue at the right place.

## Implementation

Keep a working URL for each underlying results page and identify it with `aria-current="page"` in the pagination. Hide the enhancement button until its handler is available. If loading fails, keep existing results and provide a retry or a working page link.

### Markup example

The host service must return the requested results page.

```html
<nav aria-label="Results pages">
  <a href="?page=1" aria-current="page">Page 1</a>
  <a href="?page=2">Page 2</a>
  <a href="?page=2">Next page</a>
</nav>
```

## Checkpoints for the tester

1. Load a second batch.
2. Confirm the first batch stays available, focus reaches the new batch marker and Tab moves into the new results.
3. At the end, communicate that there are no more items.
4. Block scripts and follow the pagination links.

## Related references

- [Automatic Infinite Scrolling And Accessibility](https://simplyaccessible.com/article/infinite-scrolling/)
- [So You Think You Built A Good Infinite Scroll](http://blog.adrianroselli.com/2014/05/so-you-think-you-built-good-infinite.html)
- [The Main Element](http://html5doctor.com/the-main-element/)
- [Separator (role)](https://www.w3.org/WAI/PF/aria/roles#separator)
- [ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
