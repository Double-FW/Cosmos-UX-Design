# Nav­i­ga­tion Menu Test Cri­te­ria

ACT for Navigation Menu components.

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for a navigation menu component that expands and collapses to reveal a list of links for site navigation. Navigation menus must meet W3C WCAG 2.2 standards to ensure accessible interactions for all users, including those using assistive technology.

## Accessibility Requirements

The Navigation Menu component is evaluated against WCAG 2.2 success criteria, categorized by the Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships must be programmatically defined, ensuring navigation menus are accessible to assistive technology.
- 1.4.1 Use of Color: The component must avoid using color alone to indicate expanded or collapsed states.
- 1.4.3 Contrast (Minimum): Text and any visual indicators must meet a contrast ratio of at least 4.5:1.
- 1.4.11 Non-text Contrast: Visual status indicators (e.g., arrow or caret) must have a contrast ratio of at least 3:1 against adjacent colors.

### Operable
- 2.1.1 Keyboard: The Navigation Menu must be fully navigable and operable using only the keyboard.
- 2.1.2 No Keyboard Trap: Focus must be able to enter and exit the navigation menu without trapping the user.
- 2.4.3 Focus Order: Focus order must be logical and consistent with user expectations.
- 2.4.7 Focus Visible: A visible focus indicator must be present on each navigation menu control when focused.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state (expanded/collapsed).

## ACT Test Cases

### Test Case 1: Keyboard Navigation and Focus Management / Visual and Functional Accessibility of Focus Indicators

#### Objective

To verify that the Navigation Menu is fully navigable and operable using only the keyboard.

To confirm the visibility and usability of focus indicators on the navigation menu control for keyboard users.

#### Test Steps
1. Load the page and navigate to the navigation menu control using the tab key.
2. Assess the visibility and contrast of the focus indicator against the background.
3. Optionally, use a color contrast tool to measure the indicator's contrast ratio.
4. Use the enter or space key to toggle the expanded state.
5. Once expanded, continue pressing tab to move through each link within the menu.
6. Press escape to close the menu and confirm that focus returns to the control.

#### Expected Results
- Pressing enter or space toggles the menu's expanded state.
- Pressing escape closes the menu and returns focus to the control used to open it.
- Focus indicators are clearly visible and distinguishable from the background, meeting a minimum contrast ratio of 4.5:1.

### Test Case 2: Screen Reader Announcements for Labels and States

#### Objective

To confirm that screen readers correctly announce the navigation menu control and its expanded or collapsed state.

#### Test Steps
1. Enable a screen reader and navigate to the navigation menu control.
2. Listen for announcements of the control's label and current state (expanded/collapsed).
3. Toggle the state and listen for updates.

#### Expected Results
- The screen reader announces the accessible name of the navigation menu control and its state (expanded or collapsed).
- Toggling the control updates the state announcement correctly.

### Test Case 4: Accessible Toggle of Menu by Voice Control

#### Objective

To verify that the navigation menu control can be toggled by voice commands, supporting users of voice control software.

#### Test Steps
1. Using voice control software, instruct the software to "click" on the navigation menu control using its label text.
2. Confirm that the command successfully toggles the expanded/collapsed state of the menu.

#### Expected Results
- The navigation menu responds to voice commands, expanding or collapsing as expected.

### Test Case 5: High Contrast Mode Compliance

#### Objective

To ensure that the navigation menu control and its content are visible and usable in high contrast or forced color display modes.

#### Test Steps
1. Enable high contrast or forced color mode in the operating system or browser.
2. Verify the visibility of the navigation menu control and any expanded content.

#### Expected Results
- The navigation menu remains fully accessible in high contrast mode, with clear contrast on controls and content.

### Test Case 6: Responsiveness and Content Scaling

#### Objective

To confirm that the navigation menu remains accessible when text size is increased.

#### Test Steps
1. Increase text size to 200% in the browser settings.
2. Verify that the navigation menu control and its links remain readable and fully functional at this size.

#### Expected Results
- All navigation menu content remains accessible and readable, with no overlap or disconnection from associated labels.

### Test Case 7: JavaScript Independence

#### Objective

To ensure that the navigation menu remains accessible if JavaScript is disabled.

#### Test Steps
1. Disable JavaScript in the browser.
2. Reload the page and inspect the navigation menu's content.

#### Expected Results
- The menu content is visible and accessible when JavaScript is disabled, or a suitable non-interactive fallback is provided.
