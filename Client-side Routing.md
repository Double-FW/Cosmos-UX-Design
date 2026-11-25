---
title: Client-side Routing
description: Help people move between views and identify where they have arrived.
---

# Client-side Routing

When someone follows a link in a single-page application, the router can replace the current view without loading a new document. If the old view disappears while focus is inside it, a keyboard user can lose their place. If the new view appears without an announcement, someone using a screen reader may miss the change. Give each route a real destination and manage its title, current navigation state and focus together.

## Presentation

Keep shared navigation in a consistent place and render the content for the active screen in [the main landmark](Structure.md#the-main-landmark). Place a skip link before repeated navigation so it is the first interactive element. Give each view a descriptive `h1`. Keep inactive views out of the reading and tab order if the router retains them in the document.

Give each screen a unique document title that describes its purpose. Include the main heading’s wording in the title where it fits, followed by the service name. See [Document title](Structure.md#document-title). Show which link matches the current location and expose the same information using `aria-current`. Use `page` for the current page, `step` for the current step or another value appropriate to the relationship. Clear the previous current state when the route changes.

## Behaviour

### First load and route changes

On direct entry or reload, leave initial focus to the browser so people can start at the top and use the skip link. When someone follows a link to a different view within the application, update the document title and current navigation item for the new view. Once the new main content is in the document, focus its `h1`. The focused heading introduces the view to screen reader users and sets the starting point for the next Tab.

For an update within the same view, such as filtering a list of results, follow [Dynamic Content](Dynamic%20Content.md#maintaining-focus). Moving focus to the main heading on every update can interrupt someone who is still using a control.

### Awaiting content

Consider preloading critical resources where it helps people reach the next view sooner. When a user has to wait, show a text message such as ‘Loading booking details’ and announce it using the [status message guidance](Dynamic%20Content.md#status-messages). A decorative spinner can accompany the message. Keep the status container available before changing its text so assistive technology can pick up the update.

If loading fails, show an understandable error and provide a recovery action. Avoid moving focus to a heading from an earlier request if the user has since chosen a different route. Update the title, navigation and focus for the view that actually renders.

### Back and Forward

Preserve browser history for meaningful route changes. Back and Forward should restore the expected URL, content, document title and current navigation item. When returning to a view where the user was browsing a list, restore their previous location if its control is still available and doing so makes it easier to resume. Otherwise use the restored view’s main heading. If the interface offers its own Back action, test it alongside the browser controls. Give it a clear destination when the user may have arrived directly.

## Implementation

### Markup example

This fragment shows the rendered view for the sample route `/bookings`. It illustrates the document title, skip target and current link. The application supplies the real routes and arranges for each supported URL to open directly.

```html
<head>
  <title>Your bookings | Travel planner</title>
</head>
<body>
  <a href="#main">Skip to main content</a>
  <nav aria-label="Main">
    <a href="/destinations">Destinations</a>
    <a href="/bookings" aria-current="page">Your bookings</a>
  </nav>
  <main id="main" tabindex="-1">
    <h1 tabindex="-1">Your bookings</h1>
    <a href="/bookings/upcoming">View upcoming bookings</a>
  </main>
</body>
```

### Focus after rendering

Use the router’s completed navigation or render callback. Pass the newly rendered heading from the active view, for example through a component reference. The function assumes that the application has already updated the current link. Call it after an in-app route change that needs heading focus.

```js
function introduceRoute(heading, documentTitle) {
  document.title = documentTitle;
  heading.focus();
}
```

The example heading has `tabindex="-1"` so the script can focus it without adding it to the ordinary Tab sequence. Keep form controls and links in their natural tab order.

## Checkpoints for the Tester

### ACT-style checks

Rule identifier: `gel-components-routing`

Test at least two routes that share navigation. Start on a direct URL and then switch views using the keyboard. Use the supported browser and screen reader combinations to check announcements.

| Method and action | Expected result |
| --- | --- |
| Open each route directly and compare the browser tab title with the main heading. Repeat after navigating within the application. | Each screen has a unique, descriptive title. It reflects the displayed view on entry and after navigation. |
| Reload a route without a fragment or other explicit focus target and press Tab from the start of the page. Activate the skip link. | The router leaves initial focus to the browser. The skip link is the first interactive element, precedes shared navigation and takes the user to the main content. |
| Follow another route. Inspect the shared navigation and read its links with a screen reader. | The current link is visually identifiable and the appropriate `aria-current` value is exposed. The previous link clears its current state. |
| Activate a route link with the keyboard. Wait for the new view, listen to the screen reader and press Tab. | Focus lands on the newly rendered `h1`. The screen reader announces the heading and the next Tab continues from the new main content. |

### History and loading checks

1. Move between two views and use browser Back and Forward. Confirm that each transition restores the correct URL, content, title and current link. On return, check that focus reaches the previous usable position or the restored main heading. Repeat with an in-app Back action if provided.
2. Delay a route response. Check that the loading message is visible and announced. When the route finishes, check that the new heading receives focus. Repeat with a failed response and verify that the explanation and recovery action are usable.
3. Start a slow route and choose a different one before it finishes. Verify that a late response cannot replace the newer view, change its title or take focus away.

## Related references


- [Landmark navigation examples](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/at.html#jaws)
- [WCAG2.1 2.4.1 Bypass Blocks](https://www.w3.org/TR/WCAG21/#bypass-blocks)
- [Information Wayfinding Part 1](https://www.uxmatters.com/mt/archives/2013/04/information-wayfinding-part-1-a-not-so-new-metaphor.php)
- [Managing focus for accessibility (video)](https://dev.to/robdodson/managing-focus-64l)
- [Refs and The DOM](https://reactjs.org/docs/refs-and-the-dom.html)
- [Focus should cycle from named anchor](https://bugs.chromium.org/p/chromium/issues/detail?id=262171)
- [PRPL Pattern](https://www.gatsbyjs.org/docs/prpl-pattern/)
- [Fragment identifier](https://en.wikipedia.org/wiki/Fragment_identifier)
