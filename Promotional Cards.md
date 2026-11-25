---
title: Promotional Cards
description: "Choosing a destination."
---

# Promotional Cards

Use a descriptive headline link whose text matches the destination’s subject. Group related promotions using [Cards](Cards.md). Use one destination link for the image and headline where they share an action. Reserve space for the image and keep its proportions at narrow widths. Make the link focus indicator visible across the card.

## Implementation

If the whole card is clickable, preserve a genuine headline link so people can open it in a new tab or use the browser’s link menu. Keep secondary destinations independently operable. When a card changes from horizontal to stacked layout, retain its reading order.

## ACT-style checks

Rule identifier: `gel-components-promos`

Run on promotions with and without images, descriptions and media indicators. Apply each optional-content check only where that feature occurs.

1. Check the source order.
2. Only an optional image or decorative media indicator may precede the headline.
3. If an indicator means ‘video’ or ‘audio’, include that information in the link text and hide the duplicate icon from assistive technology.
4. Place the description after the headline and before [metadata](Metadata%20Strips.md).
5. Check meaningful images have an equivalent and decorative ones use empty alt text.

## Related references

- [windows use high contrast mode](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [Complementary Landmark](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html)
- [How to structure headings for web accessibility](https://www.nomensa.com/blog/2017/how-structure-headings-web-accessibility)
- [Basic screen reader commands for accessibility testing](https://developer.paciellogroup.com/blog/2015/01/basic-screen-reader-commands-for-accessibility-testing/)
- [How To Write Page Titles For Google & Other Search Engines in 2018](https://www.hobo-web.co.uk/title-tags/#page-titles-example-use)
- [`object-fit` (caniuse data)](https://caniuse.com/#feat=object-fit)
- [Keyboard Accessibility](https://webaim.org/techniques/keyboard/)
- [Accessibility and HTML5 Block Links —](https://simplyaccessible.com/article/html5-block-links/)
- [The Description List element represents an association list consisting of zero or more name-value groups (a description list). see](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl)
