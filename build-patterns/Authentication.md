---
title: "Build authentication"
description: "Standalone build brief, behaviour scenarios, retrieval pack and verification routes for authentication."
document_type: "pattern_build_specification"
language: "en-GB"
pattern: "authentication"
status: "pilot"
---

# Build authentication

## Standalone build prompt

Implement the selected web variant in the host’s framework, design system and brand. Use the existing host primitives where their rendered HTML and behaviour satisfy the outcomes below. Use semantic HTML, native controls and token-driven presentation. In the absence of a host, build a plain HTML fixture with an explicitly simulated service. Ask only for missing input that prevents useful work.

- Record the selected variant, user task, scope, the host stack, design tokens and supported browsers, devices, input modes and assistive technologies. Mark unresolved dependencies and assumptions.
- Consult the current WHATWG HTML Living Standard for the relevant elements and attributes. Record the section and date consulted in the handover. [forms](https://html.spec.whatwg.org/multipage/forms.html#the-form-element) · [autocomplete](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill) · [input modes](https://html.spec.whatwg.org/multipage/interaction.html#input-modalities:-the-inputmode-attribute)
- Before implementation, create or update a versioned [Component Semantics Contract](../Component%20Semantics%20Contract.md) for the selected variant. Record elements, names, roles, relationships, initial and reachable states, focus, announcements, fallback and check IDs. Preserve this record for future changes. Update the contract when implementation or verification changes what is supported.
- Deliver the implementation, semantics contract and verification results keyed by scenario ID. Apply every scenario whose preconditions match the selected variant. Use **pass**, **fail**, **not applicable**, **not run** or **cannot tell**, with evidence or reasons. Treat the [QA test matrix](../testing-matrix/README.md) as the source crosswalk. It is not a substitute for evidence from the rendered product.

Build only the web interface and approved integration for the declared journey: enrolment, sign-in, verification, recovery, session return and sign-out where in scope. Ask for the approved identity provider or service contract, allowed methods, recovery paths, retention policy and safe test fixtures when they materially block implementation. Without them deliver a clearly labelled UI fixture with deterministic simulated responses. Never use real credentials or replace backend identity decisions. Use a native labelled form with explicit submit, appropriate autocomplete tokens for username, current and new passwords, and one-time code; permit paste and password managers. Mask passwords by default and give an optional non-submit toggle a name describing its next action. Restore masking on submit and when the page returns through browser history if the visibility enhancement is enabled. Use a single text input for codes, with numeric input mode only for digits-only codes. Provide accessible error, resend and alternative routes and task return after successful verification. Use WCAG 2.2 SC 3.3.8 at every applicable step: offer a route that avoids the cognitive test, provide an assisting mechanism or document an applicable exception. Treat security assurance as a service-owner review. Use authorised synthetic fixtures only.

## Gherkin acceptance outcomes

The block is a portable feature specification. To run it in a BDD runner, bind the steps to host fixtures and assertions. For agent execution without a BDD runner, perform the actions and capture the same evidence against each scenario ID.

```gherkin
Feature: Accessible authentication
  Build the variant declared in the build brief.
  Evaluate conditional scenarios only where their Given conditions apply.

  @AUTH-01
  Scenario: Labels and assisted entry support authentication
    Given a supported password sign-in or password creation form
    When the person completes the form with keyboard, paste, autofill or a supported password manager
    Then visible labels and computed names identify the fields, autocomplete serves the declared purpose and submission occurs only on explicit activation

  @AUTH-02
  Scenario: Password visibility is reversible
    Given a masked password form with a visibility control
    When the person toggles visibility with keyboard input and then submits or returns through browser history
    Then toggling preserves the value and focus, changes the action name and does not submit the form, and the password is masked again on submission or page return

  @AUTH-03
  Scenario: Verification accepts a complete code and supports expiry recovery
    Given a verification form with a digits-only test code starting with zero
    When the person pastes the complete code, then encounters a simulated expiry and requests another
    Then the full text including the leading zero is retained, the feedback explains the next action and resend does not demand unrelated input

  @AUTH-04
  Scenario: The user can recover when a method is unavailable
    Given an offered verification method that can be cancelled or is unavailable
    When the person cancels or attempts to continue without the usual authenticator
    Then an approved alternative or recovery route is reachable and each in-scope authentication step avoids a cognitive test, offers assistance or has a documented applicable exception

  @AUTH-05
  Scenario: Failure gives a usable next step
    Given a supported sign-in, reset or provider-return failure using approved synthetic responses
    When the person reaches the failure and follows feedback
    Then they can reach the stated retry or recovery route, errors identify what can be corrected and messaging does not disclose account state

  @AUTH-06
  Scenario: Reauthentication restores eligible work
    Given an authorised task with progress eligible for retention
    When the session expires and the person reauthenticates
    Then they can tell what was retained and resume the authorised task with eligible progress intact and credentials excluded from drafts

  @AUTH-07
  Scenario: Authentication remains usable under display changes and lost enhancement
    Given an authentication route with its approved fallback
    When the person enlarges text, narrows the viewport, uses forced colours or blocks scripts in separate runs
    Then fields, errors and focus remain usable and the baseline or an accessible alternative remains available
```

## Retrieval pack

| Retrieve | Use it for |
| --- | --- |
| [Owning pattern guide](../Authentication.md) | Purpose, named variants, markup and interaction; apply only to the declared variant. |
| [Pinned original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#reference-implementation) | Source fixtures and code decisions reverse-engineered into the outcomes above. Preserve this revision when comparing or restoring examples. |
| [Component Semantics Contract](../Component%20Semantics%20Contract.md#contract-template) | Record a durable shared expectation before coding. |
| [forms](https://html.spec.whatwg.org/multipage/forms.html#the-form-element) · [autocomplete](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill) · [input modes](https://html.spec.whatwg.org/multipage/interaction.html#input-modalities:-the-inputmode-attribute) | Current web semantics and pattern interpretation. Check the exact section when implementing. |
| [Guide-specific QA matrix](../testing-matrix/Design%20Patterns.md#design-patterns-testing-matrix) and [canonical ID definitions](../testing-matrix/Canonical%20References.md) | Find the Authentication row, applicable canonical checks and test procedure links. |
| [ACT applicability review](../qa/Review.md#contradictions-and-applicability) and [matrix gaps](../testing-matrix/Review.md#coverage-gaps) | Resolve conflicts and keep WCAG, pattern expectations and usability evidence distinct. |
| [Guide checks](../Authentication.md#testing-approach-and-act-scripts) | Manual procedures and variant checks. |
| [ACT e086e5](../qa/w3c-act/e086e5.md) · [ACT 73f2c2](../qa/w3c-act/73f2c2.md) · [ACT 97a4e1](../qa/w3c-act/97a4e1.md) | Run only where the rule’s actual applicability matches. Preserve each rule’s status and record the exact assertion. |
| [WCAG 2.2 SC 3.3.8](https://www.w3.org/TR/WCAG22/#accessible-authentication-minimum) and [Forms error guidance](../Forms.md#error-messages) | Evaluate the complete journey and field-specific recovery separately. |

There is no supplied Authentication-specific ACT suite or canonical matrix relationship for SC 3.3.8. Use the owning guide and the normative criterion. ACT rules cover naming and token semantics, while the journey checks depend on an authorised identity integration. Approve assurance, service response handling and credential protection with the service owner.

## Verification and traceability

A. The table names candidate agent checks. Tool or fixture availability determines what can be executed. Save the variant, fixture/build, browser or tool version, command, expected and observed outcome. Use the linked manual procedures for effects beyond the available automation.

| Outcome | Source checks | Agent evidence and limits |
| --- | --- | --- |
| AUTH-01 | A1; ACT e086e5/73f2c2/97a4e1; CTL-072/118; SC 3.3.8 | Assert accessible names, tokens, paste and submit; verify real autofill and password managers separately |
| AUTH-02 | A1; password fixture; ACT 97a4e1 | Assert type, accessible name, value, focus and submit count |
| AUTH-03 | A2; code fixture; ACT e086e5/73f2c2; CTL-126 | Assert input value, attributes, explicit submit and fixture expiry/resend handling |
| AUTH-04 | A3; SC 3.3.8 | Exercise fixture routing; verify actual platform prompts, supported methods and criterion exceptions with direct evidence |
| AUTH-05 | A4; CTL-126; PRACTICE-FORM-02 | Check fixture message, links, state and associations; human review of service behaviour and recovery |
| AUTH-06 | A5; CTL-129/198/199; Cosmos journey | Exercise synthetic state and destination; require real integration evidence for session and retention behaviour |
| AUTH-07 | A6; password fixture; PRACTICE-FORM-01 | Check browser layout and baseline button/form semantics; human visual and assistive technology confirmation |

[Manual checks in the owning guide](../Authentication.md#testing-approach-and-act-scripts). Use approved service fixtures and supported password managers, assistive technology and authenticators.
