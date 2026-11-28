---
title: "Build disclosure"
description: "Standalone build brief, behaviour scenarios, retrieval pack and verification routes for disclosure."
document_type: "pattern_build_specification"
language: "en-GB"
pattern: "disclosure"
status: "pilot"
---

# Build disclosure

## Standalone build prompt

Implement the selected web variant in the host’s framework, design system and brand. Use the existing host primitives where their rendered HTML and behaviour satisfy the outcomes below. Use semantic HTML, native controls and token-driven presentation. In the absence of a host, build a plain HTML fixture. Ask only for missing input that prevents useful work.

- Record the selected variant, user task, scope, the host stack, design tokens and supported browsers, devices, input modes and assistive technologies. Mark unresolved dependencies and assumptions.
- Consult the current WHATWG HTML Living Standard for the relevant elements and attributes. Record the section and date consulted in the handover. [details and summary](https://html.spec.whatwg.org/multipage/interactive-elements.html#the-details-element) · [button](https://html.spec.whatwg.org/multipage/form-elements.html#the-button-element)
- Before implementation, create or update a versioned [Component Semantics Contract](../Component%20Semantics%20Contract.md) for the selected variant. Record elements, names, roles, relationships, initial and reachable states, focus, announcements, fallback and check IDs. Preserve this record for future changes. Update the contract when implementation or verification changes what is supported.
- Deliver the implementation, semantics contract and verification results keyed by scenario ID. Apply every scenario whose preconditions match the selected variant. Use **pass**, **fail**, **not applicable**, **not run** or **cannot tell**, with evidence or reasons. Treat the [QA test matrix](../testing-matrix/README.md) as the source crosswalk. It is not a substitute for evidence from the rendered product.

Build an in-place reveal for supplementary content. Default to a native disclosure using a details element and its first summary child. Choose an enhanced button only when the host has a documented need the native variant cannot meet. Default to collapsed when the control is operational. Keep the trigger visible and named for what it reveals. Use a distinct visible state cue and keep panel content next in reading order. For the enhanced variant use a non-submit button, aria-expanded and an ID reference to its panel. Add a named region only when it helps navigation. Use native Enter and Space activation. For blocked scripts, keep enhanced content visible and the inactive control hidden. If the host adds motion, honour reduced motion.

## Gherkin acceptance outcomes

The block is a portable feature specification. To run it in a BDD runner, bind the steps to host fixtures and assertions. For agent execution without a BDD runner, perform the actions and capture the same evidence against each scenario ID.

```gherkin
Feature: Accessible disclosure
  Build the variant declared in the build brief.
  Evaluate conditional scenarios only where their Given conditions apply.

  @DIS-01
  Scenario: Native disclosure has a meaningful control
    Given an initially closed native disclosure
    When the person reaches its summary
    Then the control has the label for its associated content and exposes the collapsed state

  @DIS-02
  Scenario: Enhanced state stays synchronised
    Given an enhanced disclosure with a link in the panel and keyboard focus on its trigger
    When the person opens it with Enter and closes it with Space
    Then focus stays on the trigger and the expanded state matches visual visibility and panel exposure after each action

  @DIS-03
  Scenario: The panel follows the trigger when open
    Given either variant with a focusable control in its panel
    When the person Tabs from the trigger in the open and closed states
    Then Tab reaches the panel control when open and skips it when closed

  @DIS-04
  Scenario: Activation and state remain perceivable
    Given either variant with its supported input methods and display settings
    When the person opens and closes it with keyboard, pointer, touch or voice as supported
    Then each method opens and closes the same panel and the label, focus and state remain distinguishable with forced colours and enlarged text

  @DIS-05
  Scenario: Essential content survives loss of enhancement
    Given a disclosure in a test page
    When scripts are blocked and CSS is disabled in separate runs
    Then native activation still works without scripts or enhanced content remains available directly, and disabling CSS does not prevent reaching or operating the content
```

## Retrieval pack

| Retrieve | Use it for |
| --- | --- |
| [Owning pattern guide](../Disclosure.md) | Purpose, named variants, markup and interaction; apply only to the declared variant. |
| [Pinned original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#reference-implementation) | Source fixtures and code decisions reverse-engineered into the outcomes above. Preserve this revision when comparing or restoring examples. |
| [Component Semantics Contract](../Component%20Semantics%20Contract.md#contract-template) | Record a durable shared expectation before coding. |
| [details and summary](https://html.spec.whatwg.org/multipage/interactive-elements.html#the-details-element) · [button](https://html.spec.whatwg.org/multipage/form-elements.html#the-button-element) | Current web semantics and pattern interpretation. Check the exact section when implementing. |
| [Guide-specific QA matrix](../testing-matrix/Design%20Patterns.md#design-patterns-testing-matrix) and [canonical ID definitions](../testing-matrix/Canonical%20References.md) | Find the Disclosure row, applicable canonical checks and test procedure links. |
| [ACT applicability review](../qa/Review.md#contradictions-and-applicability) and [matrix gaps](../testing-matrix/Review.md#coverage-gaps) | Resolve conflicts and keep WCAG, pattern expectations and usability evidence distinct. |
| [Guide checks](../Disclosure.md#testing-approach-and-act-scripts) and [supplied ACT-style cases](../qa/supplied/disclosure.md#act-test-cases) | Manual procedures and variant checks. |
| [ACT 2t702h](../qa/w3c-act/2t702h.md) · [ACT 97a4e1](../qa/w3c-act/97a4e1.md) · [ACT 6cfa84](../qa/w3c-act/6cfa84.md) | Run only where the rule’s actual applicability matches. Preserve each rule’s status and record the exact assertion. |

The supplied Cases 1 and 2 use custom state wording and a blanket focus-contrast threshold. For native HTML, verify the browser-exposed state. Assess WCAG text contrast and applicable non-text contrast separately. Reuse the focus, keyboard, visibility and fallback assertions across duplicate checks. The source’s numbering skips Case 3.

## Verification and traceability

D. The table names candidate agent checks. Tool or fixture availability determines what can be executed. Save the variant, fixture/build, browser or tool version, command, expected and observed outcome. Use the linked manual procedures for effects beyond the available automation.

| Outcome | Source checks | Agent evidence and limits |
| --- | --- | --- |
| DIS-01 | D1–D3; supplied 1, 2; ACT 2t702h | Inspect computed name and exposed state |
| DIS-02 | D1–D3; supplied 1, 2; CTL-161/162/163; ACT 97a4e1 | Assert computed name, IDREF, active element, hidden state and ARIA state |
| DIS-03 | D1–D3; supplied 1, 2; CTL-091/163/164; ACT 6cfa84 where applicable | Compare focus sequences and tree exposure; manually confirm screen reader browse order |
| DIS-04 | D1–D3; supplied 1, 2, 4, 5, 6; CTL-095 | Browser input and emulation where supported; human checks for voice, speech and visual judgement |
| DIS-05 | D4; supplied 7; DISC-03 | Compare blocked-script and disabled-CSS browser fixtures |

[Manual checks in the owning guide](../Disclosure.md#testing-approach-and-act-scripts) · [Supplied ACT-style manual procedures](../qa/supplied/disclosure.md#act-test-cases)
