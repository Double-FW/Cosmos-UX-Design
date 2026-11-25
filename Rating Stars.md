---
title: Rating Stars
description: Used to display or enter a feedback rating on a 1 to 5 star scale.
---

## Displaying rating stars

The display snippets show the markup and accessible text. Supply the star graphics and styles in the host design system.

The rating stars pattern is a well-established way of communicating an average user-ratings result. Because the stars are graphical, you must provide a text alternative to the rating content. When displaying the rating stars, we hide the icons from screen readers using an `aria-hidden` attribute while providing the information as visible text close by.

There is no need to attempt to create an _identical_ experience for screen readers. It is less useful to hear a summary like, "rating: three solid stars and two outlined stars." It leaves the user to wonder which kind of star is "selected" and what order the stars might be in. Instead, communicate the information, not the presentation: "rating: three out of five."

```html
<div class="display-star-rating">
  <div class="display-star-group" aria-hidden="true" role="img" style="width: 5em"><span class="display-star-value" style="width:3em"></span></div>
  <p>Average rating: 3 (out of 5)</p>
</div>
```

> Screen reader: "Average rating: 3 (out of 5)"

### Customizing display stars

You can adjust the number of possible stars and the number of filled-in stars by modifying the widths of the elements with classnames of `display-star-group` and `display-star-value`, respectively. The overall size of the stars will scale with the surrounding font size. You can use CSS filters [^5] to change the color of the SVG images that display the stars.

```html
<div class="display-star-rating">
  <div class="display-star-group display-star-gold" aria-hidden="true" role="img" style="font-size: 2rem; width: 10em"><span class="display-star-value" style="width: 4.5em"></span></div>
  <p>Average rating: 4.5 (out of 10)</p>
</div>
```

### Supporting internationalization

When displaying rating stars in languages read from right to left, the order of the stars should follow the same direction the text is written in and appear in right-to-left order. Use an HTML `dir` attribute with the value `rtl` and test that the project’s star styling follows the resulting reading direction.

```html
<div class="display-star-rating" dir="rtl" lang="ar">
  <div class="display-star-group" aria-hidden="true" role="img" style="width: 10em"><span class="display-star-value" style="width: 6.5em"></span></div>
  <p>متوسط ​​التصنيف: 6.5 نجوم</p>
</div>
```

## Interactive star rating

Interactive rating stars allow users to enter their feedback on a star scale, for example, rating products on sale. This data helps others make informed decisions and provides organizations with valuable customer sentiment data.

### Use web form elements

Although this component appears to be a purely custom element, we built our implementation on a semantically-aligned native HTML element: a group of `input` elements of type "radio." By starting with a usable HTML foundation, we ensure that the final component will have maximum compatibility with assistive technologies and will continue to be usable when JavaScript is unavailable on the client.

An HTML `input` element with the type "range" would have also been a semantically appropriate foundation. However, we didn't go in that direction due to incompatibilities [^4] that still exist with the new HTML5 input types when interpreted by various browser, device, and assistive technology combinations and because of the potential difficulties some disabled users have with elements that require a slider-type interaction.

> Some users of touch-based assistive technologies may experience difficulty utilizing widgets that implement this slider pattern because the gestures their assistive technology provides for operating sliders may not yet generate the necessary output. To change the slider value, touch-based assistive technologies need to respond to user gestures to increase and decrease the value by synthesizing key events. This is a new convention that some assistive technologies may not fully implement. [^6]

### Avoid background images

Implementations that utilize background images to display any input star states are inaccessible for users who rely on a high-contrast rendering mode. [^3] When this mode is activated (either at the system or browser level), web elements will no longer display any background images. This is similar to the view you may have seen when printing a webpage.

> The primary issue with non-decorative CSS background images is that when images are turned off or when high contrast mode is enabled (for example, under the Windows OS), background images disappear. If the background image either contains text ... or provides meaningful icon imagery, then users of high contrast mode and those without images will not see that content. [^2]

In the case of the ratings star component, the problem for sighted users would be that under high contrast mode, the label text would still be hidden, but if the icons were implemented as background images, they too would become hidden.

### Touch target size

Use 44 × 44 CSS pixels as the design target for each interactive star. Adjacent options should be easy to select individually. This target draws on the cited WCAG 2.1 Target Size criterion (Level AAA). [^1]

## Example implementation

The example shows native radio inputs. Place the group in a form with a submit button and a working submission endpoint. The star styling and target sizing are supplied by the implementing project. Keep the visible labels until that styling is available.


```html
<fieldset class="star-rating">
  <legend>Your rating:</legend>
  <div class="star-group">
    <input type="radio" name="rating" value="1" id="rating1">
    <label for="rating1">1 star</label>
    <input type="radio" name="rating" value="2" id="rating2">
    <label for="rating2">2 stars</label>
    <input type="radio" name="rating" value="3" id="rating3">
    <label for="rating3">3 stars</label>
    <input type="radio" name="rating" value="4" id="rating4">
    <label for="rating4">4 stars</label>
    <input type="radio" name="rating" value="5" id="rating5">
    <label for="rating5">5 stars</label>
  </div>
</fieldset>
```

> Screen reader: "1 star, radio button, 1 of 5, Your rating:, group"
> Screen reader: "selected, 1 star, radio button, 1 of 5"
> Screen reader: "2 stars, selected, radio button, 2 of 5"

### Supporting internationalization

The text used to label each star, which while not visible, will be announced by screen readers, is in the _content_ of the HTML `label` element rather than as an HTML attribute value. This means it can be translated by browser extensions and services such as Google Translate.

By applying the `dir` attribute with a value of `rtl` (right to left), the component automatically uses a layout natural to those languages.

```html
<fieldset class="star-rating" dir="rtl" lang="ar">
  ...
</fieldset>
```

```html
<fieldset class="star-rating" dir="rtl" lang="ar">
  <legend>تقييم بالنجوم</legend>
  <div class="star-group">
    <input type="radio" name="rating-ar" value="1" id="rating-ar1">
    <label for="rating-ar1">نجمة واحدة</label>
    <input type="radio" name="rating-ar" value="2" id="rating-ar2">
    <label for="rating-ar2">2 نجوم</label>
    <input type="radio" name="rating-ar" value="3" id="rating-ar3">
    <label for="rating-ar3">3 نجوم</label>
    <input type="radio" name="rating-ar" value="4" id="rating-ar4">
    <label for="rating-ar4">4 نجوم</label>
    <input type="radio" name="rating-ar" value="5" id="rating-ar5">
    <label for="rating-ar5">5 نجوم</label>
  </div>
</fieldset>
```

## Checkpoints for the Tester

1. Activate high contrast mode rendering [^7] (removing background images and applying a forced color mode of a black foreground on a white background). Confirm that the star icons are all visible and indicate a filled or empty state. For interactive rating, ensure that the focus indicator is visible and shows which star currently has focus when navigating by keyboard.
2. Using only the keyboard, ensure that it is possible to use the <kbd>tab</kbd> key to move focus into the interactive rating star group and that it is possible to select the focused star using the <kbd>space</kbd> key. By using the <kbd>←</kbd> <kbd>→</kbd> arrow keys, ensure that changing the focused / selected star is possible. For interactive rating, ensure that focus indication is visible when navigating by keyboard and shows which star currently has focus.
3. Enable automatic dark mode rendering [^8] (applying a forced color mode of a light foreground on dark background). Confirm that the star icons are all visible and indicate a filled or empty state. While in dark mode, ensure that focus indication is visible when navigating by keyboard and shows which star currently has focus.
4. Using a screen reader, move focus into the interactive rating star group. Ensure that the screen reader announces that the items are "stars," which number star currently has focus, if the currently focused star is selected or unselected, and what the total number of stars is.
5. With JavaScript disabled, ensure it is possible to see the star rating value. For interactive rating, ensure it is possible to enter and submit a rating.
6. For this pattern, check each star’s interactive area against the 44 × 44 CSS pixel design target.
7. With the text size enlarged to 200% of its default size, ensure that the touch target area scales in relation to the surrounding text. Ensure that the star icons cover the entire touch target area for interactive rating.
8. Ensure that the focus indication and the filled-in status of each star do not depend solely on a color difference and that the contrast of the indication is sufficient.

[^1]: [_WCAG 2.1 Understanding Docs_, Target Size (Level AAA)](https://www.w3.org/WAI/WCAG21/Understanding/target-size.html)
[^2]: [_CSS Background Images and Accessibility_, Level Access](https://www.levelaccess.com/blog/css-background-images-and-accessibility/)
[^3]: "In WebAIM's 2018 low vision survey, 30.6% of respondents used a high contrast mode." [assistivlabs "High Contrast Mode"](https://assistivlabs.com/assistive-tech/display/high-contrast-mode)
[^4]: [_We still can't use the HTML range input, either_](https://jamescatt.ca/blog/2020/we-still-cant-use-the-html-range-input-either), James Catt
[^5]: [_filter_, mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)
[^6]: [w3C Aria Practices](https://w3c.github.io/aria-practices/examples/slider/slider-rating.html)
[^7]: Versions of the Chrome (and Edge browsers) later than 98 allow you to set `forced-colors` to "active" in the "Rendering" pane of DevTools.
[^8]: [_Auto Dark Theme_, Chrome Developer's Blog](https://developer.chrome.com/blog/auto-dark-theme/#test-auto-dark-theme-on-your-device)
