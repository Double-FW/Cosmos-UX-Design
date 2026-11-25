---
title: Promotional Cards
description: "A promotional card (promo) helps people choose whether to follow a link to featured content."
---

# Promotional Cards

Use a promotional card (promo) to link to featured content. Give it one primary destination. Use [Cards](Cards.md) when people can complete a task within the card, such as playing a video or revealing more detail.

## Structure

Place a list of featured items in the main content when browsing them is the purpose of the page. For related items that supplement the page, use an `aside` with a descriptive name such as ‘Related programmes’. Group peer promos in an unordered list. A headline link can stand alone in a list item. Use a heading when the item introduces a section and choose its level from the surrounding structure.

Make the headline recognisable when read on its own. Use wording that readers will recognise in both the destination page title and its main heading. A short description can follow the link. Place [metadata](Metadata%20Strips.md) after the description.

An optional image may come before the headline in source order. Give a decorative image empty alternative text (`alt=""`) so a screen reader skips it. If the image helps someone choose the destination, describe the information it adds. In that case a screen reader user may hear the image description before the headline. See [Functional Alternative Text](Functional%20Alternative%20Text.md).

A media indicator may accompany the image before the headline. Hide the indicator from assistive technology with `aria-hidden="true"` and include its meaning in the headline link after the headline text, for example ‘Coastal walks, video, four minutes’. Include the duration when the indicator gives one.

## Implementation

Use a genuine headline link. Extend its pointer target across the promo, for example with a positioned pseudo-element. Keep the description and metadata outside the link so its name stays concise. Leave the image without a separate link. Include a secondary link only when it serves a distinct destination, such as a category. Keep it above the extended target so people can activate it independently.

In a grid, let items in a row share the height needed by the longest item. Metadata can align at the bottom. Reserve space for images and preserve their proportions. Stack the image and text when they no longer fit side by side. Keep the source order and ensure the list remains readable without the grid layout.

Give the focused link a visible indicator. A card outline can help identify which promo contains focus. Keep the card boundary visible in forced colours.

## ACT-style checks

Rule identifier: `gel-components-promos`

Inspect each promo in the DOM. Apply image, media, description and metadata checks wherever those features appear.

1. Follow the headline link. Check that it contains text and that its wording identifies the same content as the destination page title and main heading.
2. Inspect the source order. Only the optional image and its media indicator may precede the headline link. Check that any description follows the headline and precedes any metadata.
3. Check the image alternative against the information the image adds. Read the promo with a screen reader. A decorative image is skipped. A meaningful image before the headline is described before the link. Check that the image has no separate link.
4. For a media indicator, check that the icon is hidden from assistive technology and that the headline link conveys the same format and any displayed duration. Listen to the link on its own to confirm the information is available once.

## Checkpoints for the tester

1. Tab through the promo. Check that the primary destination has one tab stop, that focus is visible and that any secondary link has its own visible focus indicator. Repeat with forced colours enabled and check that the card boundary remains visible.
2. Use the headline link’s browser menu to open the destination in a new tab. Activate the card surface with a pointer, then activate any secondary link. Check that each opens its intended destination.
3. Narrow the viewport until the promo stacks. Read it again and check that the sequence is unchanged, the image keeps its proportions and the content remains readable.

## Related references

- [windows use high contrast mode](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [Complementary Landmark](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html)
- [How to structure headings for web accessibility](https://www.nomensa.com/blog/2017/how-structure-headings-web-accessibility)
- [Basic screen reader commands for accessibility testing](https://developer.paciellogroup.com/blog/2015/01/basic-screen-reader-commands-for-accessibility-testing/)
- [How To Write Page Titles For Google & Other Search Engines in 2018](https://www.hobo-web.co.uk/title-tags/#page-titles-example-use)
- [`object-fit` (caniuse data)](https://caniuse.com/#feat=object-fit)
- [Keyboard Accessibility](https://webaim.org/techniques/keyboard/)
- [Accessibility and HTML5 Block Links](https://simplyaccessible.com/article/html5-block-links/)
- [The description list element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl)
