---
title: "Search"
description: "Help people find content through site-wide or collection search."
---

# Search

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people find a page or explore a set of results by entering a query. State what the search covers before someone types. Keep the form usable without suggestions so people can still search when suggestions are unavailable.

This guide covers query entry and linked suggestions. Use [Page Structure](Page_Structure.md#search) for the search landmark in the page outline, [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md) for cross-cutting semantic choices, and [Visually Hidden Content](Visually%20Hidden%20Content.md) when considering hidden field labels or suggestion states. Use [Filter and Sort](Filter%20and%20Sort.md) to help people refine the results from a submitted query.

## Recommended markup

Use the native `<search>` element for a search or filtering interface; it provides search landmark semantics without a redundant `role="search"`. Give the landmark a clear accessible name when its purpose is not clear from its heading or context, and give the query field its own associated label. Use an explicit submit button. Use `role="search"` on another suitable container only when the native element is unavailable in the host environment.

## Recommended layout

<span id="presentation"></span>

Use consistent controls for site-wide and local search. Name the scope in the visible label, such as ‘Search this site’ or ‘Search the help centre’. When a page has both searches, give their search landmarks distinct names so someone using a screen reader can find the right form. See [Landmarks](Landmarks.md).

Place suggestions below the search controls in the visual layout and document order. Use a list of links with descriptive names. Where you offer several kinds of suggestion, give each list a heading that identifies what it contains. Keep the links and headings readable at narrow widths and enlarged text sizes. Let the form controls wrap as space runs out and keep the [keyboard focus indicator](Focus.md#focus-indication) visible.

## Recommended behaviour

<span id="behaviour"></span>

### Open and close search

A local search can stay visible within the collection it searches. If you offer site-wide search from a header, provide a working link to a search page or an inline form as the baseline. Use a native button to open an inline panel once it works. On opening, reveal the panel and move focus to the search field. Provide a clearly labelled close button. On dismissal, hide the panel and return focus to its opener. Keep hidden controls and suggestions out of keyboard navigation and the accessibility tree. See [Global Navigation](Global%20Navigation.md#baseline-navigation) and [Progressive Enhancement](Progressive%20Enhancement.md).

### Use suggestions or submit a query

- Submitting the form sends the typed query to results within the stated scope. Activating a suggestion link opens its destination. Make both paths available while suggestions are showing.
- Keep focus in the field as the person types. Let people reach suggestion links in the ordinary Tab order and activate them with Enter. Pressing Enter from the field submits the query for this linked-suggestions pattern.
- Announce the updated count with a polite status message once the suggestions are ready, such as ‘3 suggestions available’. Keep the message separate from the links. Update it when the set for the current query is ready and avoid announcements during each intermediate update.
- When suggestions are empty, unavailable or the query is cleared, remove stale links. If it would help, explain that people can still submit the query. If a refresh would remove a focused suggestion, keep it available until focus leaves or return focus to the field before removing it. Ensure a response for an older query cannot replace the current suggestions.
- Prefer a simple reveal. If you animate the suggestions, honour [reduced motion preferences](Motion%20and%20Animation.md#reduced-motion-preferences).

## Reference implementation

<span id="implementation"></span>

### Search field labels

Prefer a persistent visible text label. In a compact single-field search, a visible ‘Search’ button can provide visual context if it clearly belongs to the field. Give the field its own associated label. If the design cannot show the label, use a tested visually hidden technique only when the search context remains clear; keep the label available to speech input and screen reader users. Keep a visible field label if the submit button is icon-only. Give the icon-only button an accessible name and hide its decorative icon from assistive technology. Keep essential query instructions visible as people type. Use placeholder text only as a supplementary example.

Use `type="search"` for the query field. Use `enterkeyhint="search"` on the field to suggest what the Enter key says on an on-screen keyboard. Keep an explicit submit button available. For an inline panel, use a separate `type="button"` control to close it.

### Markup example

This always-visible local search submits without JavaScript. Replace the example action with the service’s search endpoint. Give each form and label pair its own IDs where needed. The native `<search>` element identifies the search interface and can wrap the form and suggestions.

```html
<search aria-label="Help centre">
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
</search>
```

The markup starts with an empty status message and hidden list. To enhance it, populate the list with working links for the current query, remove `hidden` from the section and update the existing status text with the count. When hiding the section, clear or update the status text to match. Keep the status element in the document and outside the hidden section so it is available before the update. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

When suggestions fill in a query value, choose a pattern for that interaction, such as [Combobox](https://www.w3.org/TR/wai-aria-practices/#combobox). Use ordinary links for suggestions that lead directly to pages.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

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

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)

## Sources

<span id="related-references"></span>

- [Where To Put Your Search Role](http://adrianroselli.com/2015/08/where-to-put-your-search-role.html)
- [When To Use An ARIA Role](https://developer.paciellogroup.com/blog/2012/06/html5-accessibility-chops-when-to-use-an-aria-role/)
- [Why it's important to give your HTML button a type](https://dev.to/claireparkerjones/why-its-important-to-give-your-html-button-a-type-58k9)
- [`<search>` element (HTML Standard)](https://html.spec.whatwg.org/multipage/grouping-content.html#the-search-element)
- [`<datalist>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)
- [Combobox - ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices/#combobox)
