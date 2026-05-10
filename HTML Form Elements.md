---
title: HTML Form Elements
description: Using basic HTML form elements, applied accessibly, remove barriers for users as they interact and provide feedback using web forms.
---

## Basic HTML form elements

Several basic HTML form input elements are used to collect information from users. These include:

* **text** - including related text-like inputs, including email, password, and tel, used for collecting short text input from users
* **textarea** - used for collecting longer text input from users
* **select** - used for collecting a single choice from a drop-down list of options
* **checkbox** - used for collecting multiple choices from a list of options
* **radio** - used for collecting a single choice from a list of options

There are a few rules to using each of these elements that will help ensure they are accessible to all users.

## Visible labels

All form elements must have a visible label. This label should be clearly associated with its form element by its position; it should be obvious which label belongs to which form input element. For pages presented in a left-to-right language, the label should be positioned above or to the left of its form element; the exception to this rule is the checkbox and radio button elements, for which the convention is for the visible label to be positioned to the right of the form element.

The visible label for a form element need not be text, but in most cases, it should be. In specific cases, an icon could be an appropriate label, for example, a magnifying glass icon, to indicate a search field. Avoid using an icon as a label if it is not widely conventional and immediately obvious what the icon represents or if the form contains multiple fields. When using an icon, you must provide a comparable text alternative to indicate the accessible name to users who may not be able to perceive or understand the icon.

{% aside "aside-warning" %}
The `placeholder` attribute is not a substitute for an HTML `label` element. Screen readers, and other assistive technologies, do not treat placeholders as labels, and they may not be supported at all in older assistive technologies.
{% endaside %}

## Accessible names

Every form element must have an accessible name that identifies its purpose; this is used by assistive technologies, such as screen readers, to identify the form element. The simplest way to provide an accessible name is to wrap the visible label in an HTML `label` element and associate it to the form element using the `for` attribute on the HTML `label` element and the `id` attribute on the form input element. The value of the `for` attribute must match the value of the form input element's `id` attribute exactly:

```html
<label for="customer-name-input">Name</label>
<input type="text" id="customer-name-input" name="customer-name">
```

{% aside "aside-info" %}
The `name` attribute on the HTML `input` element is used when processing the submitted form data but it does not create an accessible name in the web browser.
{% endaside %}

In cases where it is not possible to provide each form element with a unique `id` attribute, the associated visible label can be associated with the form element by wrapping them both within the same HTML `label` element:

```html
<label>
  Name
  <input type="text" name="customer-name">
</label>
```

As well as providing an accessible name, the benefit of using a `label` element is that clicking or tapping on the label automatically moves focus to its form element. This is particularly useful for touchscreen users who may not be able to tap on the form element itself accurately.

{% demoiframe title="Using a label element with a form element" %}
<label for="name">Name</label>
<input type="text" id="name" name="customer-name">
{% enddemoiframe %}

In rare cases, it may not be practical to use the recommended HTML `label` element, for example, when implementing some custom form input elements or due to design constraints where a visible label would make the layout or visual style of the input confusing. In such cases, the accessible name can be provided using either the `aria-label` attribute or the `aria-labelledby` attribute on the form element.

The value of the `aria-label` attribute should be the same as the visible label or include the visible label text verbatim:

```html
Name
<input type="text" name="customer-name" aria-label="Name">
```

The value of the `aria-labelledby` attribute should be the same as the value of the `id` attribute of the visible label:

```html
<span id="name-label">Name</span>
<input type="text" name="customer-name" aria-labelledby="name-label">
```

## Grouping related form elements

When a form contains several related form input elements, presenting them as a group is helpful. This will help to make the form easier to navigate and understand. The simplest way to group form input elements is to use an HTML `fieldset` element, which visually and semantically groups related form input elements. The HTML `fieldset` element should include an HTML `legend` element that provides a visible label for the entire group of form elements.

```html
<fieldset>
  <legend>Address</legend>
  <label for="address-line-1">Line 1</label>
  <input type="text" id="line-1" name="address-line-1">
  <label for="address-line-2">Line 2</label>
  <input type="text" id="address-line-2" name="address-line-2">
  <label for="address-town">Town</label>
  <input type="text" id="address-town" name="address-town">
  <label for="address-county">County</label>
  <input type="text" id="address-county" name="address-county">
  <label for="address-postcode">Postcode</label>
  <input type="text" id="address-postcode" name="address-postcode">
</fieldset>
```

{% demoiframe title="Grouping form elements with a fieldset." %}
<fieldset>
  <legend>Address</legend>
  <label for="address-line-1">Line 1</label>
  <input type="text" id="address-line-1" name="address-line-1">
  <label for="address-line-2">Line 2</label>
  <input type="text" id="address-line-2" name="address-line-2">
  <label for="address-town">Town</label>
  <input type="text" id="address-town" name="address-town">
  <label for="address-county">County</label>
  <input type="text" id="address-county" name="address-county">
  <label for="address-postcode">Postcode</label>
  <input type="text" id="address-postcode" name="address-postcode">
</fieldset>
{% enddemoiframe %}

> Screen reader: "Line 1, edit text, Address, group"
> ...
> Screen reader: "end of, Address, group"

Grouping input elements is particularly useful for multiple checkboxes and radio buttons, as the question asked by the group of checkboxes or radio buttons should be provided as a visible label for the whole group. The legend should be positioned above the group of checkboxes or radio buttons.

```html
<fieldset>
  <legend>What are your favorite colors?</legend>
  <label>
    <input type="checkbox" name="favorite-color" value="red">
    Red
  </label>
  <label>
    <input type="checkbox" name="favorite-color" value="green">
    Green
  </label>
  <label>
    <input type="checkbox" name="favorite-color" value="blue">
    Blue
  </label>
</fieldset>
```

{% demoiframe title="Grouping radio buttons with a fieldset." %}
<fieldset>
  <legend>What is your favorite color?</legend>
  <label>
    <input type="radio" name="favorite-color" value="red">
    Red
  </label>
  <label>
    <input type="radio" name="favorite-color" value="green">
    Green
  </label>
  <label>
    <input type="radio" name="favorite-color" value="blue">
    Blue
  </label>
</fieldset>
{% enddemoiframe %}

A screen reader will announce the visible label for the group of form elements when the user enters the group but will not repeat it for every form element within the group:

> Screen reader: "What is your favorite color? group. Green, radio button, checked."

## Instructions and error messages

If a form field requires additional instructions or an error message, these should be presented alongside the visible label. It is conventional for instructions to appear after the visible label, before the form element, and for error messages to appear after the form element.

Any additional content must be programmatically associated with the field it relates to using. The simplest way to do this is to use the `aria-describedby` attribute. The value of the `aria-describedby` attribute should be the same as the value of the `id` attribute of the element containing the additional content:

```html
<label class="form-label" for="dob">Date of Birth</label>
<span class="form-help" id="dob-instructions">Please enter your date of birth in the format DD/MM/YYYY.</span>
<input type="text" id="dob" name="dob" aria-describedby="dob-instructions dob-error">
<span class="form-error" id="dob-error">Please enter a valid date of birth.</span>
```

The `aria-describedby` attribute takes a space-separated list of `id` values. This allows multiple elements to be associated with a form field. For example, a form field may have both instructions and an error message associated with it.

{% demoiframe
   title="Instructions and error messages associated with a form field.",
   head="<link rel=\"stylesheet\" href=\"/demos/atoms/html-form-elements.css\">"
%}
<label class="form-label" for="dob">Date of Birth</label>
<span class="form-help" id="dob-instructions">Please enter your date of birth in the format DD/MM/YYYY.</span>
<input type="text" id="dob" name="dob" aria-describedby="dob-instructions dob-error">
<span class="form-error" id="dob-error">Please enter a valid date of birth.</span>
{% enddemoiframe %}

> Screen reader: "Date of Birth. Edit. Please enter your date of birth in the format DD/MM/YYYY. Please enter a valid date of birth."

## HTML5 input types

The HTML5 specification includes additional input types for specific input types such as email, URL, number, range, date, and time. [^1]

```html
<label>Month: <input name="yourMonth" type="month"></label>
<label>Range: <input name="yourRange" type="range" name="range" min="0" max="100" step="10" value="0"></label>
<label>Color: <input name="yourColor" type="color"></label>
<label>Time: <input name="yourTime" type="time"></label>
```

{% demoiframe title="HTML input types" %}
<p><label>Month:&nbsp;<input name="yourMonth" type="month"></label></p>
<label>Range:&nbsp;<input type="range" name="yourRange" name="range" min="0" max="100" step="10" value="0"></label>
<p><label>Color:&nbsp;<input name="yourColor" type="color"></label></p>
<p><label>Time:&nbsp;<input name="yourTimeh" type="time"></label></p>
{% enddemoiframe %}

These input types have fairly good support by modern web browsers [^4] but present mixed benefits for assistive technology users. For example, while browsers may assist users by presenting specialized on-screen keyboards, sliders, date, and color pickers and may assist with input validation, users of older or more specialized software can find that their assistive technology is not fully compatible yet. [^2] This has led to some design teams recommending against their current usage. [^3]

As a compromise, you can provide the browser with a hint about the type of data you expect to be input while still retaining the universally supported input type "text" interface by using the `inputmode` attribute.

```html
<label>Enter your age: <input type="text" name="yourAge" inputmode="numeric" pattern="[1-9][0-9]{0,2}">
```

{% demoiframe title="HTML button types" %}
<form onsubmit="return false">
<p><label>What is your age, in years? (required)&nbsp;<input type="text" name="yourAge" inputmode="numeric" pattern="[1-9][0-9]{0,2}" required></p>
<p><button type="submit">Enter</button></p>
</form>
{% enddemoiframe %}

In this example, we use the `inputmode` attribute set to "numeric" to tell the browser that if it shows an on-screen keyboard, it should use a numeric keypad. The  `pattern` attribute is used to activate the browser's client-side validation; we provide it with a regular expression matching the pattern of the age-like number we expect.

## Buttons

Standard HTML `button` elements are more accessible to screen reader and keyboard-only users, as they are more likely to be recognized and supported by their assistive technologies.

```html
<button type="button">Button</button>
<button type="reset">Reset</button>
<button type="submit">Submit</button>
```

{% demoiframe title="HTML button types" %}
<button type="button">Button</button>
<button type="reset">Reset</button>
<button type="submit">Submit</button>
{% enddemoiframe %}

{% aside "aside-warning" %}
There is arguably little reason to use the button type `reset` (when was the last time _you_ needed to empty all the data you entered into a form by pressing a button?) However, the "reset" button can confuse keyboard users; activating it can accidentally delete the entire form input without warning.
{% endaside %}

### Submit buttons

A submit button should be provided for each form. The submit button should be positioned at the form's end and clearly labeled. The submit button should be an HTML `button` element or an HTML `input` element with a `type` attribute of `submit`.

```html
<form action="…">
  <label for="name">Name</label>
  <input type="text" id="name" name="name">
  <button type="submit">Submit</button>
</form>
```

The same approach should be taken for forms that are submitted using JavaScript rather than an HTML `form` element.

You must only submit a form when the user explicitly initiates it. For example, do not automatically submit a form when the user changes the value of an HTML `select` element. This is not an expected behavior and can cause issues for users of assistive technologies who may accidentally trigger the submission when filling out the form, causing data loss and frustration.

## Checkpoints for the Tester

1. Ensure each form element has a visible label that identifies the element's purpose.
1. Ensure that each form element has an accessible name that is either the same or includes, as an unbroken string within the accessible name, the text of the visible label.
1. Ensure that related groups of form elements are collected together using an HTML `fieldset` element.
1. Ensure that HTML `fieldset` elements have an HTML `legend` element as their first child, with visible text content that describes the group's purpose.
1. Ensure additional instructions and error messages are associated with the form element they relate to.
1. Ensure that each form has a submit button that is clearly labeled.
1. Ensure that forms are not automatically submitted when the user changes the value of an element.

[^1]: [_The input element_ Whatwg HTML Living Standard](https://html.spec.whatwg.org/multipage/input.html)
[^2]: For example, see the issue reported by Gov.UK of [incompatibility of input type of "email" and Dragon Naturally Speaking](https://github.com/alphagov/govuk-design-system/issues/2187) voice recognition software.
[^3]: [_Why the GOV.UK Design System team changed the input type for numbers_ Gov.UK Technology in government blog](https://technology.blog.gov.uk/2020/02/24/why-the-gov-uk-design-system-team-changed-the-input-type-for-numbers/)
[^4]: [_Date and time input types_ caniuse.com](https://caniuse.com/input-datetime)
