---
title: Motion and Animation
description: Use motion deliberately and let people read and operate the interface with less motion.
---

# Motion and Animation

Use motion when it helps people understand a change. Large movements, parallax scrolling and repeated effects can make content hard to read or use. Start with a usable still presentation. Keep instructions, changes of state and progress understandable through text or other cues that do not depend on movement.

## Reduced motion preferences

Respect the device or browser’s `prefers-reduced-motion` setting. Replace non-essential movement with an immediate change or a still image. For example, show a panel in its final position and keep its focus target available immediately. Keep the interface functional when the preference is unavailable.

### Conditional stylesheets

Keep essential styles in the baseline. Load optional motion styles only when the browser reports no reduced-motion preference. The example assumes `animations.css` contains only optional effects and is supplied by the project.

```html
<link rel="stylesheet" href="animations.css" media="(prefers-reduced-motion: no-preference)">
```

### Detect motion preference in JavaScript

For motion controlled by script, check for an explicit match on `(prefers-reduced-motion: no-preference)` before starting an optional automatic effect. Listen for changes while the page is open so you can stop the effect if the person enables reduced motion.

### Check for motion preference before playing

Leave decorative motion paused when the page opens. If people choose to play an effect, provide a nearby way to pause or stop it. When the preference changes to reduced motion, stop current decorative motion and let the person decide whether to restart it. Scrolling away and back should respect a previous pause.

### Background video

Use a still poster and keep the surrounding message readable without video. The following example starts paused and lets the person play or pause a silent decorative clip. Supply the sample media assets and adjust the labels to the context. For video that carries information, use [Video Controls](Video%20Controls.md).

```html
<article>
  <h2>Find us on the high street</h2>
  <p>Our shop is open Monday to Saturday.</p>

  <video id="shop-video" aria-hidden="true" playsinline muted
         preload="none" poster="street-scene.jpg" style="max-inline-size: 100%">
    <source src="street-scene.mp4" type="video/mp4">
  </video>
  <button id="shop-motion" type="button" hidden>Play shop video</button>
  <p id="shop-motion-status" role="status"></p>
</article>

<script>
  const video = document.getElementById('shop-video');
  const button = document.getElementById('shop-motion');
  const status = document.getElementById('shop-motion-status');
  const motionPreference = window.matchMedia?.('(prefers-reduced-motion: reduce)');

  function updateButton() {
    button.textContent = video.paused || video.ended ? 'Play shop video' : 'Pause shop video';
  }

  for (const event of ['play', 'pause', 'ended']) {
    video.addEventListener(event, updateButton);
  }

  button.addEventListener('click', async () => {
    status.textContent = '';
    if (video.paused || video.ended) {
      try {
        await video.play();
      } catch {
        status.textContent = 'The shop video could not play.';
        updateButton();
      }
    } else {
      video.pause();
    }
  });

  motionPreference?.addEventListener?.('change', event => {
    if (event.matches) video.pause();
  });

  button.hidden = false;
</script>
```

## Autoplay

Prefer playback initiated by the user. Animation started by a script on page load or as content enters the viewport is also automatic. Keep decorative video silent. WCAG 2.1 [1.4.2 Audio Control](https://www.w3.org/TR/WCAG21/#audio-control) (Level A) requires a way to pause or stop audio that plays automatically for more than three seconds, or to control its volume independently of the system volume.

## Looping

Under WCAG 2.1 [2.2.2 Pause, Stop, Hide](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html) (Level A), moving, blinking or scrolling information that starts automatically, lasts more than five seconds and appears alongside other content needs a way to pause, stop or hide it, unless the effect is essential. A short clip that keeps looping can run beyond five seconds. The criterion has a separate provision for automatically updating information, which has no five-second condition.

If the project targets WCAG 2.1 [2.3.3 Animation from Interactions](https://www.w3.org/TR/WCAG21/#animation-from-interactions) (Level AAA), people must be able to disable motion animation triggered by interaction unless it is essential to the function or information.

## Blinking and flashing

Avoid flashing content. WCAG 2.1 [2.3.1 Three Flashes or Below Threshold](https://www.w3.org/TR/WCAG21/#three-flashes-or-below-threshold) (Level A) allows no more than three flashes in any one-second period unless the flash is below both the general and red flash thresholds. A pause control or reduced-motion setting does not establish that the flashing itself meets the criterion.

Review suspected flashing using a suitable analysis tool or verified evidence. Avoid asking testers to judge a potentially hazardous sequence by watching it.

## Checkpoints for the Tester

| Method and action | Expected result |
| --- | --- |
| Open the page with reduced motion enabled. Operate any feature that normally moves. | Non-essential motion is removed or reduced. Controls, focus and feedback remain available. |
| Start a decorative clip, pause it, then scroll away and back. Enable reduced motion while the clip is playing in a separate run. | The pause remains in effect after scrolling. Enabling reduced motion stops current decorative playback and leaves the controls usable. |
| Find automatically moving or looping content alongside other content. Check its duration and operate any pause, stop or hide control with the keyboard. | Where the criterion applies, the user can stop the effect and continue reading. |
| Check the page for automatically playing audio. Where it plays for more than three seconds, try the available controls. | The audio can be paused, stopped or turned down independently of system volume. |
| Review flashing assets and effects using analysis or verified evidence. | The result meets the applicable flash limits. |
| Block scripts and reload the decorative video example. | The still poster and nearby information remain available, and no inactive enhancement button appears. |

## Sources

- [Web accessibility for seizures and physical reactions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Seizure_disorders)
- [Photosensitive epilepsy](https://epilepsysociety.org.uk/about-epilepsy/epileptic-seizures/seizure-triggers/photosensitive-epilepsy)
