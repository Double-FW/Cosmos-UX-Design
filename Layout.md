---
title: Layout
description: Good webpage layout can make the content easier to navigate, understand, and interact with for people with disabilities.
---

# Layout

Start with the [Page Structure example](Page_Structure.md#applying-regions) to organise the header, navigation, main content and footer in HTML before applying layout styles.

## Visual order versus source order

Often web design starts from the perspective of how a component will look. Designers take for granted that positioning elements together visually will make it clear that they are related. While this is true for sighted users, for users of assistive technology, such as a screen reader, the order of the content in the HTML source code controls their experience.

A widespread example is a card pattern that features an image, a topic heading, a card heading (linked), and some card copy. If we assume that the card image is part of the content, that is, it has been attributed with alternative text for screen reader users, then it would be incorrect to have the card image _before_ the card heading. Technically the card image and the card text are both described and introduced by the card heading, so both image and text should immediately follow the heading in the source order of the web page:

- `H3`: "BLOG POST Nature is our most important ingredient"
  - `IMG`: "Green leaves of an agave plant."
  - `TEXT`: "We're committed to an environmentally-friendly way of living and working."

It would be confusing to a screen reader user, proceeding from the top of the component to the bottom, to hear the image alternative text before the heading that introduces the topic. It might sound as if the image belonged to the previous heading. Worse, a screen reader user who navigates across a webpage by jumping from heading to heading will actually miss the image entirely. While this is acceptable for purely decorative images, images considered "content" shouldn't be made so difficult to find.

> "For screen readers, the HTML _is_ the UI." [^1]

Being aware of this, the best place to start is with the content as it exists in the HTML. The example below keeps the card heading before the content it describes. Choose the heading level from [its place in the page structure](Headings.md#choose-a-level-from-the-structure). The example uses an `h3` for a card within a level-two section.

```html
<div class="card card-image">
  <div class="card-image-heading">
    <h3 data-topichead="blog post">
      <a href="https://example.com/blog">Nature is our most important ingredient</a>
    </h3>
    <img src="agave-600x200.jpg" alt="Green leaves of an agave plant.">
  </div>
  <div class="card-image-body">
    We're committed to an environmentally-friendly way of living and working.
  </div>
</div>
```

If the image placement can change without changing meaning or keyboard order, this CSS places it above the heading while leaving the source order intact.

```css
.card-image-heading { display: flex; flex-direction: column; }
.card-image-heading img { order: -1; }
```

> Screen reader: "heading level 3, 2 items BLOG POST, link, Nature is our most important ingredient"
> Screen reader: "Green leaves of an agave plant., image"
> Screen reader: "We're committed to an environmentally-friendly way of living and working." 

Displaying elements in a visual order that is different than the HTML source order should only be used in rare cases and only where it improves understanding of the content.

## Visual order versus tab order

Screen reader users, and other users too, rely on their keyboards to navigate around webpages instead of, or in addition to, using their mouse or other pointing device. These users apply keyboard shortcuts and the <kbd>tab</kbd> key to move focus between interactive elements on a page.

Use a focus sequence that is predictable, preserves meaning and lets people complete the task. Compare what people see with what happens as they Tab through controls. See [Keyboard Navigation](Keyboard%20Navigation.md#reach-and-operate-each-function) for the task-level checks.

> A user with low vision who uses a screen magnifier in combination with a screen reader may be confused when the reading order appears to skip around on the screen. A keyboard user may have trouble predicting where focus will go next when the source order does not match the visual order. [^2]

In this example, CSS moves Blog to the start of the visible list. The visual order is Blog, About, Contact. The tab order is About, Blog, Contact. Focus jumps backwards on the second Tab, which makes the sequence harder to follow.

```html
<ul class="nav-links">
  <li><a href="">About</a></li>
  <li><a href="">Blog</a></li>
  <li><a href="">Contact</a></li>
</ul>
```

```css
.nav-links { display: flex; gap: 1rem; }
.nav-links li:nth-child(2) { order: -1; }
```

> Screen reader: "link, About, list 3 items"
> Screen reader: "link, Blog"
> Screen reader: "link, Contact" 

You can watch a demonstration of the [CSS reordering issue for screen readers](https://youtu.be/spxT2CmHoPk?t=1934), hosted on YouTube.

[^1]: [Source of the quotation](https://ianpouncey.com/)
[^2]: [_Making the DOM order match the visual order_, Techniques for WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/C27.html)

## Responsive grids

Keep each item with its own heading and content in source order. Use lists for collections and tables for data with row and column relationships. Use valid nesting. Build a readable single-column baseline, then apply CSS Grid when supported. Respect the document’s writing direction.

### ACT-style checks

Rule identifier: `gel-components-grids`

Run on a responsive collection in its wide and narrow layouts. Inspect source nesting and confirm ordinary layout does not expose table semantics.

1. Inspect the reading order with layout styles disabled.
2. Resize the viewport, increase magnification and test a right-to-left layout.
3. Items should remain associated with their headings.
4. For data that depends on two dimensions, use [Data Tables](Data%20Tables.md).

### Related references

- [nvda](https://webaim.org/resources/shortcuts/nvda)
- [data tables](https://inclusive-components.design/data-tables/)
- [zoomtext magnifierreader](https://www.zoomtext.com/products/zoomtext-magnifierreader/)
- [The Accessibility Tree](https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree)
- [WCAG2.1 1.3.1 Info & Relationships](https://www.w3.org/TR/WCAG21/#info-and-relationships)
- [WCAG2.1 4.1.1 Parsing](https://www.w3.org/TR/WCAG21/#parsing)
- [Using the presentation role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_presentation_role)
- [WCAG2.1 1.3.2 Meaningful Sequence](https://www.w3.org/TR/WCAG21/#meaningful-sequence)
- [WCAG2.1 2.4.3 Focus Order](https://www.w3.org/TR/WCAG21/#focus-order)
- [`:lang()`](https://developer.mozilla.org/en-US/docs/Web/CSS/:lang)
- [CSS Grid Layout Module Level 1](https://www.w3.org/TR/css-grid-1/#overview-grid)
- [How to make your site accessible for screen magnifiers](https://axesslab.com/make-site-accessible-screen-magnifiers/)
