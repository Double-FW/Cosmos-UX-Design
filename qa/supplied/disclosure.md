# Dis­clo­sure Test Cri­te­ria

ACT for Disclosure components.

On this page

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for a Disclosure web component. A Disclosure pattern hides non-essential content by default, allowing users to expand and collapse it as needed. This pattern can improve page usability but must be implemented accessibly to ensure users relying on assistive technology can navigate and interact with it effectively. This document ensures the Disclosure component meets WCAG 2.2 requirements, enhancing accessibility for all users.

## Accessibility Requirements

The Disclosure component is evaluated against WCAG 2.2 success criteria, organized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships must be programmatically defined to ensure accessibility.
- 1.4.1 Use of Color: The component must avoid using color alone to convey expanded or collapsed states.
- 1.4.3 Contrast (Minimum): Text and any visual status indicators must meet a contrast ratio of at least 4.5:1 against the background.
- 1.4.11 Non-text Contrast: Visual elements indicating the disclosure state must have a contrast ratio of at least 3:1.

### Operable
- 2.1.1 Keyboard: The Disclosure must be fully navigable and operable using only the keyboard.
- 2.1.2 No Keyboard Trap: Focus must be able to enter and exit the Disclosure without trapping the user.
- 2.4.3 Focus Order: Focus order must be logical and consistent with user expectations.
- 2.4.7 Focus Visible: A visible focus indicator must be present on the Disclosure control when focused.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state (expanded/collapsed).

## ACT Test Cases

### Test Case 1: Keyboard Navigation and Focus Management / Visual and Functional Accessibility of Focus Indicators

#### Objective

To verify that the Disclosure component is fully navigable and operable using only the keyboard.

To confirm the visibility and usability of focus indicators on the Disclosure control for keyboard users.

#### Test Steps
1. Load the page and navigate to the Disclosure control using the tab key.
2. Assess the focus indicator's visibility and contrast against the background.
3. Optionally, use a colour contrast tool to measure the indicator's contrast ratio.
4. Use the tab key to move focus to the next element after the Disclosure.
5. Use the space or enter key to toggle the expanded state.

#### Expected Results
- All Disclosure content is navigable and operable via keyboard alone, with expanded and collapsed states toggled using space or enter.
- Focus indicators are clearly visible with a contrast ratio of at least 4.5:1 against the background.

### Test Case 2: Screen Reader Announcements for Labels and States

#### Objective

To confirm that Disclosure controls and their states are accurately announced by screen readers.

#### Test Steps
1. Enable a screen reader and navigate to the Disclosure control.
2. Listen for announcements of the control's label and current state (expanded/collapsed).
3. Toggle the state and listen for updates.

#### Expected Results
- The screen reader announces the Disclosure control's label and current state (expanded/collapsed).
- Users hear an appropriate description of the Disclosure component's role, such as "toggle" or "disclosure."

### Test Case 4: Accessible Toggle of Disclosure Content by Voice Control

#### Objective

To verify that the Disclosure control can be toggled by voice commands, supporting users of voice control software.

#### Test Steps
1. Using voice control software, instruct the software to "click" on the Disclosure control using its label text.
2. Verify that the command successfully toggles the expanded/collapsed state.

#### Expected Results
- The Disclosure component responds to voice commands, toggling the state when activated by label.

### Test Case 5: High Contrast Mode Compliance

#### Objective

To ensure that the Disclosure control and its content are visible and usable in high contrast or forced colour display modes.

#### Test Steps
1. Enable a high contrast or forced colour mode in the operating system or browser.
2. Verify the visibility of the Disclosure control and ensure all content remains distinguishable when expanded.

#### Expected Results
- All Disclosure content remains accessible and distinguishable in high contrast mode, with sufficient contrast.

### Test Case 6: Responsiveness and Content Scaling

#### Objective

To confirm that Disclosure content remains accessible and readable when text size is increased.

#### Test Steps
1. Increase text size to 200% in the browser settings.
2. Verify that the Disclosure control and its content remain readable and fully functional at this size.

#### Expected Results
- All Disclosure content remains accessible and readable at increased text sizes, with no visual disconnection from associated labels.

### Test Case 7: JavaScript Independence

#### Objective

To ensure that the Disclosure component remains accessible if JavaScript is disabled.

#### Test Steps
1. Disable JavaScript in the browser.
2. Reload the page and inspect the Disclosure component's content.

#### Expected Results
- Disclosure content is visible and accessible when JavaScript is disabled, or a suitable non-interactive fallback is provided.
