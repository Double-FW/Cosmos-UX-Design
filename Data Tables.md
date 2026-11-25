---
title: Data Tables
description: "Reading, scrolling and sorting data."
---

# Data Tables

Give the table a descriptive caption. Mark row and column headings with `th` and associate them with the corresponding cells. Keep the two-dimensional relationships when space is limited. If the table overflows, make its scroll region discoverable and operable by keyboard. Keep sticky headers from covering data at zoom.

## Implementation

Place a button inside a sortable column header. Put `aria-sort` on the currently sorted header and update it when the direction changes. Use an indicator whose shape communicates direction without colour. Give a keyboard-scrollable wrapper a name when horizontal scrolling is needed. Keep header cells aligned with the same values at every width.

### Markup example

This example illustrates the semantic structure.

```html
<table>
  <caption>Collection times by location</caption>
  <thead><tr><th scope="col">Location</th><th scope="col">Time</th></tr></thead>
  <tbody><tr><th scope="row">Town hall</th><td>10:00</td></tr></tbody>
</table>
```

## ACT-style checks

Rule identifier: `gel-foundations-data-tables`

Run on a table containing data with row or column relationships. Apply custom-role checks only to implementations that recreate table semantics.

1. Navigate across a row and down a column with a screen reader.
2. Confirm the correct headers are available for each value.
3. For a custom table, inspect the table, row, columnheader, rowheader and cell roles as applicable.
4. At narrow widths, scroll inside the table and confirm the surrounding page still reflows.
5. Activate a sortable header twice and check row order, keyboard focus and the exposed sort direction.

## Related references

- [Layout Tables Versus Data Tables](https://webaim.org/techniques/tables/#uses)
- [VoiceOver and Tables with an Empty First Header Cell](http://accessibleculture.org/articles/2010/10/voiceover-and-tables-with-an-empty-first-header-cell/)
- [The Table Caption element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/caption)
- [Pure CSS scrolling shadows with `background-attachment: local`](http://lea.verou.me/2012/04/background-attachment-local/)
