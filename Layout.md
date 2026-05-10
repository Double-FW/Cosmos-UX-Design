---
title: Layout
description: aGood webpage layout can make the content easier to navigate, understand, and interact with for people with disabilities.
---

## Visual order versus source order

Often web design starts from the perspective of how a component will look. Designers take for granted that positioning elements together visually will make it clear that they are related. While this is true for sighted users, for users of assistive technology, such as a screen reader, the order of the content in the HTML source code controls their experience.

A widespread example is a card pattern that features an image, a topic heading, a card heading (linked), and some card copy. If we assume that the card image is part of the content, that is, it has been attributed with alternative text for screen reader users, then it would be incorrect to have the card image _before_ the card heading. Technically the card image and the card text are both described and introduced by the card heading, so both image and text should immediately follow the heading in the source order of the web page:

- `H3`: "BLOG POST Nature is our most important ingredient"
  - `IMG`: "Green leaves of an agave plant."
  - `TEXT`: "We're committed to an environmentally-friendly way of living and working."

It would be confusing to a screen reader user, proceeding from the top of the component to the bottom, to hear the image alternative text before the heading that introduces the topic. It might sound as if the image belonged to the previous heading. Worse, a screen reader user who navigates across a webpage by jumping from heading to heading will actually miss the image entirely. While this is acceptable for purely decorative images, images considered "content" shouldn't be made so difficult to find.

> "For screen readers, the HTML _is_ the UI." [^1]

Being aware of this, the best place to start is with the content as it exists in the HTML. The example below keeps the card content in the expected order, with the card heading before the content it describes.

```html
<div class="card card-image">
  <div class="card-image-heading">
    <h3 data-topichead="blog post">
      <a href="https://example.com/blog">Nature is our most important ingredient</a>
    </h3>
    <img src="/static/img/layout/agave-600x200.jpg" alt="Green leaves of an agave plant.">
  </div>
  <div class="card-image-body">
    We're committed to an environmentally-friendly way of living and working.
  </div>
</div>
```

The challenge is how to display the card contents according to the visual design. For this, we can use CSS `flex` layout and visually reverse the order using the `order: -1` rule.

%source(src/demos/foundations/layout/layout.css#1-7)

{% demoiframe
   title="Visual and source order.",
   head="<link rel=\"stylesheet\" href=\"/demos/foundations/layout/layout.css\">"
%}
<div class="card card-image">
  <div class="card-image-heading">
    <h3 data-topichead="blog post">
      <a href="https://example.com/blog">Nature is our most important ingredient</a>
    </h3>
    <img src="/static/img/layout/agave-600x200.jpg" alt="Green leaves of an agave plant.">
  </div>
  <div class="card-image-body">
    We're committed to an environmentally-friendly way of living and working.
  </div>
</div>
{% enddemoiframe %}

> Screen reader: "heading level 3, 2 items BLOG POST, link, Nature is our most important ingredient"
> Screen reader: "Green leaves of an agave plant., image"
> Screen reader: "We're committed to an environmentally-friendly way of living and working." 

{% aside "aside-danger" %}
Displaying elements in a visual order that is different than the HTML source order should only be used in rare cases and only where it improves understanding of the content.
{% endaside %}

## Visual order versus tab order

Screen reader users, and other users too, rely on their keyboards to navigate around webpages instead of, or in addition to, using their mouse or other pointing device. These users apply keyboard shortcuts and the <kbd>tab</kbd> key to move focus between interactive elements on a page.

For webpages written in a left-to-right language, such as English, tab-key navigation should follow the same direction as the text: moving focus from top to bottom and left to right. There are many examples where failing to follow this rule can lead to confusion for disabled users:

> A user with low vision who uses a screen magnifier in combination with a screen reader may be confused when the reading order appears to skip around on the screen. A keyboard user may have trouble predicting where focus will go next when the source order does not match the visual order. [^2]

The example below shows a failing case, where the visual presentation of the navigation links implies they are in the following order: Blog, About, and Contact. CSS has been used to visually move the About link to the middle of the navigation. Users who use their keyboard to move focus across the links will confusingly land on the second link, then on the first, and finally on the third.

```html
<ul class="nav-links">
  <li><a href="">About</a></li>
  <li><a href="">Blog</a></li>
  <li><a href="">Contact</a></li>
</ul>
```
%source(src/demos/foundations/layout/layout.css#30-37)

{% demoiframe
   title="Visual and tab order.",
   head="<link rel=\"stylesheet\" href=\"/demos/foundations/layout/layout.css\">"
%}
<ul class="nav-links">
  <li><a href="">About</a></li>
  <li><a href="">Blog</a></li>
  <li><a href="">Contact</a></li>
</ul>
{% enddemoiframe %}

> Screen reader: "link, About, list 3 items"
> Screen reader: "link, Blog"
> Screen reader: "link, Contact" 

{% aside "aside-info" %}
You can watch a demonstration of the [CSS reordering issue for screen readers](https://youtu.be/spxT2CmHoPk?t=1934) presented by Léonie Watson, hosted on YouTube.
{% endaside %}

[^1]: Quoted from [Ian Pouncey](https://ianpouncey.com/)
[^2]: [_Making the DOM order match the visual order_, Techniques for WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/C27.html)
