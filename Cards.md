---
title: Cards
description: Help people browse related items and use content and actions within each card.
---

# Cards

Use cards when people need to scan a collection and preview, play, save or share an item without leaving the page. Put each card’s headline first in the reading order so people navigating by headings or using a screen reader can identify the item before they encounter its contents. Use [Promotional Cards](Promotional%20Cards.md) when the main purpose is to link to featured content elsewhere.

## Presentation

Use an unordered list for a collection of peer cards. Put a descriptive collection heading before the list. Use the same heading level for every card in that collection, one level below the collection heading. Give a standalone card a heading that fits the surrounding page.

Put the headline before the card’s media, description and actions in source order. Include any headline link within the heading. Keep separate actions out of the headline. If a meaningful image is the main content, describe what it shows and why it matters in context. The description should add to the headline. See [Functional Alternative Text](Functional%20Alternative%20Text.md).

Group optional actions after the content. Give each button an accessible name that identifies its action and item, such as ‘Share the woodland walk’. Prefer visible text, especially when the card has only a few actions. When an icon accompanies a label, keep the icon out of the accessible name.

## Behaviour

Use a link for navigation and a button for an action on the current page. For a reversible choice such as saving an item, use a stable button label such as ‘Save the woodland walk’ and update `aria-pressed` to communicate the current state. For an action that opens sharing options, follow [Share Tools](Share%20Tools.md).

### Revealing content or opening a dialog

Use [Disclosure](Disclosure.md) when readers can explore supplementary information within the card. Give the trigger a label that identifies the item and place the panel immediately after the trigger, before any remaining actions. Opening it should keep focus on the control so the user can continue into the revealed content. If an enhanced card supports Escape from inside the panel, close that panel, update the trigger’s expanded state and return focus to that trigger.

Use a modal dialog when a task needs the user’s attention and the underlying page must be inactive. Follow [Modal Dialogs](Modal%20Dialogs.md) for implementation, dismissal and focus restoration.

## Implementation

### Markup example

This HTML example shows a list with a linked headline and an in-place disclosure. It needs no script or stylesheet. Replace the sample routes with the destination URLs.

```html
<section aria-labelledby="walks-heading">
  <h2 id="walks-heading">Local walks</h2>
  <ul>
    <li>
      <h3><a href="/walks/woodland">Woodland walk</a></h3>
      <p>A short loop through the woods.</p>
      <details>
        <summary>Route details for the woodland walk</summary>
        <p>The path is uneven after the bridge.</p>
      </details>
    </li>
    <li>
      <h3><a href="/walks/riverside">Riverside walk</a></h3>
      <p>A level route beside the river.</p>
      <details>
        <summary>Route details for the riverside walk</summary>
        <p>There are benches along the path.</p>
      </details>
    </li>
  </ul>
</section>
```

### Card content

- **Image.** Keep its proportions when the card resizes. Where a cropped preview uses `object-fit: cover`, check that the crop preserves the information described in the alternative text.
- **Video.** Let the viewer start playback. Provide captions for dialogue and use controls that work with a keyboard and screen reader. Apply the same checks to any extra play or pause button. See [Video Controls](Video%20Controls.md).
- **Quotation.** If the quotation has a caption, group them in a `figure`. Put the quotation, marked up as a `blockquote`, before its `figcaption`.

### Layout and enhancement

Start with a readable list and use [responsive grid layout](Layout.md#responsive-grids) where space permits. Keep actions in normal flow. With a flex column, an automatic margin before the action group can align actions across cards while allowing each card to grow. Keep visual order aligned with source and focus order wherever possible. If an image appears above its headline visually, keep the headline first in source order and check that keyboard focus still follows a predictable path.

If cards scroll horizontally, preserve their list structure and check that each item and its controls can be reached and brought into view with the keyboard. At narrow widths, let each card fit the viewport. Check that controls and the card boundary remain clear in forced colours.

Keep supplementary information available before an enhancement runs. If an enhanced design overlays the information on the card, keep the dismissal control visible and make overflowing content reachable at high zoom. Prefer an in-flow panel when a fixed overlay would obscure content or controls.

## Checkpoints for the Tester

### ACT-style checks

Rule identifier: `gel-components-cards`

Run these checks on a standalone card and a collection with at least two cards. Apply the collection checks to the grouped variant and the image check where an image is central to the item.

| Method and action | Expected result |
| --- | --- |
| Inspect the collection structure. Navigate by headings and list items with a screen reader. | A descriptive collection heading precedes a `ul`. Each peer card is a `li`. Card headings share a level one below the collection heading. |
| Inspect the markup and read from the start of each card with a screen reader. | Each card has a headline marked up as a heading. It is the first text encountered in the card, including text alternatives for media. |
| Read the alternative text of an image that is central to the card. Compare it with the image and headline. | The alternative conveys the relevant information from the image without just repeating the headline. |

### Variant and fallback checks

1. In a collection, use the keyboard to open and close the second card’s disclosure. Check that the label identifies the card, its panel is next in reading order and the other cards keep their state. When closed, the panel’s links and controls are skipped by Tab and screen reader navigation. If Escape is supported, press it from inside the open panel and check that focus returns to its trigger.
2. Open a modal from one card using the keyboard. Check that the content and accessible name identify the selected item. Dismiss it and check that focus returns to the opener, unless the action takes the user to a new page or step. Repeat from a different card.
3. Operate any save or share actions from two different cards. Verify that each action affects the intended item and that a toggle’s exposed pressed state matches its visible state. For cards with video, verify that playback waits for activation, captions are available for dialogue and the player controls can be reached and operated.
4. Block scripts and verify that readers can reach supplementary information. In a separate run with CSS disabled, verify that each headline, its content and its actions can still be read in order.
5. Increase text size and check for clipping or overlap. In a separate run, narrow the viewport. Check that cards fit, controls remain reachable and images retain their proportions. With forced colours enabled, check the focus indicator and any borders needed to distinguish cards or controls.

## Sources

- [Screen reader commands for testing](https://developer.paciellogroup.com/blog/2015/01/basic-screen-reader-commands-for-accessibility-testing/)
- [Definition of the expanded state](https://www.w3.org/WAI/PF/aria-1.1/states_and_properties#aria-expanded)
- [Accessibility issues with autoplay](https://abilitynet.org.uk/news-blogs/why-autoplay-accessibility-issue)
- [High contrast mode instructions](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
