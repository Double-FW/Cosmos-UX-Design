# Skip Link Test Cri­te­ria

ACT for Skip Link components.

On this page
1. Introduction
2. Accessibility Requirements
3. ACT Test Cases

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for the implementation of a skip link within a web page. A skip link is an essential feature that enhances the navigability of a website for users who rely on keyboard navigation, by allowing them to bypass repetitive content and access the main content directly. This ACT document is structured to provide a detailed examination of the skip link component against W3C Web Content Accessibility Guidelines (WCAG) 2.2, focusing on ensuring that the component meets basic accessibility standards and enhances the user experience for individuals with disabilities.

## Accessibility Requirements

The Skip Link component is evaluated against the following WCAG 2.2 success criteria, categorized under the Perceivable, Operable, Understandable, and Robust (POUR) principles:

### Perceivable
- 2.4.1 Bypass Blocks: The component must provide a mechanism to allow users to navigate to content in a manner that bypasses blocks of content that are repeated on multiple Web pages.

### Operable
- 2.1.1 Keyboard: The skip link must be operable through keyboard interfaces, ensuring that users can navigate to and activate the skip link using only the keyboard.
- 2.4.7 Focus Visible: The skip link must be easily identifiable when it receives focus, to ensure that keyboard users can clearly perceive which element has keyboard focus.

### Robust
- 4.1.2 Name, Role, Value: The skip link must be properly coded so that assistive technologies can identify the skip link's name, role, and value, allowing users to understand and interact with the component as intended.

## ACT Test Cases

### Test Case 1: Visibility in Default State

#### Objective

To verify that the skip link is not visible when the page loads but becomes visible upon receiving keyboard focus.

#### Test Steps
1. Load the web page.
2. Observe the initial visibility of the skip link without interacting with the keyboard.
3. Use the tab key to navigate through the page's interactive elements.
4. Note when and how the skip link becomes visible.

#### Expected Results

The skip link should not be visible upon page load but should become visible when it receives keyboard focus.

### Test Case 2: Keyboard Focusability and Order

#### Objective

To ensure that the skip link can receive keyboard focus and is the first focusable element encountered through keyboard navigation, following any required legal consent requests.

#### Test Steps
1. Load the web page.
2. Use the tab key to start keyboard navigation from the beginning of the page.
3. Observe the order of focusable elements, noting the position of the skip link in this sequence.

#### Expected Results

The skip link should be the first focusable element encountered using the tab key, immediately following any legal consent requests if present.

### Test Case 3: Activation with Keyboard / Effective Bypass of Repetitive Content

#### Objective

To test the skip link's functionality using only the keyboard for activation.

To ensure that activating the skip link successfully bypasses repetitive or non-essential content and leads directly to the main content.

#### Test Steps
1. Navigate to the skip link using the tab key.
2. Activate the skip link using the enter key or the space key.
3. Observe the new focus point in the content and whether repetitive content (like navigation menus or headers) has been bypassed.
4. Verify that the focus is on the most relevant main content of the page.

#### Expected Results

Activating the skip link with the keyboard should bypass repetitive content and move the focus directly to the main content area.

Activating the skip link should effectively bypass repetitive or non-essential content, leading the user directly to the main content area without navigating through other focusable elements.

### Test Case 4: Contrast and Visibility Upon Focus

#### Objective

To confirm that the skip link is easily visible with high contrast against its background when it is focused.

#### Test Steps
1. Use the tab key to bring the skip link into focus.
2. Evaluate the visibility and contrast of the skip link against its background.
3. Optionally, use color contrast tools to measure the contrast ratio.

#### Expected Results

The skip link should exhibit high contrast and be easily visible when focused, meeting the minimum contrast ratio guidelines for text against its background.

### Test Case 5: Screen Reader Announcement

#### Objective

To verify that the skip link is properly announced by screen readers, including its purpose and destination.

#### Test Steps
1. Using a screen reader, navigate to the skip link.
2. Listen to the announcement of the skip link and note how its purpose and destination are described.
3. Confirm that the information provided is clear and accurate.

#### Expected Results

The skip link should be correctly announced by the screen reader, with its purpose and destination clearly communicated to the user.

These revised test cases are designed to thoroughly assess the accessibility and functionality of the skip link component, ensuring it meets the needs of users who rely on keyboard navigation and assistive technologies.
