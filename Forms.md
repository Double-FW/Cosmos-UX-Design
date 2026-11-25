---
title: Forms
description: Help people enter information, correct errors and submit a web form.
---

# Forms

## Summary

Help people understand what to enter, review their answers and recover when something goes wrong. A label that disappears while someone types makes it harder to check an answer. An error that clears the original entry can make the person start over.

## Purpose

Use this guide to design and test the whole form journey. For field markup and accessible names, use the linked guides below. For background on how design choices affect successful submission, see [Designing Usable Web Forms](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/42513.pdf).

## Form markup

Use native controls suited to the information being collected. Give each field an accessible name and each editable field a persistent visible label. Put the label and relevant instructions where people can see them while editing, including when an on-screen keyboard is open. For an explicit label, match its `for` value to the control’s `id`. Use `name` for the submitted value’s key. Group related options with a `fieldset` and a `legend`. See [HTML Form Elements](HTML%20Form%20Elements.md) and [Labels](Labels.md).

Show which fields are required before submission and use the native `required` attribute where the control supports it. Keep format and other persistent hints visible and associate them using `aria-describedby`. When a field has a hint and an error, include both IDs in its description so the person can hear the correction and still access the instructions.

Help people reuse information they have supplied before. Use the [input purpose and autofill guidance](HTML%20Form%20Elements.md#input-purpose-and-autofill) for personal details, and keep fields editable after autofill. Choose [input types and keyboard hints](HTML%20Form%20Elements.md#html5-input-types) that make entry easier while supporting the keyboard and assistive technologies people use.

For a compact single-field search, see [Search field labels](Search.md#search-field-labels). Keep the general form label visible and use its wording in the accessible name so people can refer to the field by voice.

## Further Advice

### Reset

Omit a general [Reset button](https://www.nngroup.com/articles/reset-and-cancel-buttons/) unless users have a clear need to start over. If a radio group should allow people to withdraw a choice, offer a meaningful option such as ‘None of these’.

### Validation

Start a fresh form without `aria-invalid`. Validate on submission and give earlier feedback where it helps people fix a completed entry. If validating during typing, give people time to finish an answer and avoid repeatedly announcing intermediate errors. Use client-side validation to give convenient feedback. Validate submitted values on the server even when the browser has already checked them. Disable the browser’s validation UI with `novalidate` only when the form has an accessible alternative for reporting and recovering from errors.

When validation identifies an error, set `aria-invalid="true"` and associate the visible error with the field. Keep the entered values and leave the fields editable. Keep the submit control available so a person can attempt submission and find out what needs attention. During a pending request, communicate progress and prevent duplicate submissions as needed. If an entry passes a format check, describe that result accurately. A valid format can still contain incorrect information.

### Error messages

Identify the field, explain why the value was rejected and say how to correct it. Use visible text and keep error indicators clear in forced colours. See [the colour guide](Color%20and%20Meaning.md).

When submission fails, make the failure and the next action easy to find. For several errors, provide a summary with links to the affected fields and move focus to the summary when that is the chosen recovery path. If focus stays on the submit control, make the error notification available without moving focus. Avoid announcing the same message through both focus and a live region. Follow [Dynamic Content](Dynamic%20Content.md#status-messages).

#### Markup example

This fragment shows a field after a failed validation attempt. The host form provides submission, validation and any error summary or announcement logic.

```html
<label for="account-name">Account name (required)</label>
<p id="account-name-hint">Use letters and numbers only.</p>
<input id="account-name" name="accountName" type="text" required
       value="river walk" aria-invalid="true"
       aria-describedby="account-name-error account-name-hint">
<p id="account-name-error">Error: Remove the space from your account name.</p>
```

## Correcting a field

When validation confirms that a field is corrected, set `aria-invalid="false"` or remove the attribute. Remove the obsolete text and its association. Keep separate hints linked to the field. Update any summary so it reflects the remaining errors. On another submission, confirm success only after the service accepts the form. Distinguish a submission or connection failure from a problem with a field value.

### ACT-style checks

Rule identifier: `gel-components-form-fields-validation`

Check a fresh form, a form with validation errors and the corrected form. Use the project’s supported browsers and screen readers. Apply field instructions and error checks to the variants that use them.

| Method and action | Expected result |
| --- | --- |
| Inspect the accessible names of input fields and read-only fields that display a value. Navigate the form with a screen reader. | Each field has a meaningful accessible name that identifies its purpose. |
| Focus, fill and revisit each editable field. Check the field again when it is invalid. | Its visible label stays available in every state, including after entry. A placeholder can provide an example while the label remains visible. |
| Read any format hint or instruction. Focus the field and inspect its accessible description before and after entering a value. | The instruction remains visible and programmatically associated with the field. |
| Inspect a fresh field, submit or otherwise validate an invalid value, and then correct it. | The fresh field has no authored `aria-invalid` state. It exposes `aria-invalid="true"` when validation finds an error. Once revalidated as correct, it exposes no invalid state. |
| Find the visible error, then move focus to the field and listen to its description. | The message identifies the problem and the next step. The field exposes the associated error and retains any persistent hint. |
| Correct the field and trigger validation. Revisit it and any error summary. | The obsolete error is gone visually and is no longer associated with the field or announced. The hint and any remaining errors are still available. |

## Checks

### Checks which could be automated

Detect missing or empty accessible names and broken ID references. Use manual checks to decide whether a name or description conveys the right information.

### Checks which should be (at least partly) performed manually

1. Complete, correct and submit the form using the keyboard. Check that focus is visible and follows a logical order, and that required fields are identifiable before submission.
2. Submit with several errors. Verify that entered values remain and each error can be located from the summary or submission feedback. Correct one field and check that the others still show their errors.
3. Simulate a failed request and retry. Check that the draft remains available and success is communicated only after acceptance.
4. Where autofill or a specialised input is offered, check that people can enter and correct values manually. Confirm that autofill puts the expected information in the right fields when it is enabled.
5. Increase text size and test a narrow viewport separately. Repeat with forced colours. Check that labels, instructions, focus and errors remain visible and usable.

## Sources

- [Problems with using placeholders for labels](https://www.smashingmagazine.com/2018/06/placeholder-attribute/)
- [Native form validation](https://medium.com/samsung-internet-dev/native-form-validation-part-3-8e643e1dd06)
- [Problems with floating labels](https://medium.com/simple-human/floating-labels-are-a-bad-idea-82edb64220f6)
- [Visually hidden text example](https://gist.github.com/Heydon/c8d46c0dd18ce96b5833b3b564e9f472)
- [WCAG 2.1 1.4.1 Use of Color](https://www.w3.org/TR/WCAG21/#use-of-color)
- [Problems with disabled submit buttons](https://axesslab.com/disabled-buttons-suck/)
