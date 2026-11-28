---
title: "Expandable Content Previews"
description: "Let people reveal long content and resume reading at the point where the preview ended."
---

# Expandable Content Previews

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use an expandable preview when a short opening section helps people decide whether to read the rest of a long article. Place the preview in the [main landmark](Page_Structure.md#the-main-landmark) so people can reach it using the page’s skip link. Keep essential instructions visible. Use [Disclosure](Disclosure.md) for supplementary information within a section, or [Load More](Load%20More.md) when the service fetches additional results on demand.

## Recommended markup

### Reading across the preview boundary

Place the preview within the main content so readers can reach it using the page’s skip link. In the collapsed state, start on the last available link or control in the preview and press Tab. Focus should reach Show more without entering the clipped content. Activate it and check that the continuation marker is visible when it receives focus and identifies where to resume reading. With scripts blocked, read past the same point and check that the rest of the content is already available.

## Recommended layout

<span id="presentation"></span>

End the preview at a complete paragraph, list item or section. Keep the expansion button immediately after the visible preview in reading order. Label it for the content it controls, such as ‘Show full route guide’. Provide a visible boundary and a focus indicator that remain clear in forced colours.

When expanded, place a short continuation marker, such as ‘Continue reading’, immediately before the newly revealed content. Keep the collapse control after the revealed content. Use a visible focus indicator on the marker so keyboard users can see where reading resumes.

## Recommended behaviour

<span id="behaviour"></span>

Start with the full content available. Once the enhancement is ready, hide the remainder visually and remove it from screen reader navigation and keyboard focus order. Keep all three states in sync.

When the user expands the preview, reveal the remainder, update the button’s expanded state and focus the continuation marker. Keep the marker out of the ordinary tab order. Reading forward should enter the newly revealed text. Pressing Tab should reach the next interactive element in that content, or the collapse control if it contains none.

When the user collapses the preview, keep focus on the same button. Hide the continuation marker and remainder, and update the button label and expanded state. Leave the preview and surrounding content available.

## Reference implementation

<span id="implementation"></span>

The example uses a fixed content boundary and needs no application framework or network request. Place the fragment inside the page’s main content. The rest of the guide starts visible and the button starts hidden. When the script is available, it installs the handler before collapsing the remainder.

```html
<article>
  <h2>Walking route guide</h2>
  <p>The route starts beside the station and follows the river.</p>

  <div id="route-remainder">
    <p id="route-continuation" tabindex="-1" hidden>Continue reading</p>
    <p>After the footbridge, follow the path towards the park entrance.</p>
    <h3>Returning to the station</h3>
    <p>Follow the marked path back from the park.</p>
  </div>

  <button type="button" id="route-preview-button"
          aria-controls="route-remainder" hidden>Show full route guide</button>
</article>

<style>
  #route-continuation:focus {
    outline: 2px solid currentColor;
    outline-offset: 2px;
  }
</style>

<script>
  const previewButton = document.getElementById('route-preview-button');
  const remainder = document.getElementById('route-remainder');
  const continuation = document.getElementById('route-continuation');

  function setPreviewExpanded(expanded) {
    remainder.hidden = !expanded;
    continuation.hidden = !expanded;
    previewButton.setAttribute('aria-expanded', String(expanded));
    previewButton.textContent = expanded
      ? 'Show less of the route guide'
      : 'Show full route guide';
  }

  previewButton.addEventListener('click', () => {
    const expanded = previewButton.getAttribute('aria-expanded') === 'true';
    if (expanded) {
      previewButton.focus();
    }
    setPreviewExpanded(!expanded);
    if (!expanded) {
      continuation.focus();
    }
  });

  setPreviewExpanded(false);
  previewButton.hidden = false;
</script>
```

### Boundaries that change with layout

Prefer a boundary between authored sections. If the boundary depends on available height, calculate which complete blocks fit. A block that intersects a height threshold can still contain text or controls below it. Crop only at a complete block boundary, and hide the remaining blocks or make them inert as part of the same update. If you use `inert`, apply visual hiding separately.

Recalculate after resizing, zoom or content changes. If a new boundary would hide the element that currently has focus, leave that content visible until the user can move on. If the full content fits, a collapse control is unnecessary. Keep full content available whenever the enhancement cannot reliably synchronise visibility and interaction.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Test with the project’s supported browsers and screen readers. Include a link in the preview and one in the initially hidden content so the keyboard check can distinguish them.

| Method and action | Expected result |
| --- | --- |
| Start collapsed. Read through the preview with a screen reader. Tab from the final visible link. | The hidden content is skipped in both reading and tab order. Focus reaches the expansion button, whose name and collapsed state identify what can be opened. |
| Activate the expansion button. Read forward from the focus target, then press Tab. | Focus moves to the visible continuation marker immediately before the revealed content. Reading continues at the boundary and Tab reaches the next link or control. |
| Reach the collapse control and activate it. | Focus stays on the control. Its name and state reflect the collapsed preview. The remainder leaves visual, reading and focus order. |
| Increase text size, then test a narrow viewport separately. If the boundary adapts, resize while a link near it has focus. | No text or controls are partly hidden. Resizing leaves focus on usable content. The button, boundary and continuation focus indicator remain clear in forced colours. |
| Block scripts and reload. Then enable scripts and disable CSS in a separate run. | With scripts blocked the full content is readable. With CSS disabled, the working control and reading order remain available. |

### Supplied QA scripts

- [Disclosure Test Criteria](qa/supplied/disclosure.md) — Apply shared disclosure checks. Use this guide’s continuation-focus procedure when the preview moves focus.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](qa/w3c-act/6cfa84.md)

## Sources

- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [WCAG2.1 2.4.1 Bypass Blocks](https://www.w3.org/TR/WCAG21/#bypass-blocks)
- [CSS Viewport Units: A Quick Start](https://www.sitepoint.com/css-viewport-units-quick-start/)
- [`IntersectionObserver` support table](https://caniuse.com/#feat=intersectionobserver)
- [`inert` polyfill](https://github.com/WICG/inert)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)
