---
title: Search
description: Help people find content through site-wide or collection search.
---

# Search

Help people find a page or explore a set of results by entering a query. State what the search covers before someone types. Keep the form usable without suggestions so people can still search when suggestions are unavailable.

This guide covers query entry and linked suggestions. Use [Filter and Sort](Filter%20and%20Sort.md) to help people refine the results from a submitted query.

## Presentation

Use consistent controls for site-wide and local search. Name the scope in the visible label, such as ‘Search this site’ or ‘Search the help centre’. When a page has both searches, give their search landmarks distinct names so someone using a screen reader can find the right form. See [Landmarks](Landmarks.md).

Place suggestions below the search controls in the visual layout and document order. Use a list of links with descriptive names. Where you offer several kinds of suggestion, give each list a heading that identifies what it contains. Keep the links and headings readable at narrow widths and enlarged text sizes. Let the form controls wrap as space runs out and keep the [keyboard focus indicator](Focus.md#focus-indication) visible.

## Behaviour

### Open and close search

A local search can stay visible within the collection it searches. If you offer site-wide search from a header, provide a working link to a search page or an inline form as the baseline. Use a native button to open an inline panel once it works. On opening, reveal the panel and move focus to the search field. Provide a clearly labelled close button. On dismissal, hide the panel and return focus to its opener. Keep hidden controls and suggestions out of keyboard navigation and the accessibility tree. See [Global Navigation](Global%20Navigation.md#baseline-navigation) and [Progressive Enhancement](Progressive%20Enhancement.md).

### Use suggestions or submit a query

- Submitting the form sends the typed query to results within the stated scope. Activating a suggestion link opens its destination. Make both paths available while suggestions are showing.
- Keep focus in the field as the person types. Let people reach suggestion links in the ordinary Tab order and activate them with Enter. Pressing Enter from the field submits the query for this linked-suggestions pattern.
- Announce the updated count with a polite status message once the suggestions are ready, such as ‘3 suggestions available’. Keep the message separate from the links. Update it when the set for the current query is ready and avoid announcements during each intermediate update.
- When suggestions are empty, unavailable or the query is cleared, remove stale links. If it would help, explain that people can still submit the query. If a refresh would remove a focused suggestion, keep it available until focus leaves or return focus to the field before removing it. Ensure a response for an older query cannot replace the current suggestions.
- Prefer a simple reveal. If you animate the suggestions, honour [reduced motion preferences](Motion%20and%20Animation.md#reduced-motion-preferences).

## Implementation

### Search field labels

Prefer a persistent visible text label. In a compact single-field search, a visible ‘Search’ button can provide visual context if it clearly belongs to the field. In that case, give the field its own associated label, visually hidden only where the context is clear. Keep a visible field label if the submit button is icon-only. Give the icon-only button an accessible name and hide its decorative icon from assistive technology. Keep essential query instructions visible as people type. Use placeholder text only as a supplementary example.

Use `type="search"` for the query field. Use `enterkeyhint="search"` on the field to suggest what the Enter key says on an on-screen keyboard. Keep an explicit submit button available. For an inline panel, use a separate `type="button"` control to close it.

### Markup example

This always-visible local search submits without JavaScript. Replace the example action with the service’s search endpoint. Give each form and label pair its own IDs where needed. In this example the search landmark wraps the form so it can also contain the suggestions.

```html
<div role="search" aria-label="Help centre">
  <form action="/help/search" method="get">
    <label for="help-query">Search the help centre</label>
    <input id="help-query" name="q" type="search" enterkeyhint="search">
    <button type="submit">Search</button>
  </form>

  <p id="help-search-status" role="status"></p>

  <section aria-labelledby="help-suggestions-heading" hidden>
    <h2 id="help-suggestions-heading">Suggested help pages</h2>
    <ul id="help-suggestion-links"></ul>
  </section>
</div>
```

The markup starts with an empty status message and hidden list. To enhance it, populate the list with working links for the current query, remove `hidden` from the section and update the existing status text with the count. When hiding the section, clear or update the status text to match. Keep the status element in the document and outside the hidden section so it is available before the update. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

When suggestions fill in a query value, choose a pattern for that interaction, such as [Combobox](https://www.w3.org/TR/wai-aria-practices/#combobox). Use ordinary links for suggestions that lead directly to pages.

## Checkpoints for the tester

Run the checks that match the implementation. For suggestions, use a query with known destinations and a query that produces none. Record the browser, device and assistive technology used where relevant.

| Method and action | Expected result |
| --- | --- |
| Start with site-wide search closed. Tab to its opener and activate it. Tab to Close and activate it. | Focus enters the search field. Closing returns focus to the opener without submitting. Hidden controls and links are skipped. |
| Open the screen reader’s landmark list on a page with both site-wide and local search. Navigate to each landmark and inspect the field and buttons. | The two search scopes are distinct. The field and each button have names that identify their purpose. The names agree with any visible text. |
| With a screen reader running, type a query that returns suggestions. Wait for the update, then use Tab to reach a suggestion and Enter to open it. | The count is announced without moving focus from the input. Suggestions are grouped and named. The link opens the expected destination. |
| Repeat the query with suggestions visible. Press Enter from the field. Repeat using the submit button. | Both routes submit the typed query and display results in the stated scope. |
| Clear the query, enter a query with no suggestions, then simulate a delayed or failed response and type a different query. | Old suggestions become unavailable, late responses do not replace current suggestions and the form still submits. |
| Block scripts and reload. Follow the header search link if present, then submit a query through the baseline form. | Search remains findable and the results match the query and scope. |
| On a touchscreen, enter a query, tap a suggestion, then return and submit with the button. | Both paths work by touch and submission works even when the on-screen keyboard ignores the hint. |
| Increase text size, then separately narrow the viewport. Use the field, buttons and suggestions by keyboard in each condition. | The controls and suggestions remain readable and operable. Focus stays visible as controls wrap. |
| Enable reduced motion if the suggestions are animated. Enter a query with suggestions and then clear it. | Suggestions appear and disappear without the non-essential motion. |

## Related references

- [Where To Put Your Search Role](http://adrianroselli.com/2015/08/where-to-put-your-search-role.html)
- [When To Use An ARIA Role](https://developer.paciellogroup.com/blog/2012/06/html5-accessibility-chops-when-to-use-an-aria-role/)
- [Why it's important to give your HTML button a type](https://dev.to/claireparkerjones/why-its-important-to-give-your-html-button-a-type-58k9)
- [`<datalist>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)
- [Combobox - ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices/#combobox)
