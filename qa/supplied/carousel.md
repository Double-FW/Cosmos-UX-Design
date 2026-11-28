# Carousel Test Cri­te­ria

ACT for Carousel components.

On this page

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for the Carousel component. The Carousel pattern displays a series of slides, each containing content that users can navigate by using "next" and "previous" controls, swiping on touch screens, or auto-advancing when configured. This component must conform to W3C WCAG 2.2 standards to ensure accessibility for all users, including those who rely on assistive technologies.

## Accessibility Requirements

The Carousel component is evaluated against WCAG 2.2 success criteria, organized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.1.1 Non-text Content: Non-text elements (such as progress indicators) must provide accessible text alternatives.
- 1.3.1 Info and Relationships: The structure and relationships within the carousel must be programmatically defined.
- 1.4.1 Use of Color: The carousel must avoid using color as the only way to convey information.
- 1.4.3 Contrast (Minimum): Text and essential visual elements should meet a minimum contrast ratio of 4.5:1.
- 1.4.11 Non-text Contrast: Interactive controls (e.g., navigation arrows) must have a contrast ratio of at least 3:1 against their background.

### Operable
- 2.1.1 Keyboard: The carousel must be fully operable via keyboard navigation.
- 2.1.2 No Keyboard Trap: Focus can move into and out of the carousel without trapping the user.
- 2.2.2 Pause, Stop, Hide: Users must be able to pause, stop, or hide any auto-advancing slides.
- 2.4.3 Focus Order: The focus order must be logical and consistent with user expectations.
- 2.4.7 Focus Visible: A visible focus indicator must be present on all focusable elements.
- 2.5.3 Label in Name: The accessible name of each carousel control should match or include its visible label text.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have programmatically determinable names, roles, and states.

## ACT Test Cases

### Test Case 1: Keyboard Navigation and Focus Management

#### Objective

To ensure the carousel is fully navigable and operable using the keyboard alone.

#### Test Steps
1. Load the page and navigate to the carousel using the tab key.
2. Verify the visibility of the focus indicator on each carousel control.
3. Use the tab key to move sequentially through each carousel control and verify that focus only reaches currently visible slides.
4. Test the ability to use the enter or space key to activate the carousel controls (e.g., "Next" and "Previous").

#### Expected Results
- The focus indicator is clearly visible on each control.
- Only visible slides are focusable, preventing focus from moving to hidden slides.
- Carousel controls are operable using the keyboard.

### Test Case 2: Screen Reader Announcements for Carousel and Slide Content

#### Objective

To confirm that screen readers correctly announce carousel and slide structure, titles, and states.

#### Test Steps
1. Enable a screen reader and navigate to the carousel.
2. Listen for announcements regarding the carousel's title and its role as a "carousel."
3. Navigate to each visible slide and confirm that the screen reader announces the slide's title and its position in the sequence (e.g., "Slide 1 of 5").

#### Expected Results
- The screen reader correctly announces the carousel title and role.
- Each slide's title and position within the sequence are clearly announced.

### Test Case 3: Accessible Operation of Carousel Controls Using Voice Commands

#### Objective

To verify that users can operate carousel controls using voice control software.

#### Test Steps
1. Use voice control software to issue commands such as "click Next slide" or "click Previous slide" using the visible labels of carousel controls.
2. Verify that each command advances or reverses the carousel slide as expected.

#### Expected Results
- Each carousel control responds to voice commands, toggling slides as instructed.

### Test Case 4: Compliance with Reduced Motion Settings

#### Objective

To confirm that the carousel respects user settings for reduced motion.

#### Test Steps
1. Enable the "Reduce Motion" setting in the device or browser.
2. Observe the behaviour of carousel transitions when switching slides.

#### Expected Results
- Transitions are either eliminated or significantly reduced when "Reduce Motion" is enabled, ensuring the carousel respects user preferences.

### Test Case 5: Auto-Advance Pause Functionality

#### Objective

To ensure that users can pause or stop an auto-advancing carousel.

#### Test Steps
1. Configure the carousel to auto-advance.
2. Identify and use any provided "pause" or "stop" control.
3. Confirm that the carousel stops advancing automatically when paused.

#### Expected Results
- Users can stop or pause the carousel, and the auto-advance function remains disabled until re-enabled by the user.

### Test Case 6: Visual and Text-Based Progress Indicators

#### Objective

To confirm that visual and text-based indicators provide users with clear information about slide progression.

#### Test Steps
1. Examine the progress indicator and ensure it uses both visual elements (e.g., dots) and text alternatives to convey the current slide position.
2. Use screen reader or other assistive technology to verify that the text-based indicator (e.g., "Slide 1 of 5") is announced accurately.

#### Expected Results
- The progress indicator is both visually clear and accessible to screen readers, accurately reflecting the current slide position.

### Test Case 7: Responsive Design and Content Scaling

#### Objective

To confirm that carousel content remains accessible and functional when text size is increased.

#### Test Steps
1. Increase text size to 200% in the browser settings.
2. Verify that all carousel content remains visible and functional, with no clipping or overlap.

#### Expected Results
- Carousel content is accessible and readable at increased text sizes, with no loss of functionality.

### Test Case 8: Accessible Content Fallback with JavaScript Disabled

#### Objective

To ensure all carousel content remains accessible when JavaScript is disabled.

#### Test Steps
1. Disable JavaScript in the browser settings.
2. Reload the page and verify that all carousel slides are visible and accessible.

#### Expected Results
- All carousel slides or a comparable alternative is displayed when JavaScript is disabled.
