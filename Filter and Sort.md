---
title: Filter and Sort
description: Help people narrow a set of results and choose the order they appear in.
---

# Filter and Sort

Use filters to help people narrow results to what matters to them. Use sorting to change the order of those results. Show the current choices and make it clear which results they affect, including when some choices move behind an overflow control.

## Presentation

Use distinct labels such as ‘Filter by content type’ and ‘Sort by’. Keep the selected filter and sort order identifiable without colour. For example, pair a visual treatment with a ‘Current’ marker. If an active filter moves into the overflow list, show its name outside the collapsed list too.

Let choices wrap where there is space. If the baseline uses a horizontal strip, let it scroll and make the overflow apparent. Check that keyboard users can reach every link and that focused links scroll into view. Keep the visible focus indicator and current selection distinguishable in forced colours.

## Behaviour

### Choosing and applying options

Use destination links when each choice opens a results URL. Keep the current search query when people change filters. Keep the current sort when they change a filter and retain the selected filters when they change the sort. Mark the current link within each independent list using `aria-current="page"` when it represents the current results page.

Use labelled checkboxes for multiple selections or radios for a short list of mutually exclusive options in a form. Use an explicit Apply action when a choice would navigate or otherwise change context. Under [WCAG 2.1 3.2.2 On Input](https://www.w3.org/TR/WCAG21/#on-input) (Level A), changing a control setting must not automatically change context unless people have been told about that behaviour before using the control.

Show the updated results, including an explanation when no items match and a way to revise or clear the filters. If results update in place, keep the initiating control usable and announce a concise result count or status without taking focus. See [Dynamic Content](Dynamic%20Content.md#status-messages). If the change opens a new view in a single-page application, follow [Client-side Routing](Client-side%20Routing.md). Preserve the query, filters and sort when people request another page or batch of the same results. See [Load More](Load%20More.md).

### Revealing overflow choices

Use [Disclosure](Disclosure.md) for a control labelled ‘More filters’ or similarly specific wording. Keep focus on the button when it opens. Put the overflow links immediately after it so the first revealed link is next in the Tab sequence. When the list closes, its links leave keyboard and screen reader navigation. If dismissal starts from inside the panel, return focus to the trigger before hiding it.

## Implementation

### Markup example

This fragment shows the results page for the selected ‘Articles’ filter with ‘Relevance’ sorting. The host service supplies the sample results routes, recognises the query parameters and renders the current states. The links work without an enhancement script.

```html
<nav aria-labelledby="filter-label">
  <p id="filter-label">Filter by content type</p>
  <ul>
    <li><a href="/search?q=walking&amp;sort=relevance">All types</a></li>
    <li>
      <a href="/search?q=walking&amp;type=article&amp;sort=relevance"
         aria-current="page">Articles</a>
    </li>
    <li><a href="/search?q=walking&amp;type=video&amp;sort=relevance">Videos</a></li>
  </ul>
</nav>

<nav aria-labelledby="sort-label">
  <p id="sort-label">Sort by</p>
  <ul>
    <li>
      <a href="/search?q=walking&amp;type=article&amp;sort=relevance"
         aria-current="page">Relevance</a>
    </li>
    <li><a href="/search?q=walking&amp;type=article&amp;sort=newest">Newest first</a></li>
  </ul>
</nav>
```

### Enhancing a narrow layout

Start from the complete list of working links. Keep the disclosure button hidden until the panel and its controls work. Measure which choices fit, allowing space for the button itself. Move complete list items into the overflow list in their original order. If the implementation uses copies, expose no more than one copy of each choice to keyboard or screen reader users at a time, and keep its destination and current state in sync.

Recheck the layout at narrow widths and after text resizing. When a choice has focus, keep it visible and operable while adjusting the layout. If a resize would put it behind a collapsed panel, defer that move until focus leaves the choice. Keep the wrapping or scrollable baseline if the enhancement cannot run.

## Checkpoints for the tester

Test the link or form variant in use with the supported browsers, keyboard and screen readers.

| Method and action | Expected result |
| --- | --- |
| Start from a results URL with a query. Apply a filter, change the sort order and then change the filter again. | Only the intended criteria change. The results, current indicators and URL agree for navigation-based controls. The other choices and search query remain in effect. |
| In a form, change a radio or checkbox with the keyboard, then apply the choices. | Labels and selection states are available. Where applying opens a new view, the explicit Apply action initiates navigation. |
| At a narrow width, Tab to More filters and open it. Tab through the revealed links, then close the list. | Focus stays on the trigger when it opens. The first revealed link follows it. Each visible choice is reachable once. Hidden links are skipped after closing. |
| Increase text size, then narrow the viewport while a filter link has focus. Check the current selection and repeat with forced colours. | The focused link stays usable, the current choice remains identifiable and the overflow control stays visible. |
| Block scripts and reload, then apply a filter and sort. | Every choice in the baseline can still be reached and applied. A horizontal baseline brings focused links into view. |
| Apply a filter that produces no matches. Where results update in place, repeat with a screen reader and a slow response. | The results state is clear, any waiting message is available where needed, and the person can revise or clear the choices. In-place updates leave focus usable. |

## Sources

- [ARIA landmark example](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/navigation.html)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
- [CSS Flexible Box Layout Module](https://caniuse.com/#feat=flexbox)
- [Perceived Affordances](http://johnnyholland.org/2010/04/perceived-affordances-and-designing-for-task-flow/)
- [`IntersectionObserver` API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [Managing focus for accessibility (video)](https://dev.to/robdodson/managing-focus-64l)
- [Discussion of `aria-controls` support](https://www.heydonworks.com/article/aria-controls-is-poop)
- [WCAG2.1 3.2.2 On Input](https://www.w3.org/TR/WCAG21/#on-input)
