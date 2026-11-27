---
title: "Rating Stars"
description: "Display a rating or let people choose a value on a star scale."
---

# Rating Stars

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a star rating when people need to read an existing rating or give one of their own. In both cases, make the value and scale clear in text. If stars are the only cue, someone who cannot see or distinguish the filled shapes may miss the score or the value they selected.

## Recommended markup

### Displaying rating stars

Show the score and maximum together, such as ‘Average rating 3 out of 5’. Add the number of ratings when it helps people interpret an average. If nobody has rated the item, use ‘No ratings yet’. Use the same precision in the text and graphic so a fractional rating does not suggest a different score.

### Customizing display stars

Keep the visual stars next to the value. When the stars repeat the visible text, hide the graphic from assistive technology. Use a shape, fill pattern or outline to distinguish filled and empty stars if readers use the graphic to compare scores. Keep the score readable if the artwork fails to load. See [Icons](Icons.md) and [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md).

```html
<p>
  <span aria-hidden="true">★★★☆☆</span>
  Average rating 3 out of 5 from 18 ratings
</p>
```

### Supporting internationalization

Localise the score, units and labels. Use the surrounding document’s language and direction, or declare them for the component when they differ. Use logical layout properties and test the visual sequence in right-to-left layouts. See [Page Metadata](Page_Metadata.md#declaring-a-web-page-language).

## Recommended layout

### Touch target size

Use 44 × 44 CSS pixels as the design target for each interactive star option, including its clickable label. This draws on [WCAG 2.1 Target Size (Level AAA)](https://www.w3.org/WAI/WCAG21/Understanding/target-size.html). Check the effective pointer area rather than the size of the star drawing. For the WCAG 2.2 minimum and spacing exceptions, use [the pointer target guidance](Call%20to%20Action.md#give-pointer-users-room-to-activate-the-action).

## Recommended behaviour

### Interactive star rating

Use the rating to collect one value from a fixed scale. Give the group a question that identifies what is being rated. Label each option with its value and keep the scale visible. For example, show ‘Your rating out of 5’ with options from ‘1 star’ to ‘5 stars’.

### Use web form elements

Use native radio buttons in a `fieldset` with a `legend` for a short discrete scale. Give every option the same form field name and its own associated label. Use Tab to enter and leave the group, arrow keys to move within it and Space to select the focused option. Let the person change their answer before submitting. Use an explicit submission action and keep any validation and confirmation available in text. If the question is optional and people need to withdraw a choice, provide a clear ‘No rating’ option or a way to clear it. See [HTML Form Elements](HTML%20Form%20Elements.md#grouping-related-form-elements) and [Forms](Forms.md#validation).

### Avoid background images

Keep the rating value and selection understandable when images or styles are unavailable. If you add stars to the labels, keep their text and use graphics such as inline SVGs that can follow the surrounding colour. Let the native radio show its checked state or provide an equally clear shape or mark for the selected value. Keep that cue distinguishable from keyboard focus in light and dark themes and forced colours. See [Focus](Focus.md#focus-indication).

## Reference implementation

### Example implementation

This example keeps labels and native radio buttons visible. It starts with no selection and needs no script. Replace the sample route with the service’s submission endpoint. The service validates the rating and shows the outcome.

```html
<form action="/reviews/rating" method="post">
  <fieldset>
    <legend>Your rating out of 5 (required)</legend>
    <div class="rating-options">
      <label class="rating-option"><input type="radio" name="rating" value="1" required> 1 star</label>
      <label class="rating-option"><input type="radio" name="rating" value="2"> 2 stars</label>
      <label class="rating-option"><input type="radio" name="rating" value="3"> 3 stars</label>
      <label class="rating-option"><input type="radio" name="rating" value="4"> 4 stars</label>
      <label class="rating-option"><input type="radio" name="rating" value="5"> 5 stars</label>
    </div>
  </fieldset>
  <button type="submit">Submit rating</button>
</form>
```

```css
.rating-options {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.rating-option {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  box-sizing: border-box;
  min-inline-size: 44px;
  min-block-size: 44px;
  padding-inline: 0.5em;
}

.rating-option:focus-within {
  outline: 2px solid currentColor;
  outline-offset: 2px;
}
```

### Supporting internationalization

Translate the legend and every option label. Keep the option order meaningful in the document so reading and keyboard navigation agree with the visual presentation. Test native arrow-key behaviour in the supported browser and direction. Keep the submitted values stable when translating labels.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

| Method and action | Expected result |
| --- | --- |
| Read a published rating with a screen reader and compare it with the graphic. Include a fractional rating and an unrated item. | The current score and maximum are understandable in text. Decorative stars add no speech, and an unrated item does not appear to have received a low score. |
| Tab into the group, move with the arrow keys, select with Space and Tab onward. Read the group with a screen reader. | The question, scale, focused option and checked state are available. Focus stays visible. Selection can change before submission and Tab can leave the group. |
| Use a pointer or touch to select adjacent values. Measure the clickable labels. | Each option is independently operable and its target meets the documented size. |
| Change themes and enable forced colours separately. Select a value, Tab to the submit button and Shift+Tab back to the group. | The selected value stays identifiable throughout. The focus indicator identifies the focused control separately, without depending on colour alone. |
| Block scripts, then test with CSS disabled separately. Submit no rating, then select and submit a value. | The display stays readable and the form remains usable. For the required example, a missing value can be corrected. The submitted value and confirmation agree. |
| Increase text size to 200%, then test a narrow width and a right-to-left locale separately. | Labels stay legible, groups can wrap and reading, visual and focus order remain meaningful. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)

## Sources

- [WCAG 2.1 Target Size](https://www.w3.org/WAI/WCAG21/Understanding/target-size.html)
- [CSS background images and accessibility](https://www.levelaccess.com/blog/css-background-images-and-accessibility/)
- [High contrast mode](https://assistivlabs.com/assistive-tech/display/high-contrast-mode)
- [Range input usability discussion](https://jamescatt.ca/blog/2020/we-still-cant-use-the-html-range-input-either)
- [CSS filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)
- [Rating slider example](https://w3c.github.io/aria-practices/examples/slider/slider-rating.html)
- [Testing automatic dark theme](https://developer.chrome.com/blog/auto-dark-theme/#test-auto-dark-theme-on-your-device)
