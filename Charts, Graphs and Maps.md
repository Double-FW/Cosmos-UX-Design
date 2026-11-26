---
title: Charts, Graphs and Maps
description: Help people find, compare and act on information presented in charts, graphs and maps.
---

# Charts, Graphs and Maps

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing notes](#testing-notes)
- [Sources](#sources)

## Introduction

Start with what someone needs to find out or do. Use a chart to show a pattern, a table to compare exact values and a map to show where things are or how they relate in space. A sentence or list may answer a simple question more directly. A person who cannot use the graphic still needs enough information and control to do the task.

| User task | What to provide |
| --- | --- |
| Understand a trend or change | A clear title, the main finding and any exception or caveat that affects interpretation. |
| Compare or explore values | Structured data with labels, units and the relevant filters or selection. Keep more detail available than the headline conclusion. |
| Find a place or follow a route | Searchable locations or ordered directions, with the same relevant destinations, route options, landmarks and constraints as the map. |
| Explore a boundary or spatial relationship | A way to answer the relevant spatial question, such as which places fall inside an area or connect along a route. Include the boundary rule and uncertainty where these affect the decision. |

## Recommended markup

- Give the graphic a descriptive heading or caption. Name the measure, place, time period and units where they affect interpretation. Explain missing, suppressed, estimated or provisional values in text.
- For an informative image, use a short text alternative that identifies it and place the explanation or structured data nearby. Use [Functional Alternative Text](Functional%20Alternative%20Text.md#complex-graphics-and-inline-svg) for the graphic and [Infographics](Infographics.md) for editorial visuals. When adjacent content already gives the same information, mark the redundant graphic as decorative and keep its surrounding text available.
- Use [Data Tables](Data%20Tables.md) for exact values. Provide a caption and meaningful row and column headers. For destinations use a list with descriptive names and links. For routes use an ordered list with start, end, turns and relevant obstacles or access conditions. Keep meaningful relationships in the HTML so people can navigate them at their own pace.
- Use native labelled inputs, buttons and links for search, filtering, zoom, reset and selection. Give location controls specific names, such as ‘Show Riverside Library’. Keep interactive controls outside any graphic hidden from assistive technology.

## Recommended layout

Put the title, context and key finding where people encounter them with the graphic. Keep the table, directions or link to further detail close by, with a descriptive label. Place filters before the results they change. Let supporting content reflow around any chart or map that needs space to preserve its meaning.

Use direct labels, line styles, marker shapes or patterns so people can identify data without relying on colour. Keep labels and units readable at narrow widths and when text is enlarged. Preserve a meaningful scale when the layout or filter changes, and explain any change that affects comparison. Use [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) for contrast and forced colours.

## Recommended behaviour

### Let people explore and act

Make values and actions available without hover. If a map supports pan or zoom, offer ordinary controls for the relevant directions, zoom and reset. Provide click or tap alternatives to authored dragging as well as keyboard access. Keep page zoom available separately from the map’s zoom. Where locations depend on device permission, offer a manual search or address entry.

Let people use the list or directions to reach the same details and actions as on the map. Keep selection and filters consistent across presentations. Use a persistent detail area where someone needs to compare values or reread an action. Keep focus on the control that changed the view when it remains useful. If an update removes the focused item, move focus to the nearest meaningful control or result heading.

### Keep every presentation in sync

Use one applied set of data, filters and selections for the graphic, explanation, structured details and any download. Show loading, empty, failed and stale states plainly. Keep any earlier results identifiable as such if you retain them during an update. Use [persistent status feedback](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback) for meaningful results and errors. Avoid announcing every mark or point as the view changes.

Keep the summary, data or route usable when scripts, map tiles or other enhancements fail. See [Progressive Enhancement](Progressive%20Enhancement.md).

## Reference implementation

### Filtered chart with a table baseline

Copy this example into an HTML file. The table is the single source for the bars and summary. Without scripts, it shows both periods. Enhancement adds the selector and filters all three presentations together. The visual bars repeat the table, so they are hidden from assistive technology. The fixed scale is 0 to 30 bookings.

```html
<!doctype html>
<html lang="en-GB">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Bookings by channel</title>

<style>
  .booking-chart { max-inline-size: 30rem; margin-inline: 0; }
  .bar {
    display: block;
    min-block-size: 0.75rem;
    border-block-end: 0.5rem solid currentColor;
  }
  th, td { padding: 0.25rem 0.5rem; text-align: start; }
</style>

<main>
  <h1>Bookings by channel</h1>
  <p id="period-control" hidden>
    <label for="period">Booking period</label>
    <select id="period">
      <option value="spring">Spring</option>
      <option value="summer">Summer</option>
    </select>
  </p>

  <p id="booking-summary" role="status"></p>

  <div id="booking-chart" class="booking-chart" aria-hidden="true" hidden>
    <p>Scale: 0 to 30 bookings</p>
    <div id="booking-bars"></div>
  </div>

  <table>
    <caption id="booking-caption">Booking counts by period and channel</caption>
    <thead>
      <tr>
        <th scope="col">Period and channel</th>
        <th scope="col">Bookings</th>
      </tr>
    </thead>
    <tbody id="booking-data">
      <tr data-period="spring"><th scope="row">Spring online</th><td>24</td></tr>
      <tr data-period="spring"><th scope="row">Spring by phone</th><td>12</td></tr>
      <tr data-period="summer"><th scope="row">Summer online</th><td>30</td></tr>
      <tr data-period="summer"><th scope="row">Summer by phone</th><td>18</td></tr>
    </tbody>
  </table>
</main>

<script>
  const period = document.getElementById('period');
  const rows = [...document.querySelectorAll('#booking-data tr')];
  const bars = document.getElementById('booking-bars');
  const summary = document.getElementById('booking-summary');
  const caption = document.getElementById('booking-caption');
  const scaleMax = 30;

  function showPeriod() {
    const selected = period.selectedOptions[0].textContent;
    const visibleRows = rows.filter(row => row.dataset.period === period.value);
    const values = visibleRows.map(row => Number(row.cells[1].textContent));

    const marks = visibleRows.map((row, i) => {
      const mark = document.createElement('p');
      mark.textContent = row.cells[0].textContent + ': ' + values[i];

      const bar = document.createElement('span');
      bar.className = 'bar';
      bar.style.inlineSize = (100 * values[i] / scaleMax) + '%';
      mark.append(bar);
      return mark;
    });

    rows.forEach(row => { row.hidden = row.dataset.period !== period.value; });
    bars.replaceChildren(...marks);
    caption.textContent = 'Booking counts by channel in ' + selected;
    summary.textContent = selected + ': ' + values[0] + ' online bookings and ' +
      values[1] + ' phone bookings.';
  }

  period.addEventListener('change', showPeriod);
  showPeriod();
  document.getElementById('booking-chart').hidden = false;
  document.getElementById('period-control').hidden = false;
</script>
```

This fixture has two known channels in each period. In a real service, use stable identifiers and handle absent or suppressed values explicitly. Choose a scale that fits the available values, then use that same scale for views intended for comparison.

### Route map with directions

This HTML fragment shows a fictitious route. Insert it into a page with a suitable heading level. The graphic repeats the start, turn and destination in the directions. For a real route, include the distances, access conditions and uncertainty people need, and keep the map and directions up to date together.

```html
<section>
  <h2>From the entrance to reception</h2>
  <svg viewBox="0 0 360 115" width="360" height="115"
    style="max-inline-size: 100%; block-size: auto"
    aria-hidden="true" focusable="false">
    <path d="M 30 80 H 190 V 32" fill="none" stroke="currentColor" stroke-width="4"/>
    <circle cx="30" cy="80" r="5" fill="currentColor"/>
    <circle cx="190" cy="80" r="5" fill="currentColor"/>
    <circle cx="190" cy="32" r="5" fill="currentColor"/>
    <g fill="currentColor" font-size="16">
      <text x="12" y="105">Entrance</text>
      <text x="130" y="105">Information desk</text>
      <text x="204" y="38">Reception</text>
    </g>
  </svg>
  <ol>
    <li>Start at the main entrance.</li>
    <li>Walk straight ahead to the information desk.</li>
    <li>Turn left at the desk. Reception is at the end of that corridor.</li>
  </ol>
</section>
```

## Testing notes

Use the task identified at the start of design. Record the view, selected data, supported browser and assistive technology where relevant. Use a fixture with an outlier, a missing value or an unavailable route where it reflects the service.

| Method and action | Expected result |
| --- | --- |
| From the graphic and then from the structured alternative, identify the main finding and answer a follow-up question about a value or relationship. | Both presentations support the intended task. Units, caveats and relevant exceptions are available. |
| Change filters or the selected location. Compare the graphic, summary, structured details and any export. Repeat with an empty or failed result. | The presentations reflect the same scope and values. The result is clear, and stale data is identified if retained. |
| Use keyboard and a supported screen reader to reach details, reset or select an item and follow its action. Repeat with touch without dragging. | Controls have usable names and predictable focus. Essential details and actions are available without hover or a gesture. |
| Enlarge text, then test narrow width and forced colours separately. | Supporting text and controls reflow. Labels, series and focus remain distinguishable. |
| Find a location or follow a route using the alternative. Try without scripts, map tiles or location permission where applicable. | The route or location can still be found through the baseline. Relevant constraints and the route to recovery remain available. |

Automate agreement between displayed values and filter state where possible. Use assistive technology and task testing to check whether people can interpret the information and complete the intended action.

## Sources

- [Double-FW Inclusive Design of Charts, Graphs, Maps and Dynamic Complex Graphics](https://github.com/Double-FW/Accessible_Charts_Graphs_and_Maps/blob/16f4d7c77200c129fe0ed81c1f815e283d6ebb1d/Inclusive_Design_Charts_Maps_and_Complex_Graphics.md)
- [W3C WAI: Complex Images](https://www.w3.org/WAI/tutorials/images/complex/)
- [Government Analysis Function: Publishing charts](https://analysisfunction.civilservice.gov.uk/support/communicating-analysis/introduction-to-data-visualisation-e-learning/module-10-publishing-charts/)
- [W3C WCAG guidance: Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html)
