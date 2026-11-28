# Accordions

## Rule Description

The accordion consists of a number of collapsed sections, each with a button to expand that section's content.

This rule tests that when an accordion is included in an HTML page it meets the requirement of the Accordion component.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Accordion
    - Required for conformance to
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects

DOM Tree CSS Styling

## Applicability

This rule applies to any HTML page that includes a Accordion component.

## Expectation 1

Each accordion component consists of two or more accordion items; an accordion item is a combination of content that can be expanded and collapsed and the control that allows the content to be expanded and collapsed.

## Expectation 3

Accordion item controls are visible in both the expanded and collapsed states of the accordion item.

## Expectation 4

Accordion item controls have a clear visual style that marks them out as interactive and related to the content that they control when the content is in the expanded state.

## Expectation 5

Each accordion item control has a visual indication of the current expanded / collapsed state of the accordion item.

## Expectation 6

Each accordion item control has content, preferably text, that describes the content that it controls.

## Expectation 12

Expanded accordion content must immediately follow the accordion item control in the document source order.

## Ac­cor­dion Test Cri­te­ria

ACT for Accordion components.

On this page

### Introduction

This document outlines the Accessibility Conformance Testing (ACT) for the Accordion web component. The Accordion pattern organizes content into expandable sections, helping users manage information display. This component must comply with W3C WCAG 2.2 standards to ensure accessibility for all users, including those relying on assistive technologies.

### Accessibility Requirements

The Accordion component is evaluated against WCAG 2.2 success criteria, categorized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

#### Perceivable
- 1.1.1 Non-text Content: Non-text elements (such as icons) must include accessible text alternatives.
- 1.3.1 Info and Relationships: Structure and relationships must be programmatically defined to ensure accessibility.
- 1.4.1 Use of Color: The component must avoid using color as the only way to convey information.
- 1.4.3 Contrast (Minimum): Text and icons must meet the minimum contrast ratio of 4.5:1.
- 1.4.11 Non-text Contrast: Visual indicators (e.g., arrows or chevrons) must have a contrast ratio of at least 3:1 against adjacent colors.

#### Operable
- 2.1.1 Keyboard: The Accordion must be navigable and operable by keyboard alone.
- 2.1.2 No Keyboard Trap: Focus can be moved into and out of the Accordion using the keyboard without trapping the user.
- 2.4.3 Focus Order: Focus order must be logical and predictable.
- 2.4.7 Focus Visible: A visible focus indicator must be present on each Accordion control.
- 2.5.3 Label in Name: The accessible name of each Accordion control should match or include its visible label text.

#### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state (expanded/collapsed).

### ACT Test Cases

#### Test Case 1: Initial Focus and Keyboard Navigation / Visual and Functional Accessibility of Focus Indication

##### Objective

To verify that the Accordion is fully operable and navigable via the keyboard.

To confirm the visibility and usability of focus indicators for keyboard and screen reader users.

##### Test Steps
1. Load the page and navigate to the Accordion using the tab key.
2. Use the tab key to move focus to the next Accordion control.
3. Observe the focus indicator on each control and assess the contrast against its background.
4. Measure the contrast ratio using a color contrast tool.
5. Test the ability to toggle each Accordion section by pressing enter or space.

##### Expected Results
- Each Accordion control receives keyboard focus in logical sequence.
- Toggling each section is possible using the keyboard, with a clear visual indicator of focus.
- Each Accordion control displays a clearly visible focus indicator, distinguishable from the background.
- The contrast ratio meets the minimum of 4.5:1.

Expectation 2

Each accordion item has a keyboard, touch, and pointing device accessible control that is used to expand and collapse the accordion item.

Expectation 9

When the accordion item control is activated keyboard focus must remain on the control.

#### Test Case 2: Screen Reader Announcements

##### Objective

To confirm that Accordion controls and states are accurately announced by screen readers.

##### Test Steps
1. Enable a screen reader and navigate to the Accordion controls.
2. Focus on each Accordion control and listen to the announcement of its label and expanded/collapsed state.

##### Expected Results
- The screen reader announces each Accordion control with its label and current state (expanded or collapsed).
- Users hear an appropriate description of the Accordion's role, such as "toggle."

Expectation 7

Each accordion item control has an aria-expanded attribute; the value of this attribute is true when the accordion item content is expanded and false when the accordion item content is collapsed.

Expectation 8

When the accordion item control is activated the value of its aria-expanded attribute is toggled between true and false.

Expectation 10

When an accordion item control has an aria-expanded attribute value of true, its associated content must be visible, accessible to screen readers and other assistive technologies, and accessible to keyboard users.

Expectation 11

When an accordion item control has an aria-expanded attribute value of false, its associated content must be invisible, inaccessible to screen readers and other assistive technologies, and inaccessible to keyboard users.

#### Test Case 4: Accessible Toggle of Accordion Sections by Voice Control

##### Objective

To validate that the Accordion controls can be toggled by voice commands, supporting users of voice control software.

##### Test Steps
1. Using voice control software, instruct the software to "click" on each Accordion control using its visible label text.
2. Verify that each command results in the intended toggle action, expanding or collapsing the corresponding section.

##### Expected Results
- Each Accordion section expands or collapses as expected upon issuing voice commands that reference the control's visible label.

#### Test Case 5: Compliance with Reduced Motion Settings

##### Objective

To ensure that the Accordion respects user preferences for reduced motion.

##### Test Steps
1. Enable "Reduce Motion" in the device or browser settings.
2. Open and close Accordion sections, observing any animations.

##### Expected Results
- Animations are reduced or disabled, respecting the user's "Reduce Motion" setting.

#### Test Case 6: Responsiveness and Accessibility of Content Scaling

##### Objective

To confirm that Accordion content remains accessible when text size is increased.

##### Test Steps
1. Increase the browser's text size to 200%.
2. Ensure that all Accordion content is fully visible and functional within the viewport at this scaling level.

##### Expected Results
- All Accordion content remains accessible, with no clipping, overlapping, or loss of functionality at 200% text size.

#### Test Case 7: JavaScript Independence

##### Objective

To verify that the Accordion content remains accessible if JavaScript is disabled.

##### Test Steps
1. Disable JavaScript in the browser.
2. Reload the page and inspect the Accordion content.

##### Expected Results
- All Accordion content remains accessible, or a suitable non-interactive fallback is provided.
