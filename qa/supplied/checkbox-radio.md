# Check­box and Ra­dio But­ton Test Cri­te­ria

ACT for custom-styled, accessible checkboxes and radio buttons.

On this page

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for custom-styled checkboxes and radio buttons, focusing on maintaining accessibility for users relying on assistive technologies. While visually customized, these controls must comply with W3C WCAG 2.2 standards to ensure accessibility for all users.

## Accessibility Requirements

The Checkbox and Radio Button components are evaluated against WCAG 2.2 success criteria, organized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships must be programmatically defined to ensure accessibility.
- 1.4.1 Use of Color: Avoid using colour alone to convey information regarding states like "checked" or "unchecked."
- 1.4.3 Contrast (Minimum): Ensure text and icons meet the minimum contrast ratio of 4.5:1 against their backgrounds.
- 1.4.11 Non-text Contrast: Indicators (e.g., borders for selected states) must have a contrast ratio of at least 3:1.

### Operable
- 2.1.1 Keyboard: All functionality must be operable via keyboard alone.
- 2.1.2 No Keyboard Trap: Focus can be moved into and out of the checkbox or radio button groups without trapping the user.
- 2.4.3 Focus Order: The focus order must be logical, predictable, and consistent with user expectations.
- 2.4.7 Focus Visible: A visible focus indicator must be present on each control when focused.
- 2.5.3 Label in Name: The accessible name of each control should match or include its visible label text.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state (checked/unchecked, disabled, etc.).

## ACT Test Cases

### Test Case 1: Keyboard Navigation and Focus Management / Visibility and Contrast of Focus Indicators

#### Objective

To verify that the checkbox and radio button controls are fully operable and navigable using only the keyboard.

To confirm the visibility and contrast of focus indicators on checkbox and radio button controls.

#### Test Steps
1. Load the page and navigate to each checkbox and radio button using the tab key.
2. Assess the focus indicator's visibility and contrast against the background.
3. Optionally, use a colour contrast tool to measure the indicator's contrast ratio.
4. For radio buttons, use the up and down arrow keys to move focus within the group.
5. Use the space key to toggle the checked state of each checkbox or radio button.

#### Expected Results
- All controls are fully operable via keyboard, with checkboxes toggled by space and radio buttons navigable with arrow keys.
- Focus indicators are clearly visible and have a contrast ratio of at least 4.5:1.

### Test Case 2: Screen Reader Announcements for Control Labels and States

#### Objective

To confirm that each control's label, role, and state (checked/unchecked, disabled) are accurately announced by screen readers.

#### Test Steps
1. Enable a screen reader and navigate to each checkbox and radio button.
2. Listen for announcements of the control's label, role (e.g., "checkbox"), and state (checked/unchecked, disabled).

#### Expected Results
- The screen reader announces each control's label, role, and current state accurately, including any "mixed" or "indeterminate" states.

### Test Case 4: Accessible Operation Using Voice Control

#### Objective

To verify that checkboxes and radio buttons can be toggled by voice commands for users with voice control software.

#### Test Steps
1. Use voice control software to activate each checkbox and radio button using voice commands that reference the visible labels (e.g., "click checkbox label").
2. Verify that the voice command toggles the control's state as expected.

#### Expected Results
- Each checkbox and radio button responds to voice commands, toggling the state when activated by label.

### Test Case 5: Compliance with High Contrast and Forced Color Modes

#### Objective

To ensure the visibility and operability of checkboxes and radio buttons in high contrast or forced colour display modes.

#### Test Steps
1. Enable a high contrast or forced colour mode in the operating system or browser.
2. Inspect each control for visibility and state indication.
3. Ensure that changes in state (checked/unchecked) are still perceivable without reliance on background colours alone.

#### Expected Results
- Each control's state is perceivable, with sufficient contrast and no reliance on background colours to indicate state.

### Test Case 6: Responsiveness and Content Scaling

#### Objective

To confirm that checkboxes and radio buttons remain usable when text size is increased.

#### Test Steps
1. Increase text size to 200% in the browser settings.
2. Verify that the controls remain fully visible and functional, with labels visually connected to the controls.

#### Expected Results
- Checkbox and radio button controls are accessible and readable at increased text sizes, with no visual disconnection from associated labels.

### Test Case 7: Accessible Fallback for Disabled JavaScript

#### Objective

To ensure that checkboxes and radio buttons remain accessible if JavaScript is disabled.

#### Test Steps
1. Disable JavaScript in the browser settings.
2. Reload the page and inspect each control for usability and accessibility without JavaScript enhancements.

#### Expected Results
- All controls remain accessible and functional without JavaScript, with appropriate fallback styles.
