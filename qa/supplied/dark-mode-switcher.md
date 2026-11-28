# Dark Mode Switch­er Test Cri­te­ria

ACT for Dark Mode Switcher components.

On this page

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for a Dark Mode Switcher component. This feature allows users to toggle between light and dark themes or match the theme with their operating system's settings, improving accessibility for users with different visual preferences. This component must comply with W3C WCAG 2.2 standards to ensure accessibility for all users, including those relying on assistive technologies.

## Accessibility Requirements

The Dark Mode Switcher component is evaluated against WCAG 2.2 success criteria, organized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships must be programmatically defined to ensure accessibility.
- 1.4.1 Use of Color: Avoid using colour as the only method to convey information about the selected mode.
- 1.4.3 Contrast (Minimum): Ensure text and visual elements meet a minimum contrast ratio of 4.5:1 in the default theme.
- 1.4.11 Non-text Contrast: Visual indicators (such as icons representing light and dark mode) must have a contrast ratio of at least 3:1 against the background.

### Operable
- 2.1.1 Keyboard: The switcher must be fully operable via keyboard navigation alone.
- 2.1.2 No Keyboard Trap: Focus can be moved into and out of the switcher without trapping the user.
- 2.4.3 Focus Order: The focus order must be logical and consistent with user expectations.
- 2.4.7 Focus Visible: A visible focus indicator must be present on all switcher elements when focused.
- 2.5.3 Label in Name: The accessible name of each theme option should match or include its visible label text.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state.

## ACT Test Cases

### Test Case 1: Keyboard Navigation and Focus Management / Visibility and Contrast of Focus Indicators

#### Objective

To verify that the Dark Mode Switcher is fully operable and navigable using only the keyboard.

To confirm the visibility and contrast of focus indicators on the Dark Mode Switcher controls.

#### Test Steps
1. Load the page and navigate to the Dark Mode Switcher using the tab key.
2. Assess the focus indicator's visibility and contrast against the background.
3. Optionally, use a colour contrast tool to measure the indicator's contrast ratio.
4. Use the tab key to move between the switcher's options.
5. Use the space or enter key to select a theme option.

#### Expected Results
- All controls are fully operable via keyboard, with the selected theme applied immediately when activated.
- Focus indicators are clearly visible and have a contrast ratio of at least 4.5:1.

### Test Case 2: Screen Reader Announcements for Labels and States

#### Objective

To confirm that each switcher option's label, role, and state are accurately announced by screen readers.

#### Test Steps
1. Enable a screen reader and navigate to the Dark Mode Switcher.
2. Listen for announcements of each option's label, role (e.g., "dropdown"), and current state (selected/unselected).

#### Expected Results
- The screen reader accurately announces each theme option's label, role, and selected state.

### Test Case 4: Accessible Operation Using Voice Control

#### Objective

To verify that the Dark Mode Switcher is operable by voice commands for users with voice control software.

#### Test Steps
1. Use voice control software to activate each theme option using voice commands that reference the visible labels (e.g., "click Dark Mode").
2. Verify that the voice command changes the theme as expected.

#### Expected Results
- Each theme option responds to voice commands, changing the theme when activated by label.

### Test Case 5: High Contrast Mode Compliance

#### Objective

To ensure that the Dark Mode Switcher remains visible and usable in high contrast or forced colour display modes.

#### Test Steps
1. Enable a high contrast or forced colour mode in the operating system or browser.
2. Verify the visibility of the Dark Mode Switcher, ensuring that each theme option is clearly distinguishable.

#### Expected Results
- Each option within the Dark Mode Switcher remains visible and distinguishable, with sufficient contrast in high contrast mode.

### Test Case 6: Responsiveness and Content Scaling

#### Objective

To confirm that the Dark Mode Switcher remains accessible when text size is increased.

#### Test Steps
1. Increase text size to 200% in the browser settings.
2. Verify that the controls remain visible, with labels correctly associated and fully functional.

#### Expected Results
- The Dark Mode Switcher is accessible and readable at increased text sizes, with no visual disconnection from associated labels.

### Test Case 7: Default Theme Compliance and User Preferences

#### Objective

To verify that the Dark Mode Switcher correctly respects user-selected themes and reverts to the system preference if the "Auto" option is selected.

#### Test Steps
1. Choose each theme option (Light, Dark, and Auto) in turn, verifying that the correct theme is applied.
2. For the "Auto" option, set the operating system to alternate between dark and light modes.
3. Verify that the webpage automatically matches the OS theme in "Auto" mode.

#### Expected Results
- The correct theme is applied for each option, with "Auto" correctly reflecting the operating system's dark/light mode preference.
