---
title: Search
description: "Finding content."
---

# Search

Use a labelled search landmark, a search input with a persistent accessible name and an explicit submit button. Keep search usable without suggestions. For linked suggestions, name the suggestions region, announce the number available and let users move to results without shifting focus as they type.

## Implementation

Keep the submit button and close button separate. Use `type="button"` for close. Keep hidden suggestions out of the tab sequence until they are visible. Avoid announcing each intermediate update while results are being assembled.

### Search field labels

Prefer a persistent visible text label for the field and keep the input easy to find. In a compact search with one field, a visible ‘Search’ button can supply the visual context if it clearly belongs to that field. Give the field its own accessible name using an associated label, visually hidden only where that context is clear. If the submit button uses only a magnifying glass, give the button an accessible name and check that people recognise the action. Keep a visible field label in that variant.

Use `type="search"` for the query. Use `enterkeyhint="search"` on the input to suggest the action label for the on-screen keyboard’s Enter key. Keep the submit button available.

### Markup example

Replace the sample search route with the service’s query endpoint.

```html
<form role="search" aria-label="Site" action="/search">
  <label for="site-query">Search this site</label>
  <input id="site-query" name="q" type="search" enterkeyhint="search">
  <button type="submit">Search</button>
</form>
```

## Checkpoints for the tester

1. Open global search and check focus reaches the field.
2. Type a query, follow a suggestion, then repeat and submit the typed query.
3. Close global search and verify focus returns to its opener.
4. For local search, verify the results are scoped to the named collection.
5. Check that the search field and submit action are easy to identify visually and have distinct accessible names. On a touchscreen, verify that people can type and submit even if the keyboard ignores the Enter key hint.

## Related references

- [Where To Put Your Search Role](http://adrianroselli.com/2015/08/where-to-put-your-search-role.html)
- [When To Use An ARIA Role](https://developer.paciellogroup.com/blog/2012/06/html5-accessibility-chops-when-to-use-an-aria-role/)
- [Why it's important to give your HTML button a type](https://dev.to/claireparkerjones/why-its-important-to-give-your-html-button-a-type-58k9)
- [`<datalist>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)
- [Combobox - ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices/#combobox)
