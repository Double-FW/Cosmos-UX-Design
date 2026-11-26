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

On Windows, test with a system contrast theme such as Windows High Contrast (WHCM). On mobile, test system light and dark appearance separately from accessibility settings such as increased contrast or colour inversion, where the device offers them. Some settings change the final display without exposing a matching CSS media query. Check the result on a supported device.

## Implementation


### Choosing a theme

When a service offers a theme control, give people **System**, **Light** and **Dark** options. Default to System so the page follows the device preference. Remember an explicit choice across pages and reloads where storage is available. Selecting System clears that override and resumes following device changes. Let forced colours continue to apply in every theme.

Use a visibly labelled native select or radio group. Make the selected option available to assistive technology. Keep focus on the control when the theme changes and keep the current page and task state available.

This example sets page text and background colours and uses `color-scheme` so browser-provided controls can match. It follows the device when scripts are blocked. The script reveals the select after it can apply a change. In a host service, apply a saved choice before the first paint where practical, then use the same theme tokens for links, controls and other surfaces.

```html
<style>
  :root {
    color-scheme: light;
    --page-text: #1b1b1b;
    --page-background: #fff;
  }

  @media (prefers-color-scheme: dark) {
    :root:not([data-theme]) {
      color-scheme: dark;
      --page-text: #f4f4f4;
      --page-background: #171717;
    }
  }

  :root[data-theme="dark"] {
    color-scheme: dark;
    --page-text: #f4f4f4;
    --page-background: #171717;
  }

  body {
    color: var(--page-text);
    background: var(--page-background);
  }
</style>

<div id="theme-control" hidden>
  <label for="theme">Colour theme</label>
  <select id="theme">
    <option value="system">System</option>
    <option value="light">Light</option>
    <option value="dark">Dark</option>
  </select>
</div>

<script>
  const themeSelect = document.getElementById('theme');
  const root = document.documentElement;
  const storageKey = 'site-colour-theme';

  function applyTheme(theme) {
    if (theme === 'system') {
      delete root.dataset.theme;
    } else {
      root.dataset.theme = theme;
    }
  }

  let savedTheme;
  try {
    savedTheme = localStorage.getItem(storageKey);
  } catch {
    // System is the initial choice if storage is unavailable.
  }

  themeSelect.value = ['light', 'dark'].includes(savedTheme) ? savedTheme : 'system';
  applyTheme(themeSelect.value);

  themeSelect.addEventListener('change', () => {
    const theme = themeSelect.value;
    applyTheme(theme);
    try {
      if (theme === 'system') {
        localStorage.removeItem(storageKey);
      } else {
        localStorage.setItem(storageKey, theme);
      }
    } catch {
      // The choice still applies to the current page.
    }
  });

  document.getElementById('theme-control').hidden = false;
</script>
```

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

For functional graphics, see [SVGs that adapt to colour changes](Icons.md#svgs-that-adapt-to-colour-changes).

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
| **Manual.** On a supported mobile device, switch between system light and dark appearance. Test increased contrast and colour inversion separately where offered. | Text, controls, images and meaningful states remain usable. The service responds to preferences the browser exposes. Other display changes leave the content understandable. |
| **Manual.** With the service set to System, change the device appearance while the page is open. Select Light or Dark and change the device appearance again. Reload, visit another page, then select System. | System follows the device. An explicit choice stays selected and is remembered when storage is available. Returning to System clears the override. The control keeps focus during changes and forced colours still apply. |
| **Manual.** Block scripts, then test again with scripts enabled and storage unavailable. | The script-free page follows the system palette and shows no inactive selector. With scripts enabled, switching still works for the current page. |

## Sources

- [Contrast checker](https://webaim.org/resources/contrastchecker/)
- [Desktop colour-vision simulator](https://michelf.ca/projects/mac/sim-daltonism/)
- [Browser colour-vision simulator](https://chrome.google.com/webstore/detail/colorblindly/floniaahmccleoclneebhhmnjgdfijgg?hl=en)
