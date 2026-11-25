---
title: Video Controls
description: "Operating playback."
---

# Video Controls

Start with native video controls. Replace them only once custom controls work. Use named buttons whose text communicates the available play, pause, mute or unmute action. Use a labelled range input for seeking and expose elapsed time and duration in understandable units. Keep controls visible on focus, usable by touch and distinguishable in forced colours. Follow [Motion and Animation](Motion%20and%20Animation.md) for autoplay.

## Implementation

Provide available captions and language choices through text tracks. Include a link to the media as a fallback. For a custom timeline, synchronise its range with the duration once known and expose elapsed time in minutes and seconds. Recalculate the displayed position after seeking. Keep the control layout usable when a narrow viewport requires wrapping.

### Markup example

Supply the video file and matching caption track.

```html
<video controls preload="metadata">
  <source src="journey.mp4" type="video/mp4">
  <track kind="captions" src="journey-en.vtt" srclang="en" label="English" default>
  <a href="journey.mp4">Download the journey video</a>
</video>
```

## Checkpoints for the tester

1. With the keyboard, start and pause playback, mute and unmute, then seek using the arrow keys.
2. Check that control names follow the actual media state, including when playback ends.
3. Verify the timeline value and its spoken time agree.
4. Block scripts and repeat using native controls.

## Related references

- [Using the group role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_group_role)
- [What is an accessible name?](https://developer.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)
- [IE focus bugs with focusable elements](https://simplyaccessible.com/article/7-solutions-svgs/#acc-heading-4)
- [Styling Cross-browser compatible range inputs](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/)
- [Flex](https://css-tricks.com/almanac/properties/f/flex/)
- [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)
