---
title: Client-side Routing
description: "Moving to a new view."
---

# Client-side Routing

Give each route a real destination, a unique page title and a descriptive primary heading. Mark the current link or step. Preserve native initial focus on first load. Once a user-initiated route finishes rendering, focus the new heading using `tabindex="-1"`. Announce a wait when content takes time to load.

## ACT-style checks

Rule identifier: `gel-components-routing`

Start at a direct URL, then navigate within the application. Inspect the title, current link and focus after each transition.

1. Follow a route with the keyboard, then use Back and Forward.
2. Verify the URL, title, current navigation item and content agree at each step.
3. Confirm the new heading receives focus on forward navigation and the previous context is usable on return.
4. Test the [skip link](Structure.md#the-main-landmark) before repeated navigation.

### Testing a delayed route

Open a direct URL and check that the browser keeps its usual initial focus. Then follow an internal route that takes time to render. Check that a screen reader user receives a text loading message while waiting. When the content is ready, focus its `<h1 tabindex="-1">`. Press Tab and check that navigation continues from the new content.

Inspect the shared navigation. Use `aria-current="page"` on the current page link or `aria-current="step"` for the current step in a sequence. Follow another route and use Back. At each point, verify that the previous current state clears and the current item matches the displayed screen. From the start of the page, check that the first interactive element is the skip link and that activating it bypasses the repeated navigation.

## Related references

- [Landmarks examples, JAWS screen reader for Windows](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/at.html#jaws)
- [WCAG2.1 2.4.1 Bypass Blocks](https://www.w3.org/TR/WCAG21/#bypass-blocks)
- [Information Wayfinding Part 1](https://www.uxmatters.com/mt/archives/2013/04/information-wayfinding-part-1-a-not-so-new-metaphor.php)
- [Managing focus for accessibility (video)](https://dev.to/robdodson/managing-focus-64l)
- [Refs and The DOM](https://reactjs.org/docs/refs-and-the-dom.html)
- [Focus should cycle from named anchor](https://bugs.chromium.org/p/chromium/issues/detail?id=262171)
- [PRPL Pattern](https://www.gatsbyjs.org/docs/prpl-pattern/)
- [Fragment identifier](https://en.wikipedia.org/wiki/Fragment_identifier)
