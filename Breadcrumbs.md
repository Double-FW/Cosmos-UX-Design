---
title: "Breadcrumbs"
description: "A navigational indicator for displaying the current location within a website or application."
---

# Breadcrumbs

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

A breadcrumb component provides users with a clear and intuitive way to understand the hierarchy of the content, and to navigate back to previous sections of the website. Breadcrumbs are particularly useful for large websites or web applications with a deep and complex structure.

## Recommended markup

Use a named `nav` with a list of ancestor links. Mark the current page with `aria-current="page"`.

## Recommended layout

<a name="presentation"></a>

Each ancestor breadcrumb should link to the corresponding section of the website or application. The breadcrumb should be visually separated from the main content, and should be distinguishable from regular links. The last item in the breadcrumb list should indicate the current page.

### Requirements

There are no further requirements for this component.

### Guidelines

Between each link there should be a separator, such as a chevron or a slash. The separator should be visually distinguishable from the links.

## Recommended behaviour

Let people follow ancestor links through ordinary browser navigation. Update the current-page marker as they move through the hierarchy.

## Reference implementation

<a name="implementation"></a>

The breadcrumb should be a list of links, using either an `<ol>` or `<ul>` element. The last item in the list should indicate the current page and should not be linked. The list of links should be contained within a `<nav>` element with an appropriate accessible name, such as "breadcrumb".

``` html
<nav aria-label="breadcrumb" data-ddp-component-breadcrumb>
  <ol>
    <li><a href="#">Home</a></li>
    <li><a href="#">Section 1</a></li>
    <li><a href="#">Section 2</a></li>
    <li aria-current="page">Current Page</li>
  </ol>
</nav>
```

> Screen reader: "breadcrumb, navigation"
> Screen reader: "list, 4 items"
> Screen reader: "link, Home, 1 of 4"
> ...
> Screen reader: "Current Page, current item, 4 of 4"
> Screen reader: "end of list"

### Requirements

The breadcrumb component should be contained within a `<nav>` element with an appropriate accessible name, such as "breadcrumb". The list of links should be contained within an `<ol>` or `<ul>` element. The last item in the list should indicate the current page and should not be linked; this can be achieved by removing the `href` attribute from the `<a>` element or removing the `<a>` element entirely, and by adding `aria-current="page"` to the last `<li>` element.

### Guidelines

The separator between each link should not be announced by a screen reader. Therefore, use CSS to create a shape or to add a background image to the separator, rather than using a Unicode character such as a chevron or a slash.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="checkpoints-for-the-tester"></a>

1. The breadcrumbs list is contained within a `nav` HTML element.
2. The containing `nav` HTML element has the HTML attribute of `aria-label="breadcrumb"` with a reasonable value (not necessarily "breadcrumb").
3. The breadcrumb items are implemented as list items in an `ol` or `ul` HTML element.
4. Ancestors are links. The current page is identified and is not a link.
5. Only the breadcrumb list item representing the current page has an HTML attribute and value of `aria-current="page"`
6. The visual separator used to divide each breadcrumb item is not read out by screen readers.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)

## Sources

<a name="links-and-resources"></a>

* [Accessible Breadcrumb Navigation Pattern](https://scottaohara.github.io/a11y_breadcrumbs/)
