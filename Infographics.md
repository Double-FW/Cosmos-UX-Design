---
title: Infographics
description: Help people understand a graphic’s main finding and explore the information behind it.
---

# Infographics

Use an infographic when a visual comparison helps people see a pattern, trend or relationship. Give people a way to understand the same finding when they cannot see the graphic, distinguish its colours or operate an interactive feature.

## Presentation

Identify the subject, time period and units. Label axes and categories clearly and explain any scale or grouping that changes how people should interpret the values. Start with a design that works in monochrome. Add colour alongside direct labels, marker shapes, line styles or patterns. Use the same cue in the graphic and its key. Follow [Colour and Meaning](Colour%20and%20Meaning.md) for contrast and forced colours.

Give an informative graphic a concise text alternative that identifies it and explains what people need to know about its form. Use a visible caption to convey the main finding. If the graphic’s arrangement carries meaning, describe it. For example, explain that each bar represents a month and is divided by travel mode if that relationship matters to the task. Keep the image alternative and caption complementary.

## Behaviour

Offer the format that lets people answer their question. A short summary may be enough for a simple diagram. Use a [data table](Data%20Tables.md) where readers need to look up exact values or make comparisons. Include the same units, time periods and categories in both views. Make the table easy to find from the graphic. If the views are separated on the page, provide a way back.

Prefer a static graphic when viewing the whole comparison is enough. If readers can reveal values, filter data or change the view, make those actions operable by keyboard and touch. Keep the alternative and any data view in sync with the displayed data. Make details available without relying on hover. If the graphic requires scripts or fails to load, keep the summary and data available.

## Implementation

Prefer a scalable graphic such as SVG when detail needs to stay clear at zoom. Give an informative inline SVG that represents a single image an accessible name. For an SVG loaded with `img`, use the `alt` attribute instead. See [Functional Alternative Text](Functional%20Alternative%20Text.md). Reserve space for externally loaded graphics so surrounding content stays in place as they load.

Use a `figure` and `figcaption` to group the graphic and its visible finding. Use `aria-describedby` for a short description when it helps to associate the explanation with the graphic, and verify this in your supported browser and screen reader combinations. Keep longer explanations and tables independently navigable in the page.

### Markup example

This standalone fragment compares walking trips in two months. The SVG has a short name, the caption explains the change and the table supplies the exact values. The data is illustrative. The fragment needs no script or external asset.

```html
<figure id="walking-trips-chart">
  <svg role="img" aria-labelledby="walking-trips-name"
       viewBox="0 0 320 130" width="320" height="130"
       style="max-inline-size: 100%; height: auto;">
    <title id="walking-trips-name">Bar chart of walking trips in May and June</title>
    <g fill="currentColor">
      <text x="8" y="20">Walking trips</text>
      <text x="8" y="54">May</text>
      <rect x="65" y="38" width="120" height="22"></rect>
      <text x="193" y="54">60</text>
      <text x="8" y="94">June</text>
      <rect x="65" y="78" width="180" height="22"></rect>
      <text x="253" y="94">90</text>
    </g>
  </svg>
  <figcaption>Walking trips increased by 50% from May to June.</figcaption>
</figure>
<p><a href="#walking-trips-data">View walking trip data</a></p>

<h2 id="walking-trips-data" tabindex="-1">Walking trip data</h2>
<table>
  <caption>Walking trips by month</caption>
  <thead>
    <tr><th scope="col">Month</th><th scope="col">Trips</th></tr>
  </thead>
  <tbody>
    <tr><th scope="row">May</th><td>60</td></tr>
    <tr><th scope="row">June</th><td>90</td></tr>
  </tbody>
</table>
```

## Checkpoints for the tester

Test the presentations and controls the project uses. Record the browser, assistive technology and outcome.

| Method and action | Expected result |
| --- | --- |
| Read the named graphic, caption and nearby explanation with a screen reader. Answer a question about the main finding without looking at the graph. | The graphic is identifiable and the finding is clear. The explanation supplies any relationships the user needs to understand it. |
| Follow the link to the table and check a value from each series or category. Compare units and periods with the graphic. | The data can be found and compared using its headers. The values and scope agree across formats. |
| Inspect the graphic in grayscale and forced colours. Follow each series from its key to a value. | Labels, shapes or patterns distinguish each series without hue. Required text and graphical marks remain legible. |
| Increase text size and test a narrow viewport separately. Zoom in to inspect labels and values. | Relevant labels and values remain visible and readable, or the adjacent equivalent provides the same information. |
| Where controls or tooltips are provided, try them with the keyboard and touch. Change a filter and compare the graphic with its alternative. | The same values can be obtained without hover. Focus remains usable and the formats reflect the same filter. |
| Block scripts or the image asset where applicable. Locate the main finding and an exact value from the alternative. | The summary and data remain available. |

## Sources

- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Offer choice](https://inclusivedesignprinciples.org/#offer-choice)
- [Preventing Content reflows from Lazy-Loaded Images](https://css-tricks.com/preventing-content-reflow-from-lazy-loaded-images/)
- [HTML5: Techniques for providing useful text alternatives](https://w3c.github.io/alt-techniques/with-pictures.html)
- [How do you figure?](https://www.scottohara.me/blog/2019/01/21/how-do-you-figure.html)
- [WCAG2.1 1.4.1: Use Of Color](https://www.w3.org/TR/WCAG21/#x1-4-1-use-of-color)
- [Accessible SVG Line Graphs](https://tink.uk/accessible-svg-line-graphs/)
