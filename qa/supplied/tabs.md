# Tabs

## Rule Description

Tabs consist of a number of sections (referred to as panels), only one of which is visible at a time, with a group of controls (referred to as tabs) preceding the panels that control which one is visible.

This rule tests that when tabs are included in an HTML page, they meet the requirement of the Tab component.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Tabs
    - Required for conformance
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects
- DOM Tree
- CSS Styling

## Applicability

This rule applies to any HTML page that includes a Tabs component.

## Expectations

### Expectation 1

Each tab component consists of two or more panels of content, only one of which is visible at a time.

### Expectation 3

The tab controls have a clear visual style that marks them out as interactive and related to the panel that they control when the panel is in the visible state.

### Expectation 4

Each tab control has a visual indication of the current state of the panel, i.e. visible or hidden.

### Expectation 5

Each tab control has content, preferably text, that describes the content of the panel that it controls.

### Expectation 10

When the tab control is activated keyboard focus must be moved to the associated panel. Note that the panel should be programmatically focusable (by applying tabindex="-1") but should not be keyboard focusable.

### Expectation 12

When a tab control has an aria-selected attribute value of false, its associated panel must be invisible, inaccessible to screen readers and other assistive technologies, and inaccessible to keyboard users.

### Expectation 15

Expanded panel content must immediately follow the set of tab controls in the document source order.

## Tabbed Con­tent Test Cri­te­ria

ACT for accessible tabbed content.

On this page
1. Introduction
2. Accessibility Requirements
3. ACT Test Cases

### Introduction

This document provides Accessibility Conformance Testing (ACT) guidelines for evaluating a tabbed content component's accessibility. Tabbed content allows users to navigate between different sections of related content using a familiar tabbed interface. Accessible tabbed content must comply with WCAG 2.2 standards and work with assistive technologies, including screen readers and keyboard navigation.

### Accessibility Requirements

The Tabbed Content component is evaluated against WCAG 2.2 success criteria, categorized by the POUR (Perceivable, Operable, Understandable, and Robust) principles.

#### Perceivable
- 1.3.1 Info and Relationships: The structure and relationships between the tablist, individual tabs, and tabpanels must be programmatically determined to ensure accessibility.
- 1.4.3 Contrast (Minimum): Text and interactive elements must have a minimum contrast ratio of 4.5:1 against their backgrounds.
- 1.4.11 Non-text Contrast: Visual indicators for selected and unselected tabs must have a contrast ratio of at least 3:1 to ensure accessibility for users with visual impairments.

#### Operable
- 2.1.1 Keyboard: All functionality, including navigating and selecting tabs, must be operable via the keyboard.
- 2.4.3 Focus Order: The focus order must be logical and intuitive, allowing keyboard users to navigate tabs and tabpanels sequentially.
- 2.4.7 Focus Visible: A visible focus indicator must be present on the currently focused tab or tabpanel.
- 2.5.3 Label in Name: The accessible name of each tab must match or include the visible label text, allowing voice control users to interact with tabs by name.

#### Robust
- 4.1.2 Name, Role, Value: The tabbed content structure should have a programmatically determinable name, role, and value (e.g., selected tab state). The user must perceive the current state of each tab and which tab is currently selected.

### ACT Test Cases

#### Test Case 1: Keyboard Accessibility and Focus Indication

##### Objective

To confirm that the tabbed content interface is fully accessible and operable via the keyboard, with clear focus indicators.

##### Test Steps
1. Load the page and navigate to the tablist using the tab key.
2. Use the arrow keys to navigate between tabs and press the enter or space key to select a tab.
3. Observe the focus indicator on the selected tab and the visibility of the corresponding tabpanel.

##### Expected Results
- The tabbed interface can be navigated using only the keyboard.
- Each tab receives focus and a clear focus indicator.
- The selected tabpanel becomes visible when a tab is activated.

Expectation 2

Each tab component has a set of tabs — each one relating to a panel in a one-to-one mapping — which is a keyboard, touch, and pointing device accessible control that is used to select the currently visible panel.

#### Test Case 2: Screen Reader Announcements / Programmatic Determinability of Name, Role, and Value

##### Objective

To verify that the tabbed content structure, including the tablist, tabs, and tabpanels, is accessible to screen readers.

To verify that the tabbed content's name, role, and value (e.g., selected tab state) are programmatically determinable.

##### Test Steps
1. Enable a screen reader and navigate to the tablist.
2. Listen for announcements regarding the role (e.g., "tab") and accessible names of each tab.
3. Select a tab and listen for the screen reader's announcement of the tabpanel content.
4. Inspect each tab and tabpanel element's HTML attributes or use accessibility inspection tools to verify the name, role, and state.
5. Ensure that each tab's accessible name matches the visible label text.

##### Expected Results
- The screen reader announces each tab's role, accessible name, and selection state (e.g., "tab 1 of 3, selected").
- The corresponding tabpanel content is announced when a tab is selected.
- Each tab and tabpanel's name, role, and value are programmatically determinable and reflect the selected or unselected state.

Expectation 6

All tab controls are grouped together and contained with an element that has a role attribute with value of tablist.

Expectation 7

Each tab control has a role attribute with a value of tab.

Expectation 8

Each tab control has an aria-selected attribute; the value of this attribute is true when the panel content is visible and false when the panel content is hidden.

Expectation 9

When the tab control is activated the value of its aria-selected attribute is toggled between true and false.

Expectation 11

When a tab control has an aria-selected attribute value of true, its associated panel must be visible, accessible to screen readers and other assistive technologies, and accessible to keyboard users.

Expectation 13

Each panel has a role attribute with a value of tabpanel.

Expectation 14

Each panel has an aria-labelledby attribute with a value that matches the id attribute of the associated tab control.

#### Test Case 3: Voice Control Accessibility

##### Objective

To ensure that users can select a tab using voice commands by speaking the tab's visible label.

##### Test Steps
1. Using voice control software, issue the "click" command followed by the visible label of a tab.
2. Verify that the command selects the intended tab and reveals the corresponding tabpanel content.

##### Expected Results
- The tab is selected, and the correct tabpanel content is displayed when using voice commands that reference the tab's label.

#### Test Case 4: High Contrast Mode Compatibility

##### Objective

To verify that the tabbed content remains accessible and distinguishable in high contrast or forced color mode.

##### Test Steps
1. Enable high contrast or forced color mode in the operating system or browser.
2. Verify that the focus indicator, selected tab, and unselected tabs are clearly perceivable and that the selected tabpanel is distinguishable.

##### Expected Results
- The tabbed interface, including selected and unselected tabs, focus indicators, and selected tabpanels, is accessible and clearly distinguishable in high contrast mode.

#### Test Case 5: Responsiveness and Accessibility of Text Scaling

##### Objective

To confirm that the tabbed interface remains accessible when text size is increased to 200%.

##### Test Steps
1. Increase the browser's text size to 200%.
2. Verify that each tab remains visible and readable and that the selected tabpanel remains visually connected to its tab.

##### Expected Results
- The tabbed interface adapts to increased text size without loss of functionality or readability.

#### Test Case 7: Fallback Display When JavaScript is Unavailable

##### Objective

To confirm that the tabbed content is accessible without JavaScript.

##### Test Steps
1. Disable JavaScript and reload the page.
2. Verify that all tabpanel content is visible and accessible to screen readers.
3. Check that each tab functions as a table of contents, or verify that it has been visually hidden if necessary.

##### Expected Results
- All tabpanel content is accessible and readable without JavaScript.
- Tabs do not simulate tab behavior but instead serve as navigational links or a table of contents for the tabpanel content.
