# Modal Di­a­log Test Cri­te­ria

ACT for Modal Dialog components.

On this page

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for the modal dialog web component. A modal dialog is a UI pattern that presents content requiring the user's immediate attention, blocking interaction with the underlying page until the dialog is closed. This document provides a comprehensive analysis based on W3C WCAG 2.2 guidelines, ensuring that the modal dialog component is accessible and offers a seamless experience for all users, including those using assistive technologies.

## Accessibility Requirements

The Modal Dialog component is evaluated against the following WCAG 2.2 success criteria, categorized under the Perceivable, Operable, Understandable, and Robust (POUR) principles:

### Perceivable
- 1.1.1 Non-text Content: Ensure that all non-text content presented to the user has a text alternative that serves the equivalent purpose.
- 1.4.3 Contrast (Minimum): The text (and text in images) must have a contrast ratio of at least 4.5:1 against the background.
- 1.4.11 Non-text Contrast: The visual presentation of UI components (including borders, edges, and actionable items) must have a contrast ratio of at least 3:1 against adjacent colors.
- 1.4.4 Resize text: Text can be resized up to 200% without loss of content or functionality.

### Operable
- 2.1.1 Keyboard: Ensure that all functionality of the content is operable through a keyboard interface without requiring specific timings for individual keystrokes.
- 2.1.2 No Keyboard Trap: Users can navigate to and from all content on the web page using a keyboard.
- 2.4.3 Focus Order: Ensure that when an interface element receives focus, it does not initiate a change of context.

### Robust
- 4.1.2 Name, Role, Value: For all user interface components, the name and role can be programmatically determined; states, properties, and values that can be set by the user can be programmatically set; and notification of changes to these items is available to user agents, including assistive technologies.

Based on the detailed testing notes provided, here are the refined ACT Test Cases tailored to evaluate the accessibility conformance of the modal dialog component:

## ACT Test Case 1: Pre-Opening State and Accessibility

### Objective

To verify that the modal dialog is not accessible to users, including those using assistive technology, before it is intentionally opened.

### Test Steps
1. Navigate the web page without triggering the modal dialog.
2. Use screen reader and keyboard navigation to check for the presence of the modal dialog.

### Expected Results
- The modal dialog should not be detected or accessible by screen readers or through keyboard navigation before it is opened.

## ACT Test Case 2: Modal Dialog Opening and Initial Focus

### Objective

To ensure that when the modal dialog opens, it becomes the focus of interaction for all users, including those using assistive technology, and the underlying page becomes non-interactive.

### Test Steps
1. Open the modal dialog through a user action.
2. Confirm that the underlying web page is visually obscured and appears non-interactive.
3. Check that keyboard focus moves to an appropriate element within the modal dialog.
4. Use a screen reader to verify that the role of "dialog" is announced, along with the dialog's accessible label.

### Expected Results
- The underlying page must not be interactable and should be visually indicated as such.
- Keyboard focus must move to the modal dialog, with the first focusable element or a relevant element receiving focus initially.
- Screen readers correctly announce the opening of the modal dialog, including its role and label.

## ACT Test Case 3: Interaction Within Open Modal Dialog

### Objective

To evaluate the accessibility of content within an open modal dialog for users, including those using assistive technology.

### Test Steps
1. With the modal dialog open, attempt to interact with the underlying web page using both keyboard and mouse inputs.
2. Navigate the modal dialog's content using keyboard and screen reader.
3. Verify that the web page does not scroll while the modal dialog is open.
4. Check that all content within the modal dialog is accessible and that interactive elements can be operated.

### Expected Results
- Interaction with the underlying page is blocked while the modal dialog is open.
- The modal dialog's content is fully accessible and navigable using assistive technologies.
- Scrolling on the underlying page is disabled.
- The modal dialog includes at least one operable focusable element to close the dialog.

## ACT Test Case 4: Responsiveness and Zoom Compatibility

### Objective

To ensure the modal dialog's content remains accessible and usable under various display conditions, including zoomed-in views and narrow screen widths.

### Test Steps
1. Zoom the browser window to 200% and adjust the width to simulate a narrow screen (down to 360px).
2. Navigate and interact with the modal dialog, checking for content accessibility and usability.

### Expected Results
- All content within the modal dialog is accessible, readable, and operable, with no loss of information or functionality, at zoom levels up to 200% and on screens as narrow as 360px wide.

## ACT Test Case 5: Closing Modal Dialog and Post-Closure State

### Objective

To confirm that closing the modal dialog restores access to the underlying web page and returns focus to an appropriate location.

### Test Steps
1. Close the modal dialog using its close mechanism (e.g., "Close" button, "Escape" key, or backdrop click, if applicable).
2. After closure, attempt to interact with the underlying web page.
3. Verify that keyboard focus returns to a logical position related to where it was before the modal dialog opened.

### Expected Results
- Upon closing, the modal dialog becomes inaccessible, and the underlying page becomes fully interactive again.
- The web page is no longer visually obscured and is scrollable.
- Keyboard focus returns to an appropriate element on the web page, ideally where it was before the modal dialog opened.

## ACT Test Case 6: Multiple Modal Dialogs Handling

### Objective

To assess the accessibility handling when multiple modal dialogs are opened in sequence.

### Test Steps
1. Open a modal dialog and then trigger another modal dialog from within the first one.
2. Navigate through the content of each opened modal dialog sequentially.
3. Close each modal dialog in turn and observe the focus management between closures.

### Expected Results
- Keyboard focus transitions from one modal dialog to the next, and finally back to the underlying web page, maintaining logical order and accessibility standards.
