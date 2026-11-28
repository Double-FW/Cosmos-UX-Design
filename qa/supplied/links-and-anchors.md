# Links and An­chors Test Cri­te­ria

ACT for Links and Anchors.

On this page
1. Introduction
2. Accessibility Requirements
3. ACT Test Cases

## Introduction

This document outlines best practices and development guidelines for creating accessible links and anchors on web pages. Accessible web links are vital for usability, aiding both people who use assistive technology and search engine optimization. The HTML &lt;a&gt; element, a fundamental web component, enables navigation to new content, file downloads, and communication actions like making a call or sending an email. Correctly implementing links with accessibility in mind enhances the user experience for everyone.

## Accessibility Requirements

The Links and Anchors component is evaluated against the following WCAG 2.2 success criteria, categorized under the Perceivable, Operable, Understandable, and Robust (POUR) principles:

### Perceivable
- 1.1.1 Non-text Content: Links containing icons or images must have appropriate alternative text that conveys the same purpose or presents the same information as the corresponding visual representation.
- 1.4.1 Use of Color: Links must be distinguishable from surrounding text without relying solely on colour, ensuring that users with visual impairments or colour blindness can identify links.
- 1.4.3 Contrast (Minimum): The text (and text in images) must have a contrast ratio of at least 4.5:1 against the background.
- 1.4.11 Non-text Contrast: The visual presentation of UI components (including borders, edges, and actionable items) must have a contrast ratio of at least 3:1 against adjacent colors.

### Operable
- 2.1.1 Keyboard: All links must be operable through keyboard interface without requiring specific timings for individual keystrokes.
- 2.4.7 Focus Visible: It must be visually clear when a link has keyboard focus to aid keyboard navigation, ensuring that the focus indicator is discernible.
- 2.5.8 Target Size: The exclusive target area for links that are not in a sentence or block of text must be at least 24 by 24 CSS pixels, including the space surrounding the link.

### Understandable
- 2.4.4 Link Purpose (In Context): The purpose of each link must be clear when the link text is read in context. For links where the context is not sufficient, additional measures (e.g., aria-labels) must be provided to clarify the link purpose.

### Robust
- 4.1.2 Name, Role, Value: For links that use icons or other non-text content, ensure that their name, role, and value are clearly defined for assistive technologies, allowing the link purpose to be programmatically determined.

## ACT Test Cases

### Test Case 1: Link Content Visibility and Icon Use / Contextual Clarity in Lists

#### Objective

Verify that every link on the page either contains visible text that accurately describes its destination or action, or uses a well-understood, conventional icon with appropriate alternative text.

Confirm that links presented in a list format have descriptive text or alternative text that provides clear context for the destination or action of the link, making them understandable when read out of context.

#### Test Steps
1. Identify all links on the page.
2. For each link, check if it contains descriptive text or a conventional icon.
3. If an icon is used, verify that alternative text is provided.
4. Identify lists of links on the page.
5. Evaluate the descriptiveness and clarity of the text for each link in the list.

#### Expected Results

All links should either have descriptive text or a conventional icon with equivalent alternative text.

Links within lists have clear, descriptive text or alternative text that accurately describes the destination or action, avoiding vague phrases like "click here."

### Test Case 2: Distinctiveness of Links

#### Objective

Ensure that links are visually distinct from non-link text without relying solely on colour, to accommodate users with low colour perception.

#### Test Steps
1. Examine the visual presentation of links within content.
2. Verify that links employ additional cues beyond colour, such as underlining or font weight, to signify their clickability.

#### Expected Results

Links are distinguishable from surrounding text through means other than colour alone, such as underlining or a different font style.

### Test Case 3: Keyboard Focus Visibility

#### Objective

Ensure that links are clearly identifiable when they receive keyboard focus, aiding keyboard-only users in navigating through page content.

#### Test Steps
1. Tab through the page to give focus to each link in turn.
2. Observe whether a visible focus indicator appears on each link.

#### Expected Results

A clear and visible focus indicator is present on links when they are navigated to using a keyboard.

### Test Case 5: Decorative Icon Exclusion by Screen Readers

#### Objective

Ensure that when a link includes both text and a decorative icon, screen readers correctly ignore the decorative icon.

#### Test Steps
1. Identify links that contain both text and decorative icons.
2. Use a screen reader to navigate to these links and listen to the announced text.

#### Expected Results

The screen reader announces only the text part of the link, ignoring the decorative icon.

### Test Case 6: Redundant Link Elimination

#### Objective

Ensure that multiple adjacent links to the same destination are combined into a single link to avoid redundancy and confusion.

#### Test Steps
1. Locate groups of adjacent links on the page.
2. Check if any such groups link to the same destination.
3. Verify that these have been combined into a single link.

#### Expected Results

No groups of adjacent links lead to the same destination; instead, they are consolidated into a single link.

### Test Case 7: Consistency of Hidden or Alternative Text

#### Objective

Verify that any hidden or alternative text provided for screen reader users does not contradict or significantly differ from the visible link text or the apparent meaning of icons.

#### Test Steps
1. Examine links with hidden or alternative text intended for screen readers.
2. Compare the hidden or alternative text with the visible text or icon's meaning.

#### Expected Results

The hidden or alternative text accurately reflects the visible link text or the icon's intended meaning, without contradiction or significant difference.

### Test Case 8: Sufficient Target Size

#### Objective

Ensure that the size of each link that is not part of a sentence or block of text is sufficiently large so that it can be clicked or tapped accurately.

#### Test Steps
1. Examine links that are not part of a sentence or block of text; and whose placement is the result of authored styles or scripts; and that do not require a specific positioning in order to be meaningful or legally compliant; and for which there is no alternative available elsewhere on the web page that already passes this test.
2. Measure a circle that extends no less than 24 CSS pixels from the center of the bounding box of the link.

#### Expected Results

The measured circle does not overlap with any similarly drawn circle extending from any nearby links or interactive elements.
