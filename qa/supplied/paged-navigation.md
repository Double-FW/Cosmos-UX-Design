# Paged Nav­i­ga­tion Test Cri­te­ria

ACT for accessible paged navigation interfaces.

On this page
1. Introduction
2. Accessibility Requirements
3. ACT Test Cases

## Introduction

This document outlines the Accessibility Conformance Testing (ACT) for a paged navigation component that allows users to move through sets of paged results. Accessible paged navigation must meet W3C WCAG 2.2 standards to ensure usability for all users, including those using assistive technology.

## Accessibility Requirements

The Paged Navigation component is evaluated against WCAG 2.2 success criteria, categorized by Perceivable, Operable, Understandable, and Robust (POUR) principles.

### Perceivable
- 1.3.1 Info and Relationships: Structure and relationships between navigation links, separators, and the current page indicator must be programmatically defined and clear to assistive technology.
- 1.4.1 Use of Color: The component must not rely solely on color to communicate the current page or skipped links.
- 1.4.3 Contrast (Minimum): Text and visual indicators (e.g., ellipses and current page marker) must meet a contrast ratio of at least 4.5:1.

### Operable
- 2.1.1 Keyboard: Each navigation link must be accessible and operable using only the keyboard.
- 2.4.4 Link Purpose (In Context): Each navigation link should clearly indicate which page it leads to, even when viewed independently of surrounding context.
- 2.4.7 Focus Visible: A visible focus indicator must be present on each interactive navigation element.

### Robust
- 4.1.2 Name, Role, Value: Each interactive element must have a programmatically determinable name, role, and state (e.g., "current page").

## ACT Test Cases

### Test Case 1: Keyboard Accessibility and Focus Management / Visual and Functional Accessibility of Focus Indicators

#### Objective

To verify that the paged navigation is fully navigable and operable using only the keyboard.

To confirm the visibility and usability of focus indicators on paged navigation links for keyboard users.

#### Test Steps
1. Load the page and navigate to the paged navigation component using the tab key.
2. Press the tab key to cycle through each page link.
3. Assess the focus indicator's visibility and contrast on each link.
4. Optionally, use a color contrast tool to measure the contrast ratio.
5. Confirm that the current page link does not take keyboard focus if it is disabled or inactive.

#### Expected Results
- Each navigation link receives keyboard focus in logical sequence, with a visible focus indicator.
- The current page link, if inactive, is skipped in the keyboard focus order.
- Focus indicators are clearly visible and distinguishable on each link, meeting a minimum contrast ratio of 4.5:1.

### Test Case 2: Screen Reader Announcements of Labels and States

#### Objective

To confirm that screen readers correctly announce each link in the navigation component, including the current page indicator and any elided content.

#### Test Steps
1. Enable a screen reader and navigate to the paged navigation.
2. Listen for the announcement of the navigation's label, e.g., "More results, navigation."
3. Navigate through each link and listen for its label, e.g., "Page 1, link."
4. Confirm that the current page is announced as "current page."
5. If any links are omitted (e.g., ellipsis for skipped pages), confirm that the separator is announced as such.

#### Expected Results
- The screen reader announces the navigation's label as well as each page link's label and role.
- The current page link is announced as "current page."
- The separator (e.g., ellipsis) is announced appropriately to indicate omitted links.

### Test Case 4: Voice Control Accessibility

#### Objective

To verify that the navigation links can be activated by voice commands, allowing voice control users to interact with paged navigation.

#### Test Steps
1. Using voice control software, instruct the software to "click" each page link by saying its visible label.
2. Confirm that the command correctly activates each link to navigate to the intended page.

#### Expected Results
- Each page link responds to voice commands, leading the user to the correct page as intended.

### Test Case 5: High Contrast Mode Compliance

#### Objective

To ensure that paged navigation remains usable in high contrast or forced color display modes.

#### Test Steps
1. Enable high contrast or forced color mode in the operating system or browser.
2. Verify that all page links, focus indicators, and any visual separators (e.g., ellipses) are clearly visible.

#### Expected Results
- The navigation component remains fully accessible in high contrast mode, with all interactive elements and indicators visible.

### Test Case 6: Responsiveness and Scaling for Larger Text

#### Objective

To confirm that paged navigation remains accessible when text size is increased to 200%.

#### Test Steps
1. Increase the browser's text size to 200%.
2. Verify that all paged navigation content is fully visible and functional, including focus indicators, links, and separators.

#### Expected Results
- All navigation content remains accessible and readable, with no overlap, clipping, or loss of functionality at 200% text size.

### Test Case 7: JavaScript Independence

#### Objective

To ensure that the paged navigation remains accessible if JavaScript is disabled.

#### Test Steps
1. Disable JavaScript in the browser.
2. Reload the page and inspect the navigation component's content and links.

#### Expected Results
- The navigation content remains accessible, or an appropriate non-interactive fallback is provided if JavaScript is unavailable.
