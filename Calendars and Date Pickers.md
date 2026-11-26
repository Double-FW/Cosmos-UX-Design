---
title: "Calendars and Date Pickers"
description: "Help people enter known dates, find available dates and review the dates they choose."
---

# Calendars and Date Pickers

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Choose a date input based on what the person knows. Someone entering their date of birth should be able to type it directly. Someone booking an appointment may need to compare dates, see availability and understand which dates are unavailable.

| User task | Pattern |
| --- | --- |
| Enter a known date, such as a date of birth | Offer direct entry with labelled day, month and year fields, or a tested date input that supports direct entry. |
| Select from a short set of available dates | Use a dated list of links or choices. Include relevant times and locations. |
| Compare many dates or choose by weekday | Consider a calendar picker alongside an entry or availability-list route that supports the same task. |
| Read upcoming events | Use a chronological agenda. If you also show a calendar, keep both views in sync and make event details reachable from either. |

## Recommended markup

Use a visible label and associate any format hint with the input. For separate fields, group them in a `fieldset` with a `legend` that names the date. Label Day, Month and Year individually. Put them in the order expected for the service language and show an example. Use `autocomplete="bday-day"`, `bday-month` and `bday-year` when asking for the person's own date of birth. See [HTML Form Elements](HTML%20Form%20Elements.md#grouping-related-form-elements).

If using `input type="date"`, test the browser's entry and picker behaviour in the supported devices and assistive technologies. The displayed format can vary by browser locale. The form value uses `YYYY-MM-DD`. Keep a date without a time as a calendar date throughout the workflow so time-zone conversion cannot shift it to another day.

Name the start and end fields for a range, such as ‘Check-in date’ and ‘Check-out date’. Explain whether endpoints are inclusive where that affects the task. For a custom calendar dialog, use a named trigger and dialog. Implement the full [W3C date picker keyboard and grid pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/examples/datepicker-dialog/) if using its grid model. Follow [Modal Dialogs](Modal%20Dialogs.md) for entry, dismissal and focus return.

## Recommended layout

Put the format example, limits and availability information near the date control. If there is a calendar button, keep it next to the field it opens and give it a name that identifies the field. Keep the month and year visible above the grid. Give abbreviated weekday headings a full-name equivalent.

Distinguish today, the selected date, availability and keyboard focus. A person should be able to tell these states apart in text or through their exposed semantics, and visually without colour alone. Let date fields and controls wrap at narrow widths. Keep focus visible in forced colours and when a picker opens at high zoom. Where a month grid becomes cramped, keep the date entry or list route available. See [Focus](Focus.md#focus-indication).

## Recommended behaviour

### Enter and validate a date

Let people type, paste, review and correct a date. Keep the date they entered when validation fails. Distinguish missing parts, impossible dates, dates outside the permitted range and dates that are valid but unavailable. Explain how to correct each case. Validate on the server, including ordering of start and end dates and any rules about availability. Follow [Forms](Forms.md#error-messages) for accessible error feedback.

### Browse and select in a calendar

When opening a grid picker, place focus on the selected date if there is one. Otherwise use a meaningful starting date such as today or the first available date in the permitted period. Keep one day in the grid's Tab sequence and use the grid's arrow-key commands to move between dates. Follow the documented Home, End, Page Up and Page Down behaviour for the chosen model. Provide named month navigation controls that also work by touch and pointer.

Expose the full date and its state when someone navigates to it. Make a meaningful month change available through a concise announcement, without announcing the whole grid. Keep unavailable dates unselectable. If people need to discover that a date is unavailable, expose that state on a reachable day or through nearby text or the equivalent availability list. Use the same availability rules for typing and calendar selection.

Separate moving through dates from selecting one. Apply the selected value on the documented action. If Escape or Cancel dismisses the dialog, preserve the value that was already accepted. Restore focus to the opener or another logical target if the opener has gone. Keep booking confirmation separate from selecting a date.

### Keep availability current

Explain the time zone when dates are paired with times or a service deadline depends on it. Recheck availability when the person confirms. If a date becomes unavailable, keep other valid entries and help them choose again. For a date range, show both endpoints and any effect on availability or price before commitment. See [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md#show-what-the-choice-means).

## Reference implementation

### Enter a date of birth

This vanilla HTML fragment works without scripts. Put it inside the service form and validate the three fields together on the server. Adjust the order and example together for other locales.

```html
<fieldset aria-describedby="birth-date-hint">
  <legend>What is your date of birth?</legend>
  <p id="birth-date-hint">For example, 27 3 1985.</p>
  <p><label for="birth-day">Day</label>
    <input id="birth-day" name="birth-day" type="text"
      inputmode="numeric" autocomplete="bday-day"></p>
  <p><label for="birth-month">Month</label>
    <input id="birth-month" name="birth-month" type="text"
      inputmode="numeric" autocomplete="bday-month"></p>
  <p><label for="birth-year">Year</label>
    <input id="birth-year" name="birth-year" type="text"
      inputmode="numeric" autocomplete="bday-year"></p>
</fieldset>
```

### Choose from available dates

This server-rendered list works without JavaScript. Integrate it into a booking form and replace the illustrative options with current availability. If you add a calendar view, use the same date values and availability for both routes. Omit the group when there are no available dates and give a way to search another period.

```html
<fieldset>
  <legend>Choose a collection date</legend>
  <p><label><input type="radio" name="collection-date"
    value="2027-03-01" required> Monday 1 March 2027</label></p>
  <p><label><input type="radio" name="collection-date"
    value="2027-03-03" required> Wednesday 3 March 2027</label></p>
</fieldset>
<button type="submit">Review collection date</button>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="testing-notes"></a>

Use a fixture with dates on both sides of a month or year boundary, one unavailable date and a range where relevant. Record the browser, locale, device and assistive technology used.

| Method and action | Expected result |
| --- | --- |
| Enter a known date directly, then try an incomplete date, 31 April and a valid leap day in a permitted year. | The format and parts are clear. Impossible and incomplete dates get specific errors. Valid dates are retained and the error can be corrected without re-entry. |
| On supported desktop and mobile browsers, use a screen reader to enter a date and operate any picker. Repeat with another supported locale. | The input name, format and chosen value are understandable. The submitted date matches the intended day even when the displayed format changes. |
| For a custom dialog, open with a selected date and then with an empty field. Navigate across weeks and months with the documented keys. Select a date, reopen and cancel with Escape. | Focus starts in a useful place, day and month states are available, and focus returns predictably. Cancelling leaves the accepted value intact. |
| Try an unavailable date through both the picker and direct entry or list. Then make a selected date unavailable before confirmation. | Availability agrees across routes. The service explains the change, preserves other valid entries and offers another choice. |
| Where a range is supported, test the same day, reversed endpoints and boundary dates. | The result follows the stated range rules. Errors identify the affected field or combination and offer a correction. |
| Enlarge text, then separately test a narrow viewport, forced colours and blocked scripts. | Dates, controls and states stay distinguishable. A usable route to enter or find dates remains available without the custom calendar. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Autocomplete attribute has valid value (73f2c2)](https://www.w3.org/WAI/standards-guidelines/act/rules/73f2c2/proposed/)

## Sources

- [GOV.UK Design System: Date input](https://design-system.service.gov.uk/components/date-input/)
- [Carbon Design System: Date picker](https://www.carbondesignsystem.com/building-blocks/core/components/date-picker/guidelines)
- [W3C ARIA Authoring Practices Guide: Date Picker Dialog Example](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/examples/datepicker-dialog/)
- [Atomic A11y: Web date picker guidance and tests](https://github.com/charlietriplett/atomica11y-skills/blob/main/atomica11y-skills/accessible-web/date-picker.md)
- [MDN: HTML date input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date)
