# Tog­gle But­ton Test Cri­te­ria

ACT for accessible Toggle Buttons.

On this page
1. Introduction
2. Accessibility Requirements
3. ACT Test Cases

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for a toggle button component, also called a toggle switch. The switch allows users to select an on or off state, representing binary choices. Accessible toggle buttons must comply with WCAG 2.2 to ensure usability for all users, including those relying on assistive technology.

## Accessibility Requirements

The Toggle Button component is evaluated against WCAG 2.2 success criteria, categorized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships, including the on/off state and label, must be programmatically defined to ensure accessibility.
- 1.4.3 Contrast (Minimum): Text, icons, and visual indicators for the switch state must meet a minimum contrast ratio of 4.5:1.
- 1.4.11 Non-text Contrast: Any visual indicator of the switch state (e.g., on/off background color) must have a contrast ratio of at least 3:1.

### Operable
- 2.1.1 Keyboard: The toggle button must be fully operable via the keyboard, including toggling between the on and off states.
- 2.4.7 Focus Visible: A visible focus indicator must be present on the switch when keyboard focus is applied.
- 2.5.3 Label in Name: The accessible name of the toggle button should match or include its visible label text to ensure compatibility with assistive technology, such as voice controls.

### Robust
- 4.1.2 Name, Role, Value: The switch must have a programmatically determinable name, role, and current value (on or off).

## ACT Test Cases

### Test Case 1: Keyboard Accessibility and Focus Indication

#### Objective

To verify that the toggle button is fully accessible and operable via the keyboard, with a clearly visible focus indication.

#### Test Steps
1. Load the page and navigate to the toggle button using the tab key.
2. Observe the focus indicator on the toggle button when it receives focus.
3. Press the space key to toggle the switch between the on and off states.

#### Expected Results
- The toggle button receives keyboard focus with a visible focus indicator.
- Pressing the space key toggles the switch state, and this change is visually indicated.

### Test Case 2: Screen Reader Announcements / Programmatic Determinability of Name, Role, and Value

#### Objective

To confirm that the toggle button's label and state are correctly announced by screen readers.

To verify that the toggle button's name, role, and value (on or off) are programmatically determinable.

#### Test Steps
1. Enable a screen reader and navigate to the toggle button.
2. Listen for the screen reader's announcement of the switch's label and its current state (e.g., "on" or "off").
3. Inspect the toggle button element's HTML attributes or use accessibility inspection tools to confirm the name, role, and value.
4. Ensure that the accessible name matches the visible label text.

#### Expected Results
- The screen reader announces the switch's label and current state accurately (e.g., "Notifications, switch, on").
- The toggle button's name, role (e.g., button or checkbox), and value (on/off) are programmatically determinable and match the visible label.

### Test Case 3: Voice Control Accessibility

#### Objective

To verify that the toggle button can be operated using voice commands, supporting users of voice control software.

#### Test Steps
1. Using voice control software, instruct the software to "click" or "toggle" the switch by saying its visible label.
2. Verify that the command correctly changes the switch state to the intended on or off state.

#### Expected Results
- The switch toggles as expected upon issuing voice commands that reference the switch's label.

### Test Case 4: High Contrast Mode Compatibility

#### Objective

To ensure that the toggle button remains perceivable and usable in high contrast or forced color modes.

#### Test Steps
1. Enable high contrast or forced color mode in the operating system or browser.
2. Verify that the toggle button, focus indicator, and any visual state changes (on or off) are perceivable.

#### Expected Results
- The toggle button, including its on/off state and focus indicator, remains perceivable in high contrast mode.

### Test Case 5: Responsiveness and Accessibility of Text Scaling

#### Objective

To confirm that the toggle button remains accessible when text size is increased to 200%.

#### Test Steps
1. Increase the browser's text size to 200%.
2. Verify that the toggle button remains visually connected with its label, focus indicator, and state.

#### Expected Results
- The toggle button remains accessible, with no overlap or loss of functionality at 200% text size.
