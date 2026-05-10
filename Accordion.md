---
title: Accordion
description: A pattern for hiding and showing related content.
---

Accordions are used to reduce the need to scroll when presenting multiple sections of content on a single page. They should be used when you have a lot of content that can be broken down into sections of information where it is useful to hide sections that are not immediately relevant to the user.

## Presentation

Accordions are usually displayed as a series of headings that function as controls that enable users to reveal or hide their associated sections of content. Individual sections within an accordion component open and close independently of one another or be grouped so that only one section can be open at a time. In the latter case, opening a new section closes the previously opened section.

### Requirements

The control for each section of an accordion must have a method, usually an icon such as a plus / minus symbol or arrow right / arrow down, to indicate whether the section is open or closed. The content for the section must immediately follow the control that expands / collapses the content.

### Guidelines

There are no further guidelines for this component.

## Implementation

### Native HTML

If you do not require the accordions to act as a group in which opening one closes the others, you can use the native HTML `<details>` and `<summary>` elements to create an accordion. The `<details>` element creates a container that can be opened or closed, and the `<summary>` element creates a heading for the container. No JavaScript is required to make this work and CSS can be used to style the accordion.

#### Example implementation

``` html
<div role="region" aria-label="[Accordion region title]" data-ddp-component-accordion>
  <details open>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
  
  <details>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
  
  <details>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
</div>
```

%source(src/demos/components/accordion/accordion.css#1-8)

{% demoiframe
   title="Accordion example.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/accordion/accordion.css\">"
%}
<div role="region" aria-label="[Accordion region title]">
  <details open>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
  
  <details>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
  
  <details>
    <summary><h5>[Accordion section title]</h5></summary>
    <div>
      [accordion section content]
    </div>
  </details>
</div>
{% enddemoiframe %}

#### Requirements

There are no requirements for this version of the accordion component.

#### Guidelines

Either a `<section>` element or a `<div role="region">` element should be used to contain the accordion. In each case, they should be given an accessible name that describes the content of the accordion. This will indicate to screen reader users that the `<details>` elements, while operating separately, are part of the same collection of content. The accessible name can be provided using an `aria-label` attribute or an `aria-labelledby` attribute that references the `id` of an element that contains the accessible name. Use of an `aria-labelledby` attribute that references a heading element is preferred:

``` html
<section aria-labelledby="accordion-group-heading">
  <h4 id="accordion-group-heading">[Accordion group heading]</h4>

  …
</section>
```

Each `<details>` element may have a heading element of the appropriate level as it's child; the heading element describes the content that follows.

### Custom HTML

If you require the accordions to act as a group in which opening one closes the others, you must use a custom solution. The basic structure is the same, but instead of using the `<details>` and `<summary>` elements, you will need to use `<div>` elements and a `<button>` element to create the accordion. The `<button>` element will be used to control the opening and closing of the accordion sections, and the `<div>` elements will be used to contain the content of each section. The `<button>` element will be given an `aria-controls` attribute that references the `id` of the `<div>` element that contains the content of the section. The `<div>` element will be given a `role="region"` attribute and an `aria-labelledby` attribute that references the `id` of the `<button>` element that controls the section. The `<div>` element will also be given a `hidden` attribute to hide the content of the section by default. The `<button>` element will be given an `aria-expanded` attribute to indicate whether the section is open or closed.

#### Code example

``` html
<div role="region" aria-label="[Accordion region title]" data-ddp-component-accordion>
  <h5><button type="button" id="accordion-control-1" aria-controls="accordion-content-1" aria-expanded="true">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-1" aria-labelledby="accordion-control-1">
    [accordion section content]
  </div>

  <h5><button type="button" id="accordion-control-2" aria-controls="accordion-content-2" aria-expanded="false">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-2" aria-labelledby="accordion-control-2" hidden>
    [accordion section content]
  </div>

  <h5><button type="button" id="accordion-control-3" aria-controls="accordion-content-3" aria-expanded="false">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-3" aria-labelledby="accordion-control-3" hidden>
    [accordion section content]
  </div>
</div>
```

%source(src/demos/components/accordion/accordion.js)

%source(src/demos/components/accordion/accordion.css#10-36)

{% demoiframe
   title="Custom HTML example.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/accordion/accordion.css\"><script src=\"/demos/components/accordion/accordion.js\"></script>"
%}
<div role="region" aria-label="[Accordion region title]" data-ddp-component-accordion>
  <h5><button type="button" id="accordion-control-1" aria-controls="accordion-content-1" aria-expanded="true">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-1" aria-labelledby="accordion-control-1">
    [accordion section content]
  </div>

  <h5><button type="button" id="accordion-control-2" aria-controls="accordion-content-2" aria-expanded="false">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-2" aria-labelledby="accordion-control-2" hidden>
    [accordion section content]
  </div>

  <h5><button type="button" id="accordion-control-3" aria-controls="accordion-content-3" aria-expanded="false">[Accordion section title]</button></h5>
  <div role="region" id="accordion-content-3" aria-labelledby="accordion-control-3" hidden>
    [accordion section content]
  </div>
</div>
{% enddemoiframe %}

#### Requirements

Each section of an accordion must be a heading element of the appropriate level that contains a `<button type="button">` element. The `<button>` element must have an `aria-controls` attribute that references the `id` of the content element that is associated with the section. When the value of `aria-expanded` is `true`, the content element must be visible and, if only one section can be expanded at a time, the `<button>` element must be disabled. When the value of `aria-expanded` is `false`, the content element must be hidden, both visually and in the accessibility tree, and the `<button>` element must not be disabled.

#### Guidelines

Either a `<section>` element or a `<div role="region">` element should be used to contain the accordion. In each case, they should be given an accessible name that describes the content of the accordion. This will indicate to screen reader users that the following content is part of the same component. The accessible name can be provided using an `aria-label` attribute or an `aria-labelledby` attribute that references the `id` of an element that contains the accessible name. Use of an `aria-labelledby` attribute that references a heading element is preferred:

``` html
<section aria-labelledby="accordion-group-heading">
  <h4 id="accordion-group-heading">[Accordion group heading]</h4>

  …
</section>
```

The `<button>` element may have an `aria-controls` attribute that is set to the `id` that references the `id` of the content element.

The content element may have a `role="region"` attribute and an `aria-labelledby` attribute that references the `id` of the `<button>` element that controls the section.

## Checkpoints for the Tester

1. It must be possible to move focus to each of the accordion title elements using the <kbd>tab</kbd> key.
1. It must be possible to open a closed accordion section using only a keyboard for example, by pressing the <kbd>enter</kbd> or <kbd>space</kbd> keys.
1. For accordions that allow all sections to be closed, it must be possible to close the currently open section using only a keyboard for example, by pressing the <kbd>enter</kbd> or <kbd>space</kbd> keys.
1. For accordions that require one section to be open (and by opening a section other sections are closed), the accordion title element for the currently open section must be disabled.
1. A screen reader should announce that each of the accordion title elements is an element that can be interacted with, whether it is currently in a closed (collapsed) or open (expanded) state, and whether it is currently disabled.
1. The text of each accordion title should indicate the content of the section: for example, "Ingredients" or "Contact information".
1. There is a visible indication must show that each accordion title is an interactive control and which state the accordion section is in, for example a pointer symbol in a closed or open position.
1. The accordion section content must _not_ be perceivable, either visibly or with a screen reader, when it is in a closed state.
1. The accordion section content must be readable: with JavaScript disabled, with CSS disabled, 
## Links and resources

* [W3C ARIA Authoring Practices Guide: Accordion](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/)
