---
title: "Data Tables"
description: "Help people compare values across rows and columns, including when a table needs to scroll."
---

# Data Tables

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a data table when people need to compare values or look up information by row and column. A value can be hard to interpret if its header is missing or if the layout moves the value under a different header at a narrow width. Preserve the relationships in the markup and on screen.

## Recommended markup

Use a table with a caption and associated headers. Where sorting is offered, use native buttons for the sort controls.

## Recommended layout

<span id="presentation"></span>

Give the table a descriptive caption so people can identify it when navigating directly to it with a screen reader. If the same text also introduces a section, you can put a heading of the appropriate level inside the caption. Give rows and columns meaningful headers where needed. For example, name the column that contains row headers ‘Location’ instead of leaving its header empty.

Keep each value aligned with its own row and column headers. Text can wrap inside a cell while the cell stays in its row and column. If the table does not fit, contain the overflow within the table area so the surrounding page can still reflow. See [Responsive grids](Layout.md#responsive-grids) for layout guidance.

Make overflow discoverable. Keep the scrollbar available where the system displays it and provide a cue such as ‘Scroll to see more columns’ when the table overflows. A shadow can supplement the cue. Check whether it remains visible in forced colours and whether it clears when the user reaches the end.

## Recommended behaviour

<span id="behaviour"></span>

### Scrolling and sticky headers

Let people reach the scroll area and move through its full width using the keyboard. Give a focusable wrapper a descriptive name and a visible focus indicator. Tab should move onward to any controls in the table and then to the next element on the page.

Consider sticky column headers for long tables. Check which container the headers stick to, since an overflow wrapper can make them stick within that wrapper instead of to the page viewport. Keep the headers aligned with the cells and leave enough room for rows and focused controls at high zoom. If the sticky headers obscure content, use the regular table layout in that condition.

### Sorting

Put a native button in each sortable column header. Use the column label as part of its name so people can distinguish the buttons. Show the active column and direction with a symbol or text that remains understandable without colour. Set `aria-sort="ascending"` or `aria-sort="descending"` on the current header cell and clear it from the previous header when the sort column changes.

Choose an initial direction that fits the data. On a repeat activation of the same button, reverse the direction. Reorder complete rows and sort by the underlying number, date or text value where display formatting could give the wrong order. For example, compare the numeric values of ‘2’ and ‘10’ as numbers. Keep focus on the activated button so the user can check the result or change direction.

## Reference implementation

<span id="implementation"></span>

### Markup example

Prefer native `table`, `tr`, `th` and `td` elements. Use `scope="col"` and `scope="row"` to identify the two kinds of header in a simple table. Use `thead` and `tbody` to group header and body rows. If you have to render a table without native elements, use `table`, `row`, `columnheader`, `rowheader` and `cell` roles as applicable. Test the resulting header associations with a screen reader.

The example shows a read-only table with a keyboard-scrollable wrapper. It needs no script. Place it where the surrounding layout lets the wrapper shrink to the available width.

```html
<div class="table-scroll" role="group" aria-labelledby="collection-caption" tabindex="0">
  <table>
    <caption id="collection-caption">Collection times by location</caption>
    <thead>
      <tr>
        <th scope="col">Location</th>
        <th scope="col">Weekday collection</th>
        <th scope="col">Weekend collection</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">Town hall</th>
        <td>10:00</td>
        <td>11:30</td>
      </tr>
      <tr>
        <th scope="row">Community centre</th>
        <td>09:30</td>
        <td>12:00</td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-scroll {
  max-inline-size: 100%;
  overflow-x: auto;
}
```

Keep the default focus outline or use the [focus indication guidance](Focus.md#focus-indication). If you enhance the wrapper with `ResizeObserver`, compare its scrollable width with its visible width as the wrapper or table changes size. Remove the extra tab stop when the whole table fits, without moving a person who is already using the wrapper. Restore the tab stop when overflow returns. Keep the keyboard-scrollable baseline if the enhancement is unavailable.

For client-side sorting, render the rows in a useful order initially and add the sort buttons when they can operate. If sorting is essential, also provide a working baseline such as server-side sorting.

## Testing Approach and ACT Scripts

The ACT-style checks in this section are pattern-specific checks maintained in this guide. The proposed W3C rules are listed separately.

<span id="checkpoints-for-the-tester"></span>

### ACT-style checks

Rule identifier: `gel-foundations-data-tables`

Run the purpose check on each table to determine its intended semantics. Run the remaining checks only on data tables, testing the native or custom variant in use at wide and narrow widths. Use the project’s supported browsers and screen readers. The purpose check is manual: determine the table’s purpose from its content and task, not from browser-style classification heuristics.

| Method and action | Expected result |
| --- | --- |
| Inspect the markup. Navigate directly to the table with a screen reader. Move across a row and down a column. | A descriptive caption identifies the native table. Data has row headers, column headers or both as appropriate. Native header cells use `th`. The correct headers are available for each value as the user navigates. |
| Manually review the content and task supported by each `<table>`. Decide whether people need relationships across rows or columns to interpret or compare its values. Inspect the role exposed in the accessibility tree and the table’s reading order. Do not infer purpose from row, column or cell counts; width; borders or coloured rows; nested tables; embedded content; or the presence of `<caption>`, `<thead>`, `<tfoot>` or `<colgroup>` alone. | A table is exposed as data when its cell relationships are meaningful to the task. If it is used only to position content, CSS layout is preferred. If a legacy layout table remains, its semantics do not create misleading table navigation and its content order remains meaningful. If purpose and exposed semantics disagree or remain unclear, record the issue for review; do not add legacy heuristic markers to force a classification. |
| For a custom table, inspect the accessibility tree and repeat the cell navigation. | The container exposes the table role. Rows expose the row role and their children expose columnheader, rowheader or cell roles as applicable. Each value has the right header context. |
| Narrow the viewport and inspect corresponding headers and cells before and after scrolling within the table. | Column headers stay above their data and row headers stay beside their data. No cell wraps into another row or column. Any necessary two-dimensional scrolling stays within the table area and the surrounding page reflows. |

### Variant and fallback checks

1. At a narrow width, Tab to an overflowing table. Scroll to its last column with the keyboard and then Tab onward. Check that the overflow cue and focus indicator are visible, that every cell can be brought into view and that focus can leave the area. If conditional focus is implemented, widen and narrow the viewport again to check that the tab stop responds to overflow.
2. Increase text size and check that cell text remains readable without changing its header association. In a separate run, increase page zoom on a long table. Scroll down and focus a control near the sticky header if one is present. Check that the header stays aligned and leaves enough room to read and operate the table.
3. Activate a sort button twice, then activate another column. Check the complete row order, the visible sort direction and the exposed sort direction. Confirm that focus stays on the activated button and that only the current header has `aria-sort`.
4. Block scripts and reload. Check that the table remains readable in its initial order and that an overflowing table still scrolls with the keyboard. Check any baseline sort offered by the service.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Table header cell has assigned cells (d0f69e)](https://www.w3.org/WAI/standards-guidelines/act/rules/d0f69e/proposed/)
- [Scrollable content can be reached with sequential focus navigation (0ssw9k)](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)

## Sources

- [Layout Tables Versus Data Tables](https://webaim.org/techniques/tables/#uses)
- [W3C WAI Tables Tutorial](https://www.w3.org/WAI/tutorials/tables/) — determine data-table structure from meaningful cell relationships and mark up headers accordingly.
- [HTML Living Standard: Obsolete features](https://html.spec.whatwg.org/multipage/obsolete.html) — identifies the table `summary` attribute as obsolete.
- [VoiceOver and Tables with an Empty First Header Cell](http://accessibleculture.org/articles/2010/10/voiceover-and-tables-with-an-empty-first-header-cell/)
- [The Table Caption element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/caption)
- [Pure CSS scrolling shadows with `background-attachment: local`](http://lea.verou.me/2012/04/background-attachment-local/)
- [Table header cell has assigned cells (d0f69e)](https://www.w3.org/WAI/standards-guidelines/act/rules/d0f69e/proposed/)
- [Scrollable content can be reached with sequential focus navigation (0ssw9k)](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
