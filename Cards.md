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

## ACT-style checks

Rule identifier: `gel-components-cards`

Run on a single card and on a collection of peer cards. Apply collection-heading and list checks to the grouped variant.

1. Navigate the collection by headings and then by list items.
2. Check that the collection name, item count, peer headings and image alternatives give enough information to choose an item.
