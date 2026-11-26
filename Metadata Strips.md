---
title: Metadata Strips
description: Present supporting content details such as publication time, source, format and duration.
---

# Metadata Strips

A metadata strip, also called a **content metadata row** or **supporting details**, groups short facts that help people decide whether to open or use an item. For example, a reader might use the publication date to judge whether an article is still relevant, or the duration to decide whether they have time to listen. Use the strip alongside the content it describes, such as a [Promotional Card](Promotional%20Cards.md).

See [Metadata](Metadata.md) for document-level information such as the page title and language. See [Badge](Badge.md) for a short status, category or count that needs to stand out.

## Presentation

Place the details near the content they describe and use a consistent reading order. Make each value understandable in context. For example, distinguish ‘Published 1 minute ago’ from ‘Duration 1 minute’. Prefer visible words for a label or unit when an abbreviation or icon would be ambiguous.

Use spacing or a decorative border to separate entries. Avoid adding separator characters to the text that a screen reader reads. Let the group wrap at narrow widths and allow a long entry to wrap at high zoom or with enlarged text. Keep each label with its value in reading order.

## Behaviour

Most details are static. Where a value links somewhere, give the link wording that identifies its destination. Keep it distinguishable without colour, for example with an underline. If a link opens a new tab, give people advance notice. See [Anchors and Links](Anchors%20and%20Links.md#new-tabs) for new-tab behaviour and [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) for visual cues.

## Implementation

Use an unordered list for a set of short details. Use a description list for explicit term–value pairs, with a `dt` for the label and a `dd` for its value. Use `time` with a machine-readable `datetime` where useful. When showing a relative publication time, make the actual date available if people need it to assess the content.

### Markup example

This fragment shows three details for a recording. The date and destination are illustrative. Replace them with the content’s actual publication date and category URL. The example needs no script or stylesheet.

```html
<dl>
  <dt>Published</dt>
  <dd><time datetime="2026-06-09">9 June 2026</time></dd>
  <dt>Duration</dt>
  <dd><time datetime="PT4M">4 minutes</time></dd>
  <dt>Category</dt>
  <dd><a href="/topics/local-travel">Local travel</a></dd>
</dl>
```

### Compact variants

Use a decorative clock icon only when the accompanying words convey the same information. Follow [Icons](Icons.md) for keeping a decorative SVG out of the accessibility tree and tab order. If a compact design needs a visibly abbreviated value, first check how it is read. Where it needs a separate text equivalent, hide the visual duplicate from assistive technology and use [visually hidden text](Anchors%20and%20Links.md#link-content) for the full wording. Keep links and other focusable elements out of the hidden subtree.

If CSS removes list markers, verify that list semantics are still exposed in the supported browsers. Use wrapping and spacing to keep groups readable without relying on a border or a single line of text.

## Checkpoints for the tester

| Method and action | Expected result |
| --- | --- |
| Read the strip in context with a screen reader. Compare the publication time and duration with the visible values. | Each detail is heard once with its meaning and units. Decorative icons and separators add no extra speech. |
| Navigate by lists or read the term–value pairs in order. Repeat with any list markers suppressed. | The list relationship or term–value order is clear. Each value can be matched to its label and the item it describes. |
| Tab to any linked values. Inspect focus and link styling. Activate any link that opens a new tab. | Links are distinguishable without colour and their names identify their destinations. Any new-tab behaviour is indicated in advance. |
| Increase text size, then narrow the viewport in a separate run. Check a long value and repeat with forced colours. | Entries can wrap without clipping or overlap, and the details, links and focus remain clear. |

## Sources

- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Accessibility chops: `hidden` and `aria-hidden`](https://developer.paciellogroup.com/blog/2012/05/html5-accessibility-chops-hidden-and-aria-hidden/)
- [Understanding Success Criterion 1.4.1: Use of Color](https://www.w3.org/WAI/WCAG21/Understanding/use-of-color)
