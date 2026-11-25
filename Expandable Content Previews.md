---
title: Expandable Content Previews
description: "Continuing from a preview."
---

# Expandable Content Previews

Use a preview to shorten long main content. Begin with all content available. Once enhanced, hide the overflow from sight, keyboard focus and screen reader navigation together. Keep the cut-off and expansion control visible in forced colours. Use a descriptive control label and expose its state.

## Implementation

The preview reveals content already present in the page. For results fetched on demand, use [Load More](Load%20More.md). Only clip complete content items if partial items would leave users with unreachable controls. When resizing changes the preview boundary, synchronise the inert or hidden state of every affected item. Keep the continuation marker out of the ordinary tab order while allowing programmatic focus.

## Checkpoints for the tester

1. Activate Show more.
2. Focus should move to a continuation marker immediately before the newly revealed content.
3. Activate Show less and check focus stays on the control.
4. Resize or zoom the preview and confirm no clipped links remain keyboard stops.

## Related references

- [windows use high contrast mode](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [WCAG2.1 2.4.1 Bypass Blocks](https://www.w3.org/TR/WCAG21/#bypass-blocks)
- [CSS Viewport Units: A Quick Start](https://www.sitepoint.com/css-viewport-units-quick-start/)
- [`IntersectionObserver` support table](https://caniuse.com/#feat=intersectionobserver)
- [`inert` polyfill](https://github.com/WICG/inert)
