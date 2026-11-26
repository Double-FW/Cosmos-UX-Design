---
title: "Promotional Cards"
description: "Help people decide whether to explore featured content, play a preview or take the next step."
---

# Promotional Cards

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a promotional card (promo) to help people decide whether to explore featured content, play a preview or take an action. Give the card one clear primary next step. If it also offers playback or a secondary action, make the choices easy to tell apart.

### Choose the interaction

| What people can do | How to present it |
| --- | --- |
| Follow the card to a page with a video | Use the headline as the primary link. A still image and a label such as ‘Video, 4 minutes’ can help people decide what to open. |
| Follow a call to action to a destination | Use a link, such as ‘View the offer’. If the headline already takes people there, use that link as the call to action so the destination has one tab stop. |
| Perform an action within the card | Use a button, such as ‘Apply discount’. Use the [Cards](Cards.md) structure for in-place functionality and follow [Call to Action](Call%20to%20Action.md) for the label, prominence and feedback. |
| Play a video and follow a call to action | Give the player and CTA separate controls, such as Play and ‘Book a guided walk’. Follow [Cards](Cards.md) for structure and [Video Controls](Video%20Controls.md) for playback. Make it clear whether watching is optional. |

## Recommended markup

### Structure

Place a list of featured items in the main content when browsing them is the purpose of the page. For related items that supplement the page, use an `aside` with a descriptive name such as ‘Related programmes’. Group peer promos in an unordered list. A headline link can stand alone in a list item. Use a heading when the item introduces a section and choose its level from the surrounding structure.

Make the headline recognisable when read on its own. Use wording that readers will recognise in both the destination page title and its main heading. A short description can follow the link. Place [metadata](Metadata%20Strips.md) after the description.

An optional image may come before the headline in source order. Give a decorative image empty alternative text (`alt=""`) so a screen reader skips it. If the image helps someone choose the destination, describe the information it adds. In that case a screen reader user may hear the image description before the headline. See [Functional Alternative Text](Functional%20Alternative%20Text.md).

A media indicator may accompany the image before the headline. Hide the indicator from assistive technology with `aria-hidden="true"` and include its meaning in the headline link after the headline text, for example ‘Coastal walks, video, four minutes’. Include the duration when the indicator gives one.

## Recommended layout

Keep the headline and primary action easy to identify. Let image and text stack in reading order as space narrows.

## Recommended behaviour

Let people choose when to start playback. A card with both a player and a CTA should let people operate each independently.

## Reference implementation

<a name="implementation"></a>

For a link-led promo, use a genuine headline link. Keep the description and metadata outside the link so its name stays concise. Leave the image without a separate link. If the headline is the only control, you can extend its pointer target across the promo. If a secondary destination is needed, such as a category, keep it independently operable.

For a card with an embedded player or an action button, keep the surrounding card surface inactive. Keep controls outside any enclosing link and place them in reading order. Position related offer conditions or other decision information before the CTA. Let people start the video themselves and keep the CTA available before and after playback. Provide captions for dialogue and other meaningful audio. Use the [Video Controls](Video%20Controls.md) and [Motion and Animation](Motion%20and%20Animation.md) guides for the player and playback behaviour.

### Markup example

This fragment shows a promotional card with optional in-place playback and a separate link to booking. Supply the poster, video, caption file and destination. The heading level assumes the card sits beneath a level-two section.

```html
<li>
  <h3>Guided coastal walks</h3>
  <p>Watch the route preview or view dates and prices for a guided walk.</p>
  <video controls preload="none" poster="coastal-walk.jpg"
         aria-label="Guided coastal walks preview">
    <source src="coastal-walk.mp4" type="video/mp4">
    <track kind="captions" src="coastal-walk-en.vtt" srclang="en" label="English">
  </video>
  <p><a href="/guided-walks">View guided walk dates and prices</a></p>
</li>
```

Insert the example in a list of peer cards. The host service supplies the booking page and the media alternatives the preview requires.

In a grid, let items in a row share the height needed by the longest item. Metadata can align at the bottom. Reserve space for images and preserve their proportions. Stack the image and text when they no longer fit side by side. Keep the source order and ensure the list remains readable without the grid layout.

Give the focused link a visible indicator. A card outline can help identify which promo contains focus. Keep the card boundary visible in forced colours.

## Testing Approach and ACT Scripts

The ACT-style checks in this section are pattern-specific checks maintained in this guide. The proposed W3C rules are listed separately.

### ACT-style checks

Rule identifier: `gel-components-promos`

Run these checks on the link-led variant. Apply image, media, description and metadata checks where those features appear. For a card with an embedded player or another in-place action, use the [Cards checks](Cards.md#act-style-checks) and the additional checks below.

1. Follow the headline link. Check that it contains text and that its wording identifies the same content as the destination page title and main heading.
2. Inspect the source order. Only the optional image and its media indicator may precede the headline link. Check that any description follows the headline and precedes any metadata.
3. Check the image alternative against the information the image adds. Read the promo with a screen reader. A decorative image is skipped. A meaningful image before the headline is described before the link. Check that the image has no separate link.
4. For a media indicator, check that the icon is hidden from assistive technology and that the headline link conveys the same format and any displayed duration. Listen to the link on its own to confirm the information is available once.

### Checkpoints for the tester

1. Tab through the promo. Check that the primary destination has one tab stop, that focus is visible and that any secondary link has its own visible focus indicator. Repeat with forced colours enabled and check that the card boundary remains visible.
2. Use the headline link’s browser menu to open the destination in a new tab. If the pointer target extends across the card, activate its surface. Then activate any secondary link. Check that each opens its intended destination.
3. Narrow the viewport until the promo stacks. Read it again and check that the sequence is unchanged, the image keeps its proportions and the content remains readable.

4. For a card linking to a video, activate its headline and check that it opens the named destination. Confirm that the still image or media indicator adds no separate tab stop and that the link name conveys the format.
5. For a card with a video and CTA, Tab through the player and activate the CTA separately. Repeat with a pointer. Playback starts only when requested. The CTA remains available while the video is paused, playing and finished. Neither action triggers the other, and focus stays visible.
6. Read the CTA on its own with a screen reader. Check that the role matches its behaviour, the name identifies the action or destination, and any conditions needed to decide are available before activation.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Image has non-empty accessible name (23a2a8)](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/proposed/)
- [Video element auditory content has captions (f51b46)](https://www.w3.org/WAI/standards-guidelines/act/rules/f51b46/proposed/)

## Sources

<a name="related-references"></a>

- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [W3C APG: complementary landmark example](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html)
- [How to structure headings for web accessibility](https://www.nomensa.com/blog/2017/how-structure-headings-web-accessibility)
- [Basic screen reader commands for accessibility testing](https://developer.paciellogroup.com/blog/2015/01/basic-screen-reader-commands-for-accessibility-testing/)
- [How To Write Page Titles For Google & Other Search Engines in 2018](https://www.hobo-web.co.uk/title-tags/#page-titles-example-use)
- [`object-fit` (caniuse data)](https://caniuse.com/#feat=object-fit)
- [Keyboard Accessibility](https://webaim.org/techniques/keyboard/)
- [Accessibility and HTML5 Block Links](https://simplyaccessible.com/article/html5-block-links/)
- [The description list element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl)
