---
title: Functional Alternative Text
description: Convey the information or purpose of an image in the context where people encounter it.
---

# Functional Alternative Text

Help people understand the page and operate its controls when they cannot see an image or it fails to load. Choose the alternative by asking what the image contributes in that context. The same photograph may need a description in a gallery and an empty alternative beside text that already supplies its meaning.

## Guidance

| What the image contributes | What to provide |
| --- | --- |
| Decoration or information already available in the same context | Give an HTML image an empty alternative, `alt=""`. Check that removing the image leaves the information and any control name intact. |
| Information people need | Give the image a concise alternative that explains what matters to the task. For example, describe an entrance’s steps if people are checking how to enter the building. |
| The only label for a link or button | Name the destination or action. For example, use ‘Search’ for an image-only search button. |
| Meaningful words shown as part of the image | Prefer real text where possible. If the image is needed, provide the words people need in the alternative or in clearly associated text. |
| Detailed information that needs more space | Give the image a short identification and put the relevant explanation or data in visible text nearby. See [Infographics](Infographics.md) for charts and diagrams. |

Write the alternative in the language of the surrounding content. Keep it brief enough to understand in context and include the detail needed to act. If a caption describes the subject but the image also conveys useful information, use the alternative to supply that information. Check what someone hears when navigating directly to the image as well as when reading the whole section.

## Implementation

### Decorative and informative images

Use the `alt` attribute on HTML images. An empty value intentionally leaves the image out of screen reader reading order. An omitted attribute leaves browsers and assistive technology to decide what to expose, which can include a file name. When using a CSS background, make any necessary meaning available through readable page text. Check that the text still works when the background is removed.

These examples use illustrative image paths. Supply the corresponding project assets.

```html
<img src="divider.svg" alt="">
<img src="entrance.jpg" alt="Three steps lead up to the main entrance.">
```

### Images in controls

If a link or button already has visible text that identifies its purpose, give a redundant image inside it an empty alternative. Where the image provides the entire label, give it an alternative that describes the destination or action. When an icon provides the label instead of an HTML image, use the [accessible name guidance for icons](Icons.md#icons-and-accessible-names). Prefer a visible text label when people need the action to stay apparent if the icon fails to load.

```html
<a href="/"><img src="site-logo.svg" alt="Home"></a>
<button type="submit">
  <img src="search.svg" alt="">
  Search
</button>
```

The link uses the sample home route. Place the submit button in the form it operates.

### Complex graphics and inline SVG

For a chart or diagram, put the conclusion, reading instructions or data people need in an adjacent section or a clearly identified link. A short `alt` can identify the chart and point readers towards the fuller explanation. Keep structured material, such as a data table, available through normal reading and navigation. Use `aria-describedby` when a short associated description helps, and verify how it is presented in supported combinations. See [Infographics](Infographics.md) and [Data Tables](Data%20Tables.md).

For a meaningful inline SVG that stands on its own, use `role="img"` and give it an accessible name. For example, use `aria-labelledby` to reference its `title` by a unique ID. Keep any longer description available in nearby text. If the SVG is decorative inside a named control, follow [Icons](Icons.md) for keeping it out of assistive technology and keyboard navigation.

## Checks

Record the image, context, method and result. Use automated checks to find missing `alt` attributes on HTML images and broken references. Review the purpose and wording manually.

| Method and action | Expected result |
| --- | --- |
| Read the section with a screen reader. Compare the meaning with the same section viewed with images available. | The meaningful information is available and follows a coherent reading order. Decorative images are skipped. Nearby text and the alternative complement each other. |
| Navigate by links and buttons. Listen to each name without reading the surrounding paragraph. | The destination or action is clear. An image that duplicates a visible control label adds no extra words to the name. |
| Navigate to an infographic or complex image. Find its explanation or data and use it to answer a question the graphic supports. | The alternative identifies the graphic and the accompanying text or data lets the reader complete the same task. |
| Block images, including CSS backgrounds, and try the task again. | Necessary information and controls remain understandable and usable. |
| Check each supported language and any image reused in different contexts. | The alternative is clear, matches the content language and serves the purpose of the image in that placement. |

## Sources

- [HTML text alternative mechanisms](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML#other_text_alternative_mechanisms)
- [Visually hidden content](https://webaim.org/techniques/css/invisiblecontent/)
- [Accessible SVG techniques](https://www.sitepoint.com/tips-accessible-svg/)
