---
title: Motion and Animation
description: Motion and animation can enhance user experience and understanding, but there are limitations. Use motion accessibly to avoid creating barriers for some users.
---

Incorporating motion and animation into a user interface can benefit users by making content more engaging and making interactions feel more natural. But some users have physical or neurological conditions that mean they experience distraction, motion sickness, or even seizures when viewing effects such as parallax scrolling, zooming/sliding, or flashing.

> Web technologies that use video, animated gifs, animated pngs, animated SVGs, Canvas, and CSS or JavaScript animations are all capable of content that can induce seizures or other incapacitating physical reactions. Certain visual patterns, especially stripes, can also cause physical reactions, even though they are not animated. [^1]

The image or media filenames in these examples stand for assets supplied by the implementing project.

## Reduced motion preferences

Users who know they are likely to be affected by motion can register their preference for reduced motion in their operating system or the web browser.

To adjust your motion preferences in the operating system follow the steps for your device:

- **MacOs**: System Preferences > Accessibility > Display > Reduce motion
- **iOs**: Settings > Accessibility > Motion > Reduce Motion
- **Windows**: Settings > Ease of Access > Display > Show animations in windows
- **Android (9+)**: Settings > Accessibility > Remove animations

### Conditional stylesheets

You may conditionally link to stylesheets based on the user's motion preference settings. In the example below the `animations.css` stylesheet is only included if the user has expressed no preference for reduced motion.

```html
<link rel="stylesheet" href="animations.css" media="(prefers-reduced-motion: no-preference)">
```

### Detect motion preference in JavaScript

All modern browsers allow the detection of user preferences for reduced motion using the JavaScript `matchMedia` method. In this case, we set a class named "motion" on the Document Element if the setting for `prefers-reduced-motion` is not set to "reduce."

```js
const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
if (mq && !mq.matches) {
  document.documentElement.classList.add('motion');
}
```

### Check for motion preference before playing

Based on the presence of the 'motion' class (see above), we can conditionally play an animation or motion-based feature according to the user's preferences.

```js
const io = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting && document.documentElement.classList.contains('motion')) {
      entry.target.play();
    }
  });
}, { threshold: [0] });
const els = document.querySelectorAll('.video-card video');
els.forEach(el => io.observe(el));
```

### Background video

For a decorative background video, keep the message readable without the video. Provide a still image for reduced-motion users and an operable way to stop playback. Supply the media filenames from your project.

```html
<article>
  <h3>Find us on the high street</h3>
  <p>Our shop is open Monday to Saturday.</p>
  <video id="shop-background" aria-hidden="true" playsinline muted loop
         poster="street-scene.jpg" style="max-inline-size: 100%">
    <source src="street-scene.mp4" type="video/mp4">
  </video>
  <button id="shop-motion" type="button" hidden>Pause background video</button>
</article>

<script>
  const shopVideo = document.getElementById('shop-background');
  const shopMotionButton = document.getElementById('shop-motion');
  const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)');

  shopVideo.addEventListener('play', () => {
    shopMotionButton.textContent = 'Pause background video';
    shopMotionButton.hidden = false;
  });

  shopVideo.addEventListener('pause', () => {
    shopMotionButton.textContent = 'Play background video';
  });

  shopMotionButton.addEventListener('click', () => {
    if (shopVideo.paused) {
      shopVideo.play().catch(() => {});
    } else {
      shopVideo.pause();
    }
  });

  reducedMotion.addEventListener('change', (event) => {
    if (event.matches) shopVideo.pause();
  });

  if (!reducedMotion.matches && 'IntersectionObserver' in window) {
    const observer = new IntersectionObserver(([entry]) => {
      if (!entry.isIntersecting) return;
      observer.disconnect();
      if (!reducedMotion.matches) shopVideo.play().catch(() => {});
    });
    observer.observe(shopVideo);
  }
</script>
```

## Autoplay

Use autoplay on video elements carefully. Multiple videos on a page could quickly exhaust a user's bandwidth, particularly if they are on a mobile device. Do not use autoplay on decorative video if the user has enabled the "prefers reduced motion" setting, and provide a mechanism for the user to stop an auto-playing video. 

In the example above, we do not give the HTML `video` element an `autoplay` attribute. Still, we do automatically start playing the video when the `video` element scrolls into the viewport, so the rules around autoplay would still apply.

Unless the core purpose of a web page is to listen to audio, if an animation or video has an audio track and the element is set to play automatically, then the audio must start in a muted state. This is to prevent a user who relies on being able to hear a screen reader device from having their assistive technology drowned out by unseen video clips when the web page loads. Individual controls may be provided to allow the user to unmute the audio if they wish.

## Looping

A constantly looping animation or video segment can be distracting for everyone, but for people with reading or attention-deficit disabilities, never-ending animated content can make it difficult or even impossible to read nearby text. Provide a way for the user to pause, stop or hide any animated, blinking, or scrolling information that:

- Starts automatically
- Lasts for more than five seconds
- Is presented near to core content such as text

## Blinking and flashing

Rapidly flashing content can cause seizures in people with photosensitive epilepsy. [^2] To limit the possibility of triggering seizures, do not display any rapidly flashing content. Some flashing in images could be allowed if it was only in a very small portion of the screen, of low contrast, and the flashes were less frequent than three times per second.

## Checkpoints for the Tester

- Ensure that no content on the webpage flashes over a large portion of the page or more than three times in any 1-second period.
- Content that blinks (over a small area or less frequently than the flashing threshold) must not continue for more than 5 seconds [^3] or provide a way for the user to stop the blinking.
- Where animated/video content is presented alongside other content such as text, ensure that the animated content does not autoplay, or if it does, that it does not last longer than 5 seconds, or else there is a way for the user to stop the animation.
- With your settings for reduced motion set to "reduced," ensure that no unnecessary animation, motion, or blinking effects are displayed.
- Ensure that no sound plays automatically: it should be muted, and the user should opt-in to hearing the sound by using an unmute button, for example. An exception is if the core purpose of the web page is to play the sound, and the user is notified that by opening the page, that soundtrack will play.

[^1]: [_Web accessibility for seizures and physical reactions_, mdn web docs](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Seizure_disorders)
[^2]: "Photosensitive epilepsy is when seizures are triggered by flashing lights or contrasting light and dark patterns." [_Photosensitive epilepsy_, Epilepsy Society](https://epilepsysociety.org.uk/about-epilepsy/epileptic-seizures/seizure-triggers/photosensitive-epilepsy)
[^3]: "Five seconds was chosen because it is long enough to get a user's attention, but not so long that a user cannot wait out the distraction if necessary to use the page." [_Pause, Stop, Hide (Level A)_, WCAG 2.1 Understanding Docs](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html)
