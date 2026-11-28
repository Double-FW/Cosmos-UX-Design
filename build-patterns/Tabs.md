---
title: "Build tabs"
description: "Standalone build brief, behaviour scenarios, retrieval pack and verification routes for tabs."
document_type: "pattern_build_specification"
language: "en-GB"
pattern: "tabs"
status: "pilot"
---

# Build tabs

## Standalone build prompt

Implement the selected web variant in the host’s framework, design system and brand. Use the existing host primitives where their rendered HTML and behaviour satisfy the outcomes below. Use semantic HTML, native controls and token-driven presentation. In the absence of a host, build a plain HTML fixture. Ask only for missing input that prevents useful work.

- Record the selected variant, user task, scope, the host stack, design tokens and supported browsers, devices, input modes and assistive technologies. Mark unresolved dependencies and assumptions.
- Consult the current WHATWG HTML Living Standard for the relevant elements and attributes. Record the section and date consulted in the handover. [links](https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-a-element) · [focus and tabindex](https://html.spec.whatwg.org/multipage/interaction.html#the-tabindex-attribute) · [ARIA tab behaviour](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)
- Before implementation, create or update a versioned [Component Semantics Contract](../Component%20Semantics%20Contract.md) for the selected variant. Record elements, names, roles, relationships, initial and reachable states, focus, announcements, fallback and check IDs. Preserve this record for future changes. Update the contract when implementation or verification changes what is supported.
- Deliver the implementation, semantics contract and verification results keyed by scenario ID. Apply every scenario whose preconditions match the selected variant. Use **pass**, **fail**, **not applicable**, **not run** or **cannot tell**, with evidence or reasons. Treat the [QA test matrix](../testing-matrix/README.md) as the source crosswalk. It is not a substitute for evidence from the rendered product.

Build a set of related panels in the same view. Default to standard horizontal manual activation for new sets. Use vertical orientation only with corresponding arrow commands, or automatic activation only with promptly available panels. Keep the existing focus-moving variant only when explicitly required. Baseline is a labelled set of working fragment links to visible sections with headings. After enhancement, use a named tablist with named tabs and one labelled tabpanel per tab, unique associated IDs and exactly one selected visible panel. Keep the tab list before the panels. Keep selected and focus styles distinct, labels readable and available in forced colours. For standard tabs, Tab enters on selection, orientation-specific arrows wrap and move focus, Enter or Space selects in manual mode, and Tab reaches the active panel or its first meaningful control. Restore the selected entry point on leaving without selection. Automatic activation selects on arrow focus. For the existing variant, Tab reaches each tab and activation focuses its named panel programmatically. Document whether selection writes history. Support deep links in either case. On a fresh load with an unrelated fragment, show the default panel and let the browser reach the unrelated destination. For a same-page move to that destination, preserve the current selection. Use Home and End if supported.

## Gherkin acceptance outcomes

The block is a portable feature specification. To run it in a BDD runner, bind the steps to host fixtures and assertions. For agent execution without a BDD runner, perform the actions and capture the same evidence against each scenario ID.

```gherkin
Feature: Accessible tabs
  Build the variant declared in the build brief.
  Evaluate conditional scenarios only where their Given conditions apply.

  @TAB-01
  Scenario: Tabs have one selected associated panel
    Given an enhanced tab set with two or more panels
    When the person selects each tab using supported inputs
    Then each named tab controls its own named panel, exactly one tab is selected and only its panel is available after the tab list

  @TAB-02
  Scenario: Manual activation separates focus from selection
    Given standard manual tabs with focus on the selected tab
    When the person uses an orientation-appropriate arrow then activates the focused tab
    Then arrow focus wraps without changing selection, activation selects while retaining tab focus, and Tab then Shift+Tab connects the active panel and tab list. If the panel begins with non-interactive content the panel itself is the first Tab stop, otherwise its first meaningful interactive control is

  @TAB-03
  Scenario: Automatic activation selects on arrow focus
    Given standard automatic tabs with promptly available content
    When the person moves to a tab using its orientation-specific arrow
    Then the newly focused tab becomes selected and the corresponding panel is available without further activation

  @TAB-04
  Scenario: The existing variant moves focus into its panel
    Given an explicitly selected focus-moving variant
    When the person Tabs to a tab and activates it
    Then focus moves to the newly visible named panel, the next Tab proceeds into or past the panel and Shift+Tab from the panel returns to the list

  @TAB-05
  Scenario: Baseline links reach visible content
    Given a tab set ready to load
    When scripts are blocked or enhancement fails before it is ready
    Then all headed sections and their fragment links remain usable without tab widget roles or hidden panels

  @TAB-06
  Scenario: Fragments and history preserve the destination
    Given a tab set with direct panel and nested fragments
    When the person loads those fragments and uses Back and Forward if selection writes the URL
    Then the target panel is exposed before reaching the target, an unrelated fragment keeps normal navigation without changing an existing selection, and supported history restores selection and keeps focus usable

  @TAB-07
  Scenario: Selection and focus remain distinguishable
    Given tabs in default and forced-colour display modes
    When the person enlarges text or narrows the viewport in separate runs and repeats selection and navigation
    Then labels remain readable, selection and focus stay distinct and visible orientation agrees with keyboard behaviour
```

## Retrieval pack

| Retrieve | Use it for |
| --- | --- |
| [Owning pattern guide](../Tabs.md) | Purpose, named variants, markup and interaction; apply only to the declared variant. |
| [Pinned original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Tabs.md#reference-implementation) | Source fixtures and code decisions reverse-engineered into the outcomes above. Preserve this revision when comparing or restoring examples. |
| [Component Semantics Contract](../Component%20Semantics%20Contract.md#contract-template) | Record a durable shared expectation before coding. |
| [links](https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-a-element) · [focus and tabindex](https://html.spec.whatwg.org/multipage/interaction.html#the-tabindex-attribute) · [ARIA tab behaviour](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) | Current web semantics and pattern interpretation. Check the exact section when implementing. |
| [Guide-specific QA matrix](../testing-matrix/Design%20Patterns.md#design-patterns-testing-matrix) and [canonical ID definitions](../testing-matrix/Canonical%20References.md) | Find the Tabs row, applicable canonical checks and test procedure links. |
| [ACT applicability review](../qa/Review.md#contradictions-and-applicability) and [matrix gaps](../testing-matrix/Review.md#coverage-gaps) | Resolve conflicts and keep WCAG, pattern expectations and usability evidence distinct. |
| [Guide checks](../Tabs.md#testing-approach-and-act-scripts) and [supplied ACT-style cases](../qa/supplied/tabs.md#act-test-cases) | Manual procedures and variant checks. |
| [ACT bc4a75](../qa/w3c-act/bc4a75.md) · [ACT 4e8ab6](../qa/w3c-act/4e8ab6.md) · [ACT 6cfa84](../qa/w3c-act/6cfa84.md) | Run only where the rule’s actual applicability matches. Preserve each rule’s status and record the exact assertion. |

Apply imported Expectation 10 and any all-tabs-in-Tab-sequence steps only to the focus-moving variant. Expectation 15 describes the active content following the set, not relocating every panel in the DOM. Preserve the original scripts and their numbering. A review of selected/hidden states is shared evidence across the overlapping checks.

## Verification and traceability

T. The table names candidate agent checks. Tool or fixture availability determines what can be executed. Save the variant, fixture/build, browser or tool version, command, expected and observed outcome. Use the linked manual procedures for effects beyond the available automation.

| Outcome | Source checks | Agent evidence and limits |
| --- | --- | --- |
| TAB-01 | T1–T3; supplied E1/E5/E12/E15 and cases 1/2/3; CTL-167/169; ACT bc4a75/4e8ab6/6cfa84 as applicable | Assert names, roles, relationships, state, DOM order, pointer and touch activation; confirm actual browse behaviour manually |
| TAB-02 | T5; supplied 1 variant subset; CTL-061/091/157 | Record active element, roving tabindex, wrap and selected panel |
| TAB-03 | T5; CTL-157/169 | Record focus and selection together; manually assess delay if relevant |
| TAB-04 | T6; supplied E10 and cases 1/2; CTL-061/091 | Exercise actual tab order and programmatic focus |
| TAB-05 | T7; supplied 7; CTL-168 | Load with scripts blocked and with simulated failed initialization |
| TAB-06 | T8–T9; Cosmos behaviour | Drive navigation across fragment, unrelated fragment and initial URLs |
| TAB-07 | T4/T10; supplied E3/E4 and cases 1/4/5; CTL-095 | Browser inputs and layout signals; human visual and voice checks |

[Manual checks in the owning guide](../Tabs.md#testing-approach-and-act-scripts) · [Supplied ACT-style manual procedures](../qa/supplied/tabs.md#act-test-cases)
