---
title: Labels
description: Help people identify and operate controls using visible labels and accessible names.
---

# Labels

Give each control a clear purpose so people can find it on screen, recognise it with a screen reader and refer to it by voice. If a button says ‘Send’ but its accessible name is ‘Submit’, someone saying ‘Click Send’ may be unable to activate it.

## Guidance

| Information | What it does |
| --- | --- |
| Visible label | Identifies the control on screen. Keep it close to the control and available while people use it. |
| Accessible name | Identifies the control to assistive technology. For example, a native button usually gets its name from its text. A form field can get its name from an associated HTML label. |
| Accessible description | Adds context, such as the expected format or how to correct an error. Keep the control’s name short and use `aria-describedby` to associate additional information where needed. |

For user interface components, the name and role must be programmatically determinable under [WCAG 2.1 4.1.2 Name, Role, Value](https://www.w3.org/TR/WCAG21/#name-role-value) (Level A). When a control’s visible label contains text or an image of text, its accessible name must include the visible wording under [WCAG 2.1 2.5.3 Label in Name](https://www.w3.org/TR/WCAG21/#label-in-name) (Level A). Use the visible wording as the name where possible. If extra words are needed, put the visible wording first.

Provide labels or instructions when people need to enter information under [WCAG 2.1 3.3.2 Labels or Instructions](https://www.w3.org/TR/WCAG21/#labels-or-instructions) (Level A). Prefer persistent text labels for fields. Keep any required-field marker understandable before entry and expose the requirement with the native `required` attribute where supported. If a label includes a decorative asterisk, the required state can carry that information for assistive technology. See [Forms](Forms.md#form-markup).

## Implementation

### Field and group labels

Use the [HTML Form Elements guidance](HTML%20Form%20Elements.md#accessible-names) to associate a visible label with its field. Give a select its own label. Use a [fieldset and legend](HTML%20Form%20Elements.md#grouping-related-form-elements) for related radio buttons or checkboxes, and label each option. Put persistent hints beside the field and follow [Forms](Forms.md#validation) for association and removal of validation errors.

This markup fragment shows a field with a name and a separate description. Both the label and hint remain visible while the field contains a value.

```html
<label for="collection-code">Collection code</label>
<p id="collection-code-hint">Enter the six characters from your confirmation email.</p>
<input id="collection-code" name="collectionCode" aria-describedby="collection-code-hint">
```

A placeholder can give an example. Keep it supplementary to the label and leave essential instructions visible. Keep `title` supplementary too. People need to be able to identify a control without waiting for a tooltip.

### Icon-only controls

Prefer visible text alongside the icon. For a familiar icon used alone, give the control an accessible name that identifies its action or destination before any tooltip opens. Follow [Icons](Icons.md#icons-and-accessible-names) for icon alternatives and [Tooltips](Tooltips.md) for supplemental descriptions. For the compact single-field search variant, follow [Search field labels](Search.md#search-field-labels).

## Checks

Use the project’s supported browsers and assistive technologies. Record the control, its visible label, its computed accessible name and the result.

| Method and action | Expected result |
| --- | --- |
| Identify each control visually. Focus and fill editable fields, then revisit them after validation. | The label is close enough to identify its control and stays available while the field is focused or contains a value. A required field is identifiable before submission. |
| Inspect the accessible name and navigate to the control with a screen reader. Compare the result with any visible text. | The name identifies the purpose and includes the visible wording when the label contains text. An icon-only control has a meaningful name even with its tooltip closed. |
| With voice control enabled, use the supported command for the visible words, such as ‘Click Send’. | The words identify the expected control. If several controls have the same label, there is a usable way to distinguish them. |
| Enter a group of related options and navigate between them. | The group’s purpose and each option’s name are available. A select exposes its own label. |
| Focus a field with a hint, trigger a validation error and correct it. | The label identifies the field throughout. Relevant instructions and errors are available separately from its name, and the outdated error is cleared after correction. |

## Sources

- [Understanding labels or instructions](https://www.w3.org/TR/UNDERSTANDING-WCAG20/minimize-error-cues.html)
