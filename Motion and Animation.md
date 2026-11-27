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

For decorative video behind brochure or landing-page content, start with a still image. Keep headings, text and links readable and usable in both states. Use a silent source and keep decorative media out of the reading and tab order. If footage carries information, provide it through the page content or an operable [Video Controls](Video%20Controls.md) player.

When the device or browser requests reduced motion, show the still image from the start. If that preference turns on while the background video is playing or loading, cancel playback and restore the still image. Keep the still image when scripts or playback fail. If reduced motion is later turned off, wait for the person to choose when to play.

If background motion is available, put a visible **Pause background motion** control early in the site-wide header, alongside the main navigation. Use a native button with a visible focus indicator and an accessible name that matches its label. Keep it usable with a keyboard and easy to find using the page-level [banner landmark](Landmarks.md#guidance). One control can pause all decorative backgrounds in its stated scope. Apply a pause across page navigation where the site carries that setting forward, and use the still image on new pages until playback is explicitly requested. Keep controls for informational videos independent.

This example controls one decorative background. It shows the still image when paused, while playback is loading and when reduced motion is requested. Supply the image and silent video assets at the same aspect ratio. For a site-wide control with several backgrounds, apply the same state to all of them and check that the label matches the scope.

```html
<header>
  <!-- Site identity and primary navigation go here. -->
  <button id="background-motion" type="button" hidden>Play background motion</button>
  <p id="background-motion-preference" hidden>Background motion is off with your current motion settings.</p>
  <p id="background-motion-status" role="status"></p>
</header>

<main>
  <h1>Find us on the high street</h1>
  <p>Our shop is open Monday to Saturday.</p>
  <img id="shop-still" src="street-scene.jpg" alt=""
       width="960" height="540" style="max-inline-size: 100%; block-size: auto">
  <video id="shop-video" hidden aria-hidden="true" playsinline muted loop
         preload="none" src="street-scene.mp4"
         width="960" height="540" style="max-inline-size: 100%; block-size: auto"></video>
</main>

<script>
  const still = document.getElementById('shop-still');
  const video = document.getElementById('shop-video');
  const button = document.getElementById('background-motion');
  const preferenceNote = document.getElementById('background-motion-preference');
  const status = document.getElementById('background-motion-status');
  const motionAllowed = window.matchMedia?.('(prefers-reduced-motion: no-preference)');
  let playRequest;

  function showStill() {
    playRequest = undefined;
    video.pause();
    video.hidden = true;
    still.hidden = false;
    button.textContent = 'Play background motion';
  }

  function updatePreference() {
    if (motionAllowed.matches) {
      button.removeAttribute('aria-disabled');
      button.removeAttribute('aria-describedby');
      preferenceNote.hidden = true;
    } else {
      showStill();
      status.textContent = '';
      button.setAttribute('aria-disabled', 'true');
      button.setAttribute('aria-describedby', preferenceNote.id);
      preferenceNote.hidden = false;
    }
  }

  function reportPlaybackError() {
    if (!playRequest) return;
    showStill();
    status.textContent = 'Background motion could not play.';
  }

  if (motionAllowed?.addEventListener) {
    button.addEventListener('click', async () => {
      if (!motionAllowed.matches) return;
      status.textContent = '';

      if (playRequest) {
        showStill();
        return;
      }

      const request = video.play();
      playRequest = request;
      button.textContent = 'Pause background motion';

      try {
        await request;
        if (playRequest === request && motionAllowed.matches && !video.paused) {
          still.hidden = true;
          video.hidden = false;
        }
      } catch {
        if (playRequest === request) reportPlaybackError();
      }
    });

    video.addEventListener('pause', () => {
      if (video.paused) showStill();
    });
    video.addEventListener('error', reportPlaybackError);
    motionAllowed.addEventListener('change', updatePreference);
    updatePreference();
    button.hidden = false;
  }
</script>
```

## Autoplay

Prefer playback initiated through an explicit control. Treat motion started on page load, mouseover, focus or scrolling into view as automatic. Muting an automatically playing video leaves the visual motion in place. Keep decorative video silent. WCAG 2.2 [1.4.2 Audio Control](https://www.w3.org/TR/WCAG22/#audio-control) (Level A) requires a way to pause or stop audio that plays automatically for more than three seconds, or to control its volume independently of system volume.

## Looping

Under WCAG 2.2 [2.2.2 Pause, Stop, Hide](https://www.w3.org/TR/WCAG22/#pause-stop-hide) (Level A), moving, blinking or scrolling information that starts automatically, lasts more than five seconds and appears alongside other content needs a way to pause, stop or hide it, unless the effect is essential. A short clip that keeps looping can run beyond five seconds. The criterion has a separate provision for automatically updating information, which has no five-second condition.

If the project targets WCAG 2.1 [2.3.3 Animation from Interactions](https://www.w3.org/TR/WCAG21/#animation-from-interactions) (Level AAA), people must be able to disable motion animation triggered by interaction unless it is essential to the function or information.

## Blinking and flashing

Avoid flashing content. WCAG 2.1 [2.3.1 Three Flashes or Below Threshold](https://www.w3.org/TR/WCAG21/#three-flashes-or-below-threshold) (Level A) allows no more than three flashes in any one-second period unless the flash is below both the general and red flash thresholds. A pause control or reduced-motion setting does not establish that the flashing itself meets the criterion.

Review suspected flashing using a suitable analysis tool or verified evidence. Avoid asking testers to judge a potentially hazardous sequence by watching it.

## Checkpoints for the Tester

| Method and action | Expected result |
| --- | --- |
| Open the page with reduced motion enabled. Operate any feature that normally moves. | Non-essential motion is removed or reduced. Controls, focus and feedback remain available. |
| Load the page with reduced motion enabled. In a separate run, enable it while the decorative clip is loading or playing, then disable it again. | The still image is shown immediately. Enabling reduced motion cancels playback and restores the still image. Turning reduced motion off leaves the image in place until Play is activated. |
| Find the background control by Tab and through the banner landmark using a supported screen reader. Start and pause background motion. Scroll away and back, then navigate to another page with the same site-wide control. | The button has a visible focus indicator and an accessible name that matches its visible action. The pause takes effect for all backgrounds in scope, remains in effect after scrolling and is honoured on the next page. Separately started informational video is unaffected. |
| Find automatically moving or looping content alongside other content. Check its duration and operate any pause, stop or hide control with the keyboard. | Where the criterion applies, the user can stop the effect and continue reading. |
| Check the page for automatically playing audio. Where it plays for more than three seconds, try the available controls. | The audio can be paused, stopped or turned down independently of system volume. |
| Review flashing assets and effects using analysis or verified evidence. | The result meets the applicable flash limits. |
| Block scripts and reload the background video example. In a separate run, block the video asset and activate Play. | The still image and nearby information remain available in both cases. With scripts blocked, no inactive button appears. When playback fails, the error is perceivable and the button returns to Play. |

## Sources

- [Web accessibility for seizures and physical reactions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Seizure_disorders)
- [Photosensitive epilepsy](https://epilepsysociety.org.uk/about-epilepsy/epileptic-seizures/seizure-triggers/photosensitive-epilepsy)
- [1.4.2 Audio Control](https://www.w3.org/TR/WCAG22/#audio-control)
- [2.2.2 Pause, Stop, Hide](https://www.w3.org/TR/WCAG22/#pause-stop-hide)
- [2.3.3 Animation from Interactions](https://www.w3.org/TR/WCAG21/#animation-from-interactions)
- [2.3.1 Three Flashes or Below Threshold](https://www.w3.org/TR/WCAG21/#three-flashes-or-below-threshold)
