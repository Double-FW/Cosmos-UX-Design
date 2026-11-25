---
title: Metadata Strips
description: "Reading supporting details."
---

# Metadata Strips

Group related details such as duration, format and category in a list. Include a text label where a number or icon would be ambiguous. When visually abbreviated and expanded versions coexist, expose each fact once to assistive technology. Use decorative separators and keep links distinguishable.

## Implementation

Use a list for a series of details, or a description list for explicit term–value pairs. A duration displayed as ‘4 min’ should retain an intelligible unit when read aloud. Hide a decorative clock icon when text supplies the same information.

### Markup example

This example illustrates the semantic structure.

```html
<dl>
  <dt>Format</dt><dd>Audio</dd>
  <dt>Duration</dt><dd>Four minutes</dd>
</dl>
```

## Checkpoints for the tester

1. Read the strip in sequence, then navigate its links.
2. Verify the meaning and units of each value, that separators add no spoken clutter and that each linked item identifies its destination.

## Related references

- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Accessibility chops: `hidden` and `aria-hidden`](https://developer.paciellogroup.com/blog/2012/05/html5-accessibility-chops-hidden-and-aria-hidden/)
- [Understanding Success Criterion 1.4.1: Use of Color](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color)
