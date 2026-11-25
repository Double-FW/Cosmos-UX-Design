---
title: Cards
description: "Cards in a collection."
---

# Cards

Use an unordered list for a collection of peer cards. Introduce the collection with a heading and use the next heading level for each card headline. Place the headline first in text reading order. Describe a salient image when people need its information to choose a card. See [Functional Alternative Text](Functional%20Alternative%20Text.md).

## Markup example

Replace the sample routes with the destination URLs.

```html
<section aria-labelledby="guides-heading">
  <h2 id="guides-heading">Travel guides</h2>
  <ul>
    <li><h3><a href="/guides/rail">Travelling by rail</a></h3>
      <p>Compare routes and ticket options.</p></li>
    <li><h3><a href="/guides/bus">Travelling by bus</a></h3>
      <p>Find local services and accessible stops.</p></li>
  </ul>
</section>
```

## Opening a dialog from a card

Use [Cards – Reveal](Cards%20-%20Reveal.md) when readers can explore supplementary information within the card. Use a modal dialog when a task needs their attention and the underlying page must be inactive. Follow [Modal Dialogs](Modal%20Dialogs.md) for implementation and accessibility checks.

To test a collection, open a dialog from one card using the keyboard. Check that its content and accessible name identify the item you chose. Close it and check that focus returns to the button that opened it, unless the action takes you to a new page or step. Repeat with a different card to catch triggers that open the wrong item or return focus to the first card.

## ACT-style checks

Rule identifier: `gel-components-cards`

Run on a single card and on a collection of peer cards. Apply collection-heading and list checks to the grouped variant.

1. Navigate the collection by headings and then by list items.
2. Check that the collection name, item count, peer headings and image alternatives give enough information to choose an item.
