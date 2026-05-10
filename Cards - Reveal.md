---
title: Cards - Reveal
description: A card that uses a disclosure component which can be used to reveal or hide additional content.
---

The reveal card is often used to display such as FAQs, terms and conditions, or other information that may not be immediately relevant to all users. It can also be used to improve the usability of a webpage by allowing users to control the amount of content they are presented with at once.

## Presentation

The disclosure component should be visually indicated as a button that, when activated, reveals or hides additional content. The button should have clear and descriptive text, such as "Show more" or "Read more", to indicate its purpose.

### Requirements

The control for a disclosure component must have a visual indication, such as a plus and minus symbol or an arrow right and arrow down, to indicate whether the section currently is opened or closed. The content for the section must immediately follow the control that expands or collapses that content.

## Implementation

### Native HTML

You can use the native HTML `<details>` and `<summary>` elements to create a disclosure component. The `<details>` element creates a container that can be opened or closed, and the `<summary>` element creates a heading for the container. No JavaScript is required to make this work and CSS can be used to style the component.

``` html
<details>
  <summary>Show More</summary>
  <div>
    [reveal content]
  </div>
</details>
```

{% demoiframe
   title="Native HTML.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/cards-reveal/cards-reveal.css\">"
%}
<details>
  <summary>Show More</summary>
  <div>
    [reveal content]
  </div>
</details>
{% enddemoiframe %}

> Screen reader: "Show More, collapsed, reveal triangle, group"
> Screen reader: "Show More, expanded, reveal triangle, group"

### Styling the reveal pointer

One of the benefits of using the native HTML element is that the browser will manage the display of the reveal pointer for you. However you may wish to exert more control over its appearance. In that case it is possible to use CSS to style the native reveal pointer, for example by using a "plus" and "minus" image instead.

%source(src/demos/components/cards-reveal/cards-reveal-pointerstyle.css)

``` html
<details data-ddp-component-disclosure>
  <summary>Show More</summary>
  <div>
    [reveal content]
  </div>
</details>
```

{% demoiframe
   title="Styled pointer example.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/cards-reveal/cards-reveal.css\"><link rel=\"stylesheet\" href=\"/demos/components/cards-reveal/cards-reveal-pointerstyle.css\">"
%}
<details data-ddp-component-disclosure>
  <summary>Show More</summary>
  <div>
  Lorem ipsum dolor sit id culpa enim ea et consequat sunt exercitation nulla eiusmod. 
  <ul>
  <li>one detail</li>
  <li>two detail</li>
  <li>three detail</li>
  </ul>
  </div>
</details>
{% enddemoiframe %}

### Custom HTML

You may also create a custom HTML version of the disclosure component. The basic structure is the same, but instead of using the `<details>` and `<summary>` elements, you will need to use `<div>` elements and a `<button>` element to create the component. The `<button>` element will be used to control the opening and closing of the disclosure component, and the `<div>` element will be used to contain the content of the disclosure component. The `<button>` element will be given an `aria-controls` attribute that references the `id` of the `<div>` element that contains the content of the disclosure component. The `<div>` element will be given a `role="region"` attribute and an `aria-labelledby` attribute that references the `id` of the `<button>` element that controls the disclosure component. The `<div>` element will also be given a `hidden` attribute to hide the content of the disclosure component by default. The `<button>` element will be given an `aria-expanded` attribute to indicate whether the disclosure component is open or closed.

``` html
<div data-ddp-component-disclosure>
  <button type="button" id="reveal-control-1" aria-controls="reveal-content-1" aria-expanded="false">Find out more</button>
  <div role="region" id="reveal-content-1" aria-labelledby="reveal-control-1" hidden>
    [reveal content]
  </div>
</div>
```

%source(src/demos/components/cards-reveal/cards-reveal.js)

%source(src/demos/components/cards-reveal/cards-reveal.css)

{% demoiframe
   title="Custom HTML example.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/cards-reveal/cards-reveal.css\"><script src=\"/demos/components/cards-reveal/cards-reveal.js\"></script>"
%}
<div data-ddp-component-disclosure>
  <button type="button" id="reveal-control-1" aria-controls="reveal-content-1" aria-expanded="false">Find out more</button>
  <div role="region" id="reveal-content-1" aria-labelledby="reveal-control-1" hidden>
    <p>Lorem ipsum dolor sit amet ipsum non ut ut enim in tempor non aliqua irure adipisicing.</p>
    <p>Laborum culpa voluptate dolor irure ut veniam quis consequat mollit officia ut. Consequat voluptate deserunt proident dolor magna sit ipsum id eu.</p>
  </div>
</div>
{% enddemoiframe %}

> Screen reader "Find out more, collapsed, button
> Screen reader "Find out more, expanded, button

#### Requirements

The `<button>` element for each disclosure component must have an `aria-controls` attribute that references the `id` of the associated content. When the value of `aria-expanded` is `true`, the content element must be visible. When the value of `aria-expanded` is `false`, the content element must be hidden, both visually and in the accessibility tree.

#### Guidelines

The `<button>` element may have an `aria-controls` attribute that is set to the `id` that references the `id` of the content element.

The content element may have a `role="region"` attribute and an `aria-labelledby` attribute that references the `id` of the `<button>` element that controls the disclosure component.

## Checkpoints for the Tester

1. It must be possible to move focus to the reveal title element using the <kbd>tab</kbd> key.
2. It must be possible to open and close the reveal using only a keyboard for example, by pressing the <kbd>enter</kbd> or <kbd>space</kbd> keys.
3. A screen reader should announce that the reveal title is an element that can be interacted with and whether it is currently in a closed (collapsed) or open (expanded) state.
4. The text of the reveal title should indicate its purpose: for example, "Show More".
5. There is a visible indication must show that the reveal title is an interactive controls and which state the reveal is in, for example a pointer symbol in a closed or open position.
6. The reveal content must _not_ be perceivable, either visibly or with a screen reader, when it is in a closed state.
7. The reveal content must be readable: with JavaScript disabled, with CSS disabled, 

## Links and resources

* [W3C ARIA Authoring Practices Guide: Disclosure (Show/Hide)](https://www.w3.org/WAI/ARIA/apg/patterns/reveal/)
