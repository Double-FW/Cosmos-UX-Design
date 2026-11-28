---
title: "Load More"
description: "Help people request another batch of results and continue from where they stopped."
---

# Load More

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use Load More when people are exploring a list and want to request more results without leaving the page. Automatic loading can move the reading position or make it hard to reach the content after the results. Give people a button to request each batch and provide working pagination as the baseline.

Use [Expandable Content Previews](Expandable%20Content%20Previews.md) when the button reveals content already on the page.

## Recommended markup

Use links for pagination and a button to request another batch in place. Keep result groups in source order.

## Recommended layout

<span id="presentation"></span>

Put the initial results in the main content. Use list items for results and use consistent heading levels when results have headings. Follow [Headings](Headings.md) for choosing their level. Make each result and its link distinguishable from its neighbours.

Place the Load more button and loading feedback after the results. Make each new batch recognisable. For example, place a visible ‘Results 11 to 20’ heading immediately before the new list. Keep the range marker outside the result items so that a list’s item count reflects the results it contains. Give the marker a visible focus indicator when it receives focus. See [Focus](Focus.md#focus-indication).

## Recommended behaviour

<span id="behaviour"></span>

### Pagination

Give each results page a working URL. Retain the current query, filters and sort order across pages. When a query or filter changes the result set, restart pagination for that selection. See [Filter and Sort](Filter%20and%20Sort.md).

Use a named navigation landmark for pagination. Mark the current page with `aria-current="page"` and distinguish it visually from both other pages and the keyboard focus indicator. Omit unavailable Previous and Next links. Where the full set of page links does not fit, retain Previous and Next where available and show the current page and total when known. Include an early [skip link](Page_Structure.md#the-main-landmark) so people can reach the main content after a full page load.

### Loading another batch

1. When the person activates Load more, keep their current results available and show a text status such as ‘Loading more results’. Announce it through a [status region](Dynamic%20Content.md#status-messages) that exists before the update. A spinner can reinforce the text. Keep the feedback available with reduced motion.
2. Prevent duplicate requests for the same batch without moving focus. Keep the button’s place in the layout while the request is pending. If the person changes the query, filters or sort while waiting, discard any response that belongs to the previous selection.
3. On success, append the new results in order and update the next destination. If focus is still on the initiating button after keyboard activation, move it to the new batch marker. Reading forward should reach the new results, and the next Tab should reach their first focusable item or the next available control. If the person has moved on, preserve their new focus and announce the result range through the status region. Use one announcement route for the same message.
4. On failure, leave previous results and their order intact. Explain that the next batch could not load and let people retry or follow a working pagination link. A failed request should leave the next page available.
5. When there are no more results, say so and remove the unavailable action. If the button still has focus and there is no new batch marker, move focus to the end-of-results message before removing the button. Keep the rest of the page reachable.

## Reference implementation

<span id="implementation"></span>

### Markup example

This fragment shows the first results page. The server renders the initial results, the correct current-page marker and the pagination URLs. Replace the sample routes and query parameters with working ones.

```html
<ul id="results">
  <li><a href="/routes/riverside">Riverside walk</a></li>
  <li><a href="/routes/woodland">Woodland walk</a></li>
</ul>

<p id="results-status" role="status"></p>
<button type="button" id="load-results" hidden>Load more results</button>

<nav aria-label="Results pages" id="results-pagination">
  <a href="/routes?q=walking&amp;page=1" aria-current="page">Page 1</a>
  <a href="/routes?q=walking&amp;page=2">Page 2</a>
  <a href="/routes?q=walking&amp;page=2">Next page</a>
</nav>
```

Reveal the enhancement button only after the request and response handlers are ready. If the enhancement replaces the pagination controls, retain a working route to the next page for recovery. Keep the status region available to assistive technology before writing to it.

Insert the new batch immediately before the status and controls. For example, the next batch can use this structure. Use the actual range and choose a heading level that fits the results section.

```html
<h2 tabindex="-1">Results 3 to 4</h2>
<ul>
  <li><a href="/routes/park">Park loop</a></li>
  <li><a href="/routes/hill">Hill walk</a></li>
</ul>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Test with the supported browsers and screen readers. Include enough results to load a second batch and then reach the end.

| Method and action | Expected result |
| --- | --- |
| Block scripts and follow the pagination links with the keyboard. Check the first, intermediate and final page where available. | The URLs load the expected results and retain the query, filters and sort. The current page is identifiable. Unavailable links are absent and the skip link can reach the main content. |
| Activate Load more with the keyboard. Delay the response and try activating it again. | The loading message is visible and available to assistive technology. The first batch remains usable and only one request appends the next batch. |
| Let the request complete, read forward from the marker and press Tab. Repeat by moving focus away from the button while a request is pending. | The new results follow the earlier ones. Focus moves to the range marker when the keyboard user is still waiting on the button. If focus has moved, it stays at its new destination and the range is announced separately. |
| Force a failed response, then retry or follow the fallback link. If filters or sort are available, change them while a request is pending. | Earlier results remain available after failure. Retry loads the intended batch once. An earlier response cannot add results to a newer selection. |
| Load the final batch. Repeat with a response that returns no further items. | The interface communicates that there are no more results. Focus remains usable and the person can reach content after the results. |
| Narrow the viewport and repeat at increased zoom. Test loading with reduced motion enabled. | Pagination and feedback remain usable. Visible text communicates progress even when animation is reduced. |

### Supplied QA scripts

- [Paged Navigation Test Criteria](qa/supplied/paged-navigation.md) — Apply only to a separate pagination fallback, where provided.
- [Progress Indicator Test Criteria](qa/supplied/progress-indicator.md) — Apply only if the loading UI exposes a progress indicator.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Link has non-empty accessible name (c487ae)](qa/w3c-act/c487ae.md)

## Sources

- [Automatic Infinite Scrolling And Accessibility](https://simplyaccessible.com/article/infinite-scrolling/)
- [So You Think You Built A Good Infinite Scroll](http://blog.adrianroselli.com/2014/05/so-you-think-you-built-good-infinite.html)
- [The Main Element](http://html5doctor.com/the-main-element/)
- [Separator (role)](https://www.w3.org/WAI/PF/aria/roles#separator)
- [ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
