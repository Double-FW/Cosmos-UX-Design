---
title: Forms
description: "Ensure that form interactions and validation are accessible."
---

## Summary

Forms are an important part of the web pages on which they appear but can be challenging for users with visual and / or cognitive disabilities. Ensuring that forms are well-designed and accessible will help users understand and use those forms correctly.

## Purpose

The study, ["Designing Usable Web Forms"](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/42513.pdf), found that when forms apply good usability guidelines, users were able to complete the forms more quickly and with less errors from the first try: 78% first-try submissions succeeded in forms following good usability guidelines, versus only 42% first-try submissions in forms not following them.

Aside from being frustrating, poorly designed or implemented forms can have serious exclusionary failings. Users with disabilities who cannot complete and submit your form are unlikely to remain your customers and may very well have a case to pursue for illegal discrimination against you.

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Does each form input item have a semantically valid HTML `label` element associated with it (clicking on the label should select the associated form input element)? [Y/n]

### Checks which should be (at least partly) performed manually

- Can the entire form be completed, including making corrections of possible errors, and submitted using only the keyboard? [Y/n]
- Is the tabbing order through the form (as shown by a visible focus indicator) logical and predictable to a sighted keyboard user?  [Y/n]
- Are required input fields visibly marked as such, and do they use a `required` attribute on the input element?  [Y/n]
- Are error messages displayed in a way that makes it clear which input fields have rejected input, why the input was not accepted, and what to do to correct it? [Y/n]

## Form markup

Use [HTML Form Elements](HTML%20Form%20Elements.md) for labels, grouped fields and associated instructions. Use [Labels](Labels.md) to check names with assistive technology.

## Further Advice

### Reset

Avoid adding [a Reset Button](https://www.nngroup.com/articles/reset-and-cancel-buttons/) on forms. They are hardly ever useful and it can be infuriating to have an entire completed form cleared with just one accidental click.

Note that without a Reset button, radio button groups may be a problem in certain cases. One of their traits is that you cannot _un-select_ the group once any option has been selected in that group; you can only _move_ the selection to another radio button in the same group. For this reason it is recommended that a "none of the above" type of option should be included where appropriate.

### Validation

When validating a form, ensure that the original value entered by the user which caused the error is not cleared. By preserving the user's original value the user can more easily see what their mistake was and then to make a fix by editing it.

Using WAI ARIA attributes can help make validation errors easier to follow by users of assistive technologies. Use the `aria-invalid` attribute to mark each invalid input field as having an error, and the `aria-describedby` attribute to associate the specific error message to that input field.

```html
<label for="uname">What user name would you like to use?</label>
<input id="uname" type="text" name="uname" aria-describedby="uname-error" aria-invalid="true" />
<div role="alert" id="uname-error">Error: That user name is already taken. Try another.</div>
```

### Error messages

It should be considered a defect when error messages are not displayed at all. When a form submission is rejected and there is no feedback explaining why or what needs to be corrected in order to continue, the form becomes unusable.

Error messages should be concise and specific in explaining why the form submission was rejected. Users will avoid reading long strings of technical jargon. However, error messages can still explain why the information, as entered, could not be accepted.

Check that all error messages that can be displayed meet the following requirements:

- Specific: Simply saying "there was an error" or "the form is invalid" is not useful. Which input field, or fields if there are more than one, was invalid, and why was it rejected?
- Friendly: Even if the user has made a mistake, the experience should feel helpful and polite. Avoid a tone that sounds aggressive, like "user error: illegal input." This can be triggering to users with mental health or anxiety disorders. On the other hand, don't try to be too glib either. Assume the user is feeling frustrated and wants help at that moment, and a flippant phrasing to the feedback you offer is probably not going to be appreciated. 
- Actionable: Communicating that there is a problem and what the problem is is only part of the job done. The user must also know what steps they can take to fix the problem. Check that there is some information on what they need to do next in order to move forward.

## Correcting a field

Associate persistent instructions and errors with their field. After validation identifies an error, expose the invalid state. Clear the error association and invalid state when the input becomes valid. Preserve any separate hint text.

### ACT-style checks

Rule identifier: `gel-components-form-fields-validation`

Run on an editable field that has instructions and supports validation. Compare its initial, invalid and corrected states.

1. Start with a fresh form.
2. Enter an invalid value, trigger validation, then correct it.
3. Check the label remains visible, the message identifies the field and the correction, and an obsolete error is no longer displayed or announced.
4. See [Labels](Labels.md) for the naming checks.
