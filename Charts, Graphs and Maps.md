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

### Choose how to render a chart

Treat a static SVG as a single named image when people need to identify the graphic as a whole. Keep its explanation and structured data in nearby HTML. If the surrounding HTML already conveys the same information, hide the duplicate graphic from assistive technology.

Canvas draws the visual display. Provide its labels, values and relationships through HTML that people can navigate independently. Put filters, legends that change the view, and drill-down actions in native controls where practical. Keep data points out of the Tab sequence when they only represent values. Test the actual output of any chart library and version you plan to use.

## Recommended layout

Put the title, context and key finding where people encounter them with the graphic. Keep the table, directions or link to further detail close by, with a descriptive label. Place filters before the results they change. Let supporting content reflow around any chart or map that needs space to preserve its meaning.

Use direct labels, line styles, marker shapes or patterns so people can identify data without relying on colour. Keep labels and units readable at narrow widths and when text is enlarged. Preserve a meaningful scale when the layout or filter changes, and explain any change that affects comparison. Use [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) for contrast and forced colours.

Start quantitative bar axes at zero so differences in length represent differences in value. If a different baseline is necessary, explain it clearly and consider whether another chart would show the comparison more honestly. In a line chart, show gaps for missing observations and explain them. Use a distinct style and label for any estimate that bridges a gap.

## Recommended behaviour

### Let people explore and act

Make values and actions available without hover. If a map supports pan or zoom, offer ordinary controls for the relevant directions, zoom and reset. Provide click or tap alternatives to authored dragging as well as keyboard access. Keep page zoom available separately from the map’s zoom. Where locations depend on device permission, offer a manual search or address entry.

Let people use the list or directions to reach the same details and actions as on the map. Keep selection and filters consistent across presentations. Use a persistent detail area where someone needs to compare values or reread an action. Keep focus on the control that changed the view when it remains useful. If an update removes the focused item, move focus to the nearest meaningful control or result heading.

### Design dense and embedded maps

Start with a searchable location list when people need to find a service among many markers. Offer the same location details and actions from the list and map. Group nearby markers when that helps sighted users scan the map. Give each interactive cluster a name that includes its area and count, such as ‘Riverside: 2 locations’. Make it operable with a native button and let people narrow the list without tabbing through every marker. Explain what selecting a cluster does. Keep focus on its control when possible, and update the result count and scope together.

Give each map iframe a descriptive `title`. Verify that keyboard users can enter, use and leave the embedded map. Test its controls, popups, consent notices and full-screen mode where present. Keep the searchable results or directions on the host page so people can use them when the embed is blocked or unsuitable. A title identifies the frame. Access to the contents needs its own checks.

### Write indoor directions alongside the map

Name the entrance, floor and destination, then give one direction at a time between landmarks. Describe access conditions that affect a route decision, such as steps, staffed entrances, lift access and temporary closures. If a floor control changes the map, update the corresponding list or directions. Include the source and last verification date where changing conditions affect whether a route can be used. If a lift is out of service, identify the affected route and offer a verified alternative where available.

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

### Searchable locations with cluster controls

This standalone fixture represents two areas schematically. The HTML location list contains all results before enhancement. Searching by name or area updates both cluster counts and the list. Selecting a cluster narrows the list to that area. In a real map, position the clusters from the same location data and keep the list’s details and actions available on the host page.

```html
<!doctype html>
<html lang="en-GB">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Find a library</title>
<style>
  .clusters { display: flex; flex-wrap: wrap; gap: 0.75rem; }
  .clusters button { min-block-size: 2.75rem; border: 1px solid currentColor; }
  :focus-visible { outline: 2px solid currentColor; outline-offset: 2px; }
</style>
<main>
  <h1>Find a library</h1>
  <form id="library-search" hidden>
    <label for="library-query">Library name or area</label>
    <input id="library-query" type="search">
    <button type="submit">Search libraries</button>
  </form>

  <section id="library-clusters" aria-labelledby="area-heading" hidden>
    <h2 id="area-heading">Browse by area</h2>
    <p>Choose an area to narrow the results.</p>
    <div class="clusters">
      <button type="button" data-cluster="Riverside" aria-controls="libraries">
        Riverside
      </button>
      <button type="button" data-cluster="Town centre" aria-controls="libraries">
        Town centre
      </button>
    </div>
    <p><button type="button" id="all-areas" aria-controls="libraries">
      Show results from all areas
    </button></p>
  </section>

  <h2>Library results</h2>
  <p role="status" id="library-status"></p>
  <ul id="libraries">
    <li data-area="Riverside">Mill Library, Riverside. 8 Mill Road.</li>
    <li data-area="Riverside">Quay Library, Riverside. 12 Quay Street.</li>
    <li data-area="Town centre">Market Library, Town centre. 3 Market Street.</li>
  </ul>
</main>
<script>
  const form = document.getElementById('library-search');
  const query = document.getElementById('library-query');
  const clusterPanel = document.getElementById('library-clusters');
  const clusters = [...clusterPanel.querySelectorAll('[data-cluster]')];
  const locations = [...document.querySelectorAll('#libraries li')];
  const status = document.getElementById('library-status');
  let appliedQuery = '';
  let area = '';

  function locationCount(count) {
    return count + (count === 1 ? ' location' : ' locations');
  }

  function updateResults() {
    const matches = locations.filter(location =>
      location.textContent.toLowerCase().includes(appliedQuery)
    );

    clusters.forEach(button => {
      const name = button.dataset.cluster;
      const count = matches.filter(location => location.dataset.area === name).length;
      button.textContent = name + ': ' + locationCount(count);
      button.disabled = count === 0;
    });

    const visible = matches.filter(location => !area || location.dataset.area === area);
    locations.forEach(location => { location.hidden = !visible.includes(location); });
    status.textContent = visible.length
      ? locationCount(visible.length) + (area ? ' in ' + area + '.' : ' across all areas.')
      : 'No matching libraries. Try a different name or area.';
  }

  form.addEventListener('submit', event => {
    event.preventDefault();
    appliedQuery = query.value.trim().toLowerCase();
    area = '';
    updateResults();
  });

  clusters.forEach(button => button.addEventListener('click', () => {
    area = button.dataset.cluster;
    updateResults();
  }));

  document.getElementById('all-areas').addEventListener('click', () => {
    area = '';
    updateResults();
  });

  updateResults();
  form.hidden = false;
  clusterPanel.hidden = false;
</script>
```

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
| Search by place name, choose a cluster, return to all areas and try an empty search. For embedded maps, repeat with the embed unavailable. | Counts match the applied search, the chosen area is clear, focus stays usable and locations remain available through the host page. |
| Follow an indoor route on another floor, then simulate a lift closure. | The directions and map identify the affected route, give the same current access information and show any verified alternative. |

Automate agreement between displayed values and filter state where possible. Use assistive technology and task testing to check whether people can interpret the information and complete the intended action.

## Sources

- [W3C technique H64: Naming an iframe](https://www.w3.org/WAI/WCAG22/Techniques/html/H64)
- [ACCESSIBILITY.md: Maps accessibility guidance](https://github.com/mgifford/ACCESSIBILITY.md/blob/1dec2d33dfa461a5bcef2a595775068c48aa3c54/examples/MAPS_ACCESSIBILITY_BEST_PRACTICES.md)
- [ACCESSIBILITY.md: Charts and graphs accessibility guidance](https://github.com/mgifford/ACCESSIBILITY.md/blob/1dec2d33dfa461a5bcef2a595775068c48aa3c54/examples/CHARTS_GRAPHS_ACCESSIBILITY_BEST_PRACTICES.md)
- [Double-FW Inclusive Design of Charts, Graphs, Maps and Dynamic Complex Graphics](https://github.com/Double-FW/Accessible_Charts_Graphs_and_Maps/blob/16f4d7c77200c129fe0ed81c1f815e283d6ebb1d/Inclusive_Design_Charts_Maps_and_Complex_Graphics.md)
- [W3C WAI: Complex Images](https://www.w3.org/WAI/tutorials/images/complex/)
- [Government Analysis Function: Publishing charts](https://analysisfunction.civilservice.gov.uk/support/communicating-analysis/introduction-to-data-visualisation-e-learning/module-10-publishing-charts/)
- [W3C WCAG guidance: Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html)
