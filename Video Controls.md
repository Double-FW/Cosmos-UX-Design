---
title: "Video Controls"
description: "Help people operate web video and use its accessibility features."
---

# Video Controls

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people start, pause and seek through a video, control its sound and reach the available access services. Start with native browser controls. If you build a custom player, keep its most-used actions easy to find and preserve access when the enhancement fails. Use [Accessible Interactive Media Control](https://github.com/Double-FW/Inclusive-Media-Interfaces/blob/main/Accessible_Interactive_Media_Control.md) for broader media interaction and input guidance.

## Recommended markup

Start with the native video element and its controls. Use labelled buttons and a labelled range input when adding custom controls.

## Recommended layout

<span id="presentation"></span>

- Keep Play/Pause first in the custom player’s control sequence and Mute/Unmute next where the player offers it. Keep the remaining controls in visual and keyboard order. Use familiar symbols and clear labels. Give each control a visible focus indicator and enough room to select it by touch or pointer. See [Focus](Focus.md#focus-indication) and [pointer target guidance](Call%20to%20Action.md#give-pointer-users-room-to-activate-the-action).
- Keep on/off controls for captions and audio description in the main custom controls when those services are available. A menu can offer track, language or style choices. If audio description is provided through a separate video, offer a clearly labelled link beside the player. See [caption and subtitle guidance](https://github.com/Double-FW/Closed-Caption-Subtitles/blob/main/Closed_Caption_Subtitle_Guidelines.md) for preparing and displaying tracks.
- Let the timeline use the available width, and wrap controls in order when space narrows. Keep controls and captions from obscuring one another. Check the slider track, thumb, buttons and focus indicator in [forced colours](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours). A native slider style is a useful fallback when a custom style loses its track or thumb.

## Recommended behaviour

<span id="behaviour"></span>

### Playback and sound

Wait for the person to activate a clearly labelled Play control by default. Keep video paused on page load, pointer hover, keyboard focus and scrolling into view. People use those actions to navigate and read. Unexpected sound can mask screen reader speech, and unexpected motion can interrupt reading or make it harder to keep track of the page.

If the person explicitly opts in to autoplay, start muted and keep Pause, sound controls and the autoplay opt-out easy to reach. Muted playback still introduces motion. Keep sound off until the person chooses to hear it. Honour reduced motion by suppressing automatic starts and leaving the person free to start an informational video themselves.

### Looping and playing the next video

End playback after one pass and offer Replay or a clearly labelled choice of the next video. If you offer looping or auto-next, keep each off by default and make it easy to turn off. Pause must stop the current video and cancel any pending start. A paused video must stay paused when the person moves focus away or scrolls away and back. Turning off auto-next must prevent the next item from starting, including after navigation. Keep focus predictable and let people tell which video is playing in an opted-in sequence.

Follow [Motion and Animation](Motion%20and%20Animation.md#autoplay) for automatic motion and sound thresholds. For decorative video behind brochure or landing-page content, use the [background video pattern](Motion%20and%20Animation.md#background-video).

Use native buttons for custom actions. Where the button names the next action, change ‘Play’ to ‘Pause’ or ‘Mute’ to ‘Unmute’ when the media state changes. Keep the text, accessible name and icon in sync. A stable label with `aria-pressed` can suit an on/off control such as ‘Captions’.

Derive controls from the actual media state, including changes initiated by native or platform controls. Return to ‘Play’ when playback ends. If a play request fails, show the current state and say how to retry or use an alternative. Give timely, perceivable feedback for buffering, unavailable media or other interruptions. Use [persistent inline feedback](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback) for messages people need to read or act on.

Keep controls visible while they contain keyboard focus. Make them easy to reveal with the supported inputs if they fade during playback. Let people interrupt an action and move out of the player with the usual navigation commands.

### Seek through the video

Provide a labelled native range input for a custom seek control. Let people move it with keyboard commands, touch and pointer. Show the current position and duration, and keep the visible position, range value and accessible time in sync during playback and after a seek.

For a recording, use seconds for `min`, `max` and `value`, and set the maximum from the duration when it becomes known. Use `aria-valuetext` for a readable value such as ‘15 seconds of 1 minute 35 seconds’. Let the native input expose the numeric values. Keep routine playback updates out of live regions.

For live media, use the actual seekable window if the stream supports seeking. When seeking is unavailable, remove or disable the seek control and explain its availability where that helps people understand the player.

## Reference implementation

<span id="implementation"></span>

### Markup example

This baseline uses native playback and text-track controls. Replace the example media and track paths with working assets. Use `kind="captions"` for tracks that include dialogue and relevant sounds. Label each track in the player so people can find the right language. The link outside the player remains available if the browser supports video but cannot load the source.

```html
<div role="group" aria-labelledby="journey-video-title">
  <h2 id="journey-video-title">Using the station entrance</h2>
  <video controls preload="metadata">
    <source src="journey.mp4" type="video/mp4">
    <track kind="captions" src="journey-en.vtt" srclang="en" label="English">
  </video>
  <p><a href="journey.mp4">Open the station entrance video file</a></p>
</div>
```

Select a fallback that can be used independently, such as an accessible video destination or a download with its matching caption file where appropriate. Provide access to the transcript or described version when supplied.

Keep custom controls hidden until their event handling, state updates and available access services work. Replace the native controls only then. If the enhancement fails, restore native controls and keep alternative access links available. See [Progressive Enhancement](Progressive%20Enhancement.md).

### Timeline markup

This snapshot shows a finite recording 15 seconds into a 95-second duration. It needs script to update the slider and visible time from the media, and to seek when the person moves the slider. Keep these updates in step with confirmed position changes.

```html
<label for="journey-timeline">Video timeline</label>
<input id="journey-timeline" type="range" min="0" max="95" value="15"
  aria-valuetext="15 seconds of 1 minute 35 seconds">
<span>0:15 / 1:35</span>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Run the checks on the supported browsers, devices and assistive technologies. Use a recording with captions and test audio description and each other access service where supplied.

| Method and action | Expected result |
| --- | --- |
| Block scripts and reload. Repeat with an enhancement failure. Simulate an unavailable media source. | Native controls remain usable when media loads. A media error is clear and any supplied alternative stays reachable. Uninitialised controls do not appear. |
| Tab into the custom controls, start and pause playback, mute and unmute, then let playback end. Where available, change state using platform controls as well. | Play/Pause comes first and Mute/Unmute comes next where present. The control label, accessible name, icon and playback state agree. The final playback action is Play. Focus remains visible and can leave the player. |
| Enable and disable captions and audio description. Check alternate tracks or the described version if provided. | On/off controls for available services are easy to find. The selected state agrees with what is shown or heard. Captions are legible, synchronised and clear of the controls. |
| Seek by keyboard, touch and pointer. With a screen reader, check the value before and after seeking, and listen during normal playback. | The video moves to the requested position within the available range. Visible and accessible elapsed time and duration agree. Continuous playback does not create repeated live announcements. |
| Wait for controls to fade, then return to the player by keyboard and touch. Enable forced colours and repeat. Check enlarged text and narrow layouts separately. | Controls can be revealed without hover. Focused controls stay visible. Labels, states, slider parts and focus remain distinguishable and targets stay usable. |
| With autoplay off, load the page, move the pointer across the video, Tab into its controls and scroll it out of view and back. Then activate Play. | The video stays paused and silent until Play is activated. Playback starts and the control reflects its state. |
| If opt-in autoplay is supported, enable it and reload. Pause the video, move focus away and scroll away and back. Turn autoplay off and reload. In a separate run, reload with reduced motion enabled. | Opted-in playback starts muted. A paused video stays paused during navigation within the page. Turning autoplay off or enabling reduced motion prevents automatic starts on load. The Play control remains usable for informational video. |
| Let a clip finish with loop and auto-next off. Where those options exist, test them separately. Pause near the end or during any auto-next countdown. Turn auto-next off, navigate to another video and let it finish. | The default is one play-through. Each opted-in mode behaves as labelled. Pausing cancels any pending start. The next item waits for activation after opt-out, and focus remains usable throughout. |
| Interrupt playback or seek with buffering, then simulate a playback failure. | Feedback is perceivable, controls reflect the actual state and the available recovery action is clear. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Video element auditory content has captions (f51b46)](https://www.w3.org/WAI/standards-guidelines/act/rules/f51b46/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)

## Sources

<span id="related-references"></span>

- [Accessible web video controls in Inclusive Media Interfaces](https://github.com/Double-FW/Inclusive-Media-Interfaces/blob/main/Accessible_Video_Controls_Organisation_Agnostic_Draft.md)
- [MDN: Using the ARIA group role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_group_role)
- [What is an accessible name?](https://developer.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)
- [IE focus bugs with focusable elements](https://simplyaccessible.com/article/7-solutions-svgs/#acc-heading-4)
- [Styling Cross-browser compatible range inputs](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/)
- [CSS flex shorthand](https://css-tricks.com/almanac/properties/f/flex/)
- [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)
