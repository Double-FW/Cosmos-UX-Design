---
title: Colour, contrast and meaning
description: Choose colour combinations that people can read and use, including when they apply their own colours.
---

# Colour, contrast and meaning

Use colour to help people find links, recognise states and compare information. Give them another way to understand the same information when they cannot distinguish the colours or when their device replaces them. Check that text and controls remain legible against the backgrounds people see.

## Guidance

### Use of colour

Under Web Content Accessibility Guidelines (WCAG) 2.1 [1.4.1 Use of Color](https://www.w3.org/TR/WCAG21/#use-of-color) (Level A), people must be able to distinguish meaningful information, actions and states using a visual cue in addition to hue when hue alone would convey the distinction.

- **Links.** Prefer an underline for inline links. When using lightness as the initial distinction, the [inline link contrast technique](https://www.w3.org/WAI/WCAG21/Techniques/general/G183) uses at least 3:1 between link text and adjacent body text. Each text colour still needs sufficient contrast against the background. Provide a visible treatment on keyboard focus and pointer hover.
- **States.** Pair coloured borders or icons with meaningful visible text. For example, say ‘Enter a gift code using letters and numbers only’ instead of asking people to correct characters highlighted in red. Associate the message with the field using the [form validation guidance](Forms.md#validation). Use shape as an additional cue when an icon changes state. See [Icons](Icons.md).
- **Prices.** Write ‘Was £48. Now £36’ so the sequence makes sense when read aloud or on a braille display. Strikethrough, text size and colour can reinforce the visible labels.
- **Charts.** Distinguish series with direct labels, patterns, marker shapes or line styles. Give readers a text or data alternative where appropriate. See [Infographics](Infographics.md).

### Contrast

Measure the actual foreground and background pair. Similar shades can lose their distinction on a low-contrast display or in glare. Check text over the least favourable part of a gradient or image and account for transparency.

| WCAG 2.1 criterion | What to measure |
| --- | --- |
| [1.4.3 Contrast (Minimum)](https://www.w3.org/TR/WCAG21/#contrast-minimum), Level AA | At least 4.5:1 for ordinary text and 3:1 for large-scale text against its background, subject to the criterion’s exceptions. For size thresholds and typography, see [Text contrast](Typography.md#text-contrast). |
| [1.4.11 Non-text Contrast](https://www.w3.org/TR/WCAG21/#non-text-contrast), Level AA | At least 3:1 against adjacent colours for visual information needed to identify a control or its state and for graphics needed to understand content, subject to the criterion’s exceptions. Check a custom focus indicator where it identifies the focused state. See [Focus indication](Focus.md#focus-indication) for focus-specific checks. |

### Palette and tokens

Give palette shades a consistent naming scheme so people can find related values. Record the measured contrast for approved foreground and background pairs. Treat the shade name as a lookup aid and use the measured ratio to approve a pairing.

Define semantic tokens for roles such as body text, links, surfaces, focus and error. Document which tokens work together in each theme and state. For example, record the combinations used for error text and an error border separately, because their contrast is measured against different adjacent colours and can have different thresholds. Retest the affected pairs when a token changes.

### User colour and contrast preferences

Some people use a dark theme, higher or lower contrast, or particular foreground and background colours. Use `prefers-color-scheme` or `prefers-contrast` where you offer corresponding themes. Test each supported theme against the same content and control states.

Forced colours can replace authored colours and suppress treatments such as box shadows. Keep text, icons, control boundaries and focus visible when that happens. Prefer native elements for their semantics and browser styling. A link with a button role can still receive the browser’s forced-colour styling for a link.

## Implementation

### Focus and icons in forced colours

Use the [Focus guide](Focus.md#consider-high-contrast-mode) for focus styling. If the normal design uses a shadow for focus, keep a real outline available in forced colours. Use `currentColor` for the fill or stroke of a single-colour SVG icon when it should follow surrounding text. If the icon duplicates a visible label, hide it from assistive technology.

```css
@media (forced-colors: active) {
  .action:focus-visible {
    outline: 2px solid Highlight;
    outline-offset: 2px;
  }
}
```

This CSS fragment assumes `.action` is a focusable control with a visible focus style in the default theme. The outline width is an example. Check the result with the user’s actual palette. Inspect borders that are transparent in the default theme because a forced palette can make them visible.

### When the colour is the content

For a colour picker, preserve the sample inside the swatch if the colour itself is what people are choosing. If needed, apply [`forced-color-adjust: none`](https://developer.mozilla.org/en-US/docs/Web/CSS/forced-color-adjust) to the swatch alone. Leave the surrounding label, input and focus indication responsive to the user’s colours. Provide a visible shade name and an independently identifiable selected state.

## Checks

Record the page, theme, component, state, method and result. For measurements include the sampled colours, the ratio and the applicable criterion. For manual checks include the browser and relevant settings.

| Method and action | Expected result |
| --- | --- |
| **Assisted.** Measure foreground and adjacent background colours for text, links, control boundaries, meaningful graphics and custom focus states. Repeat for supported themes and interaction states. | Each applicable pair meets its threshold. Text over gradients, images or partially transparent layers meets it where the text appears. |
| **Manual.** Review links, forms, status messages and charts in grayscale. Try a colour-vision simulation to identify further cases for inspection. Trigger error, warning and success states. | The action, state and recovery steps are distinguishable using visible labels or other non-colour cues. Chart series can be matched to their labels. |
| **Manual.** Read an error message, a price change and a chart alternative with a supported screen reader. | The field and recovery instruction, the previous and current prices, and the chart’s relevant information make sense without visual styling. |
| **Manual.** Turn on a system contrast theme and Tab through links, buttons, inputs and any swatch selector. Repeat with a different palette where supported. | Focus, text, boundaries, icons and selected states remain perceivable. The swatch remains identifiable by name, with its colour visible where preservation is intentional. |
| **Manual.** Switch between supported light, dark and contrast preferences. Repeat on a screen with text over media. | Controls and text remain usable, including where the background varies. Applying a user preference still leaves a clear indication of state. |

## Sources

- [Contrast checker](https://webaim.org/resources/contrastchecker/)
- [Desktop colour-vision simulator](https://michelf.ca/projects/mac/sim-daltonism/)
- [Browser colour-vision simulator](https://chrome.google.com/webstore/detail/colorblindly/floniaahmccleoclneebhhmnjgdfijgg?hl=en)
