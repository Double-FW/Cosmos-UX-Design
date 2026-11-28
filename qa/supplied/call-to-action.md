# Call to Ac­tion Test Cri­te­ria

ACT for Call to Action components.

On this page

This document outlines the accessibility conformance testing (ACT) for the Call to Action (CTA) web component, aiming to ensure that it meets W3C accessibility standards. The CTA is designed to lead users to consume promoted content, functioning between a link and a simplified card, with specific elements and behaviours that make it stand apart from main content.

## Introduction

The Call to Action (CTA) component is a critical element in web design, used to guide users towards a desired action. This document provides a detailed overview of the accessibility considerations and requirements for designing and implementing a CTA component that is both functional and accessible to all users, including those using assistive technologies. Component Overview

The CTA is conceptualized as a component that, although primarily a link, is designed with additional elements to make it more prominent and engaging. It is distinct from the main content and may include a heading, a related description, and possibly an image. The design and functionality of the CTA aim to make it easily navigable and operable for users, including those who rely on assistive technologies.

## Accessibility Requirements

The Call to Action (CTA) component is evaluated against the following WCAG 2.2 success criteria, categorized under the Perceivable, Operable, Understandable, and Robust (POUR) principles:

### Perceivable
- 1.1.1 Non-text Content: If the CTA includes non-text content (e.g., icons or images), alternative text that serves the equivalent purpose must be provided.
- 1.3.1 Info and Relationships: The structure and presentation of the CTA must be programmatically determined to enable assistive technologies to convey the information to users.
- 1.4.1 Use of Color: Color must not be the sole means of conveying information or distinguishing the CTA.
- \*1.4.3 Contrast (Minimum)\*\*: Text (and text in images) must have a contrast ratio of at least 4.5:1 against the background.
- 1.4.11 Non-text Contrast: Visual information required to identify user interface components (e.g., CTA borders, background) must have a contrast ratio of at least 3:1 against adjacent colors.

### Operable
- 2.1.1 Keyboard: The CTA must be fully operable through a keyboard interface without requiring specific timings for individual keystrokes.
- 2.1.2 No Keyboard Trap: Keyboard users must be able to navigate to and from the CTA without getting "trapped" in a loop.
- 2.4.3 Focus Order: When navigating through the page, the focus order must be logical and intuitive, ensuring that navigating to the CTA aligns with the user's expectations based on the page layout.
- 2.4.7 Focus Visible: The CTA must have a visible focus indicator to provide feedback to keyboard users.
- 2.5.8 Target Size (Minimum): The clickable area of the CTA must be at least 24 by 24 CSS pixels to be easily clickable or tapped.

### Robust
- 4.1.2 Name, Role, Value: The CTA must have a name and role that can be programmatically determined, and any user-settable values must be programmatically settable. Changes to any of these properties must be available to user agents, including assistive technologies.

## ACT Test Cases

### Test Case 1: Screen Reader Navigation / Accessible Label Presentation

#### Objective

To verify that the CTA is correctly recognized by screen readers and included in the Navigate by Links list.

To confirm that the CTA's accessible label or text alternative is properly presented to assistive technologies.

#### Test Steps
1. Access the component using a screen reader.
2. Use the screen reader's function to list all links on the page.
3. Verify that the CTA is included in this list.
4. Listen to the description provided by the screen reader.

#### Expected Results

The CTA is recognized and listed by the screen reader as a link.

The screen reader presents the CTA's label or text alternative, clearly describing its purpose or destination.

### Test Case 2: Keyboard Navigability and Focus Indication / Activation via Mouse, Touch, and Keyboard / Entire Area Clickable

#### Objective

To ensure the CTA is navigable using the keyboard alone and provides clear focus indication.

To verify that the CTA can be activated using various input methods: mouse, touch, and keyboard.

To ensure that clicking anywhere inside the CTA activates the link.

#### Test Steps
1. Navigate to the CTA using the tab key.
2. Observe if a visible focus indicator is present on the CTA.
3. Activate the CTA using the enter key.
4. Attempt to activate the CTA using a mouse click.
5. Attempt to activate the CTA using a touch (on a touch-enabled device).
6. Click on different areas within the CTA boundary.
7. Verify that all clicks activate the link.

#### Expected Results

The CTA is focusable by keyboard, shows a visible focus indicator, and can be activated using the enter key.

The CTA is activatable using all tested input methods.

All areas within the CTA boundary are clickable and activate the link.

### Test Case 6: Compliance with WCAG Target Size Criteria

#### Objective

To confirm the CTA's clickable area meets WCAG (Level AAA) criteria for Target Size.

#### Test Steps
1. Measure the size of the clickable area of the CTA using browser tools.
2. Compare the measurement to the minimum requirement of 44 by 44 CSS pixels.

#### Expected Results

The clickable area of the CTA is at least 44 by 44 CSS pixels, complying with WCAG Level AAA criteria.

### Test Case 7: Link-Specific Context Menu Invocation

#### Objective

To verify that the browser's link-specific context menu can be invoked on the CTA.

#### Test Steps
1. Focus on the CTA.
2. Attempt to invoke the context menu using a control click, right mouse button, or shift+F10.

#### Expected Results

The browser's link-specific context menu is successfully invoked using the described methods.
