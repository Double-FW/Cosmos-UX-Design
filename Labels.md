---
title: Labels
description: "Use accessible labels and names for interactive elements."
---

## Summary

Form controls and other interactive elements must be labeled properly. Failing to do so will make it difficult for assistive technology users to understand the purpose of the control. Missing and mislabeled interactive elements can lead to issues or confusion for your users.

## Purpose

The [Web Content Accessibility Guidelines](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-cues.html) state that all interactive elements should have both a label and an accessible name. The label is presented visually and the accessible name is used by assistive technologies like screen readers. Often the two are the same; for example if a link has the text "Home", such as `<a href="/home">Home</a>`, the text "Home" is the label _and_ the accessible name. Similarly a button containing the text "Send" has both a visual label and an accessible name of "Send".

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which should be (at least partly) performed manually

- Does each form item have a clear visible label that identifies the expected input? Where a conventional icon is used, does it have an equivalent accessible name? [Y/n]
- Is the label near to the control it describes, so that it is obvious which control it is referring to? [Y/n]
- Is the label still present and visible when the control has focus? [Y/n]
- If the control is a group of selectable options, such as a dropdown menu or list of radio buttons or checkboxes, is there a visible text label describing the collection of options? [Y/n]
- When the control is encountered by a screen reader, is the label read aloud as expected? [Y/n]

## Examples and Additional Information

The three aspects of labels and accessible names that must be correctly implemented are based on the user and how they interact with the web page. Having a visual label is obviously needed to allow sighted user who navigate by using a cursor or mouse to know what interactive elements are designed to do: clicking the "Close Tab" button, or tabbing to the"Support Page" link are examples.

By providing those labels in accessible ways (using text according to HTML standards) you will also serve users of screen reader devices, which will use that text to speak the accessible name to the user.

In some cases however it is possible to apply a visible label that is different than the accessible name. This creates potential problems for users who use voice control and speech recognition to navigate, such as by saying "click Send". These users will see the label and use that to direct their assistive technology, which in turn will rely on the accessible name. For this reason it is recommended to keep the visible label the same as the accessible name.

## Further Advice

### Keep labels visible when people type

Use the [form-control labelling examples](HTML%20Form%20Elements.md#visible-labels) for implementation. Test that the label remains visible when the field has focus and contains a value.

### Don't add a title attribute

A title attribute is sometimes added to form input elements. While it is true that most screen reader devices will read out the title attribute, there is no benefit to this: using the more semantic `<label>` element would also work and comes with the benefit that the label can be clicked to focus the input. Having both a label and a title attribute of course means that _both_ those strings of text may be read out leading to useless duplication.

```html
<!-- DO NOT add a title attribute text to an input element -->
<form>
First name: <input name="first-name" type="text" title="First Name">
</form>
```

### Hiding parts of labels from screen readers

In some cases, specific parts of the text used in a label has a conventional meaning, but only when viewed visually. For example, the asterisk is often used to indicate to the user that a form item is required. While some screen readers may read out the asterisk, some screen readers will not. Whether or not the user would understand that a form item is required even if they could hear "Name asterisk" for example is uncertain.

It is acceptable to hide the asterisk from screen readers if the information is made clear in an alternative way. In the example below, simply using the native HTML `required` attribute on the input field is reliable and screen readers will pick up on this and announce it.

```html
<label for="address">Address<span aria-hidden="true">*</span></label>
<input id="address" name="address" type="text" required />
```

### Hiding parts of labels from _non_ screen readers

 A label is often implemented using body text but may be an element with alternative text, like an image, assuming the image is a well-understood icon. When a label is visible only as an icon, alternative text must be provided for users who cannot see the icon. Ideally the control would make that text visible for everyone along with the icon, but design limitations may not make this ideal. In that case, providing text as a label which is only accessible to a screen reader is acceptable, assuming the icon is well-known and understood to sighted users without having the text cue (a "magnifying hand lens" icon for example, as the label to a search form).

The example below uses one of the techniques from [Functional Alternative Text](Functional%20Alternative%20Text.md). These text alternatives will naturally be hidden from sighted users looking at the image.

```html
<form>
    <label class="search-label" for="search" aria-labelledby="searchLabelTitle">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 120">
            <title id="searchLabelTitle">Search by product name.</title>
            ...
        </svg>
    </label>
    <input id="search" type="text">
    <input class="button" type="button" value="Search">
</form>
```
