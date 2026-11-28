# Build Disclosure

## A. Standalone build prompt

Read optional details in place and return to the same point. Prefer native details with a first summary. Use an enhanced button when the host needs custom behaviour. Keep the panel immediately after the trigger. A named region is optional. Confirm initial openness, meaningful label and whether the panel contains a control.

The worked contract uses a paragraph. Its example cannot exercise the next-Tab requirement. Include an interactive descendant in a test fixture. The native relationship comes from containment. For the custom variant use a button, synchronise aria-expanded and hidden, and link aria-controls to the panel. Keep decorative state marks out of the name.

Use native HTML where it supplies the behaviour. Use host components and brand tokens where they preserve these outcomes. Before coding, copy the [Cosmos semantics contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) for each materially different variant. Record names, roles, states, relationships, focus, keyboard commands, feedback, fallback, recovery and the supported environments. Mark unresolved host decisions. Deliver the implementation, the contract reconciled against the rendered interface, and evidence against the [Disclosure matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L17). Accept each applicable scenario only with observed evidence. Record pass, fail, not applicable or not run and explain exclusions and gaps.

```gherkin
Feature: Disclosure

  Scenario: disclosure-state-sync — The person opens it with Enter and closes it with Space
    Given a named, collapsed control has keyboard focus
    When the person opens it with Enter and closes it with Space
    Then the panel opens then closes, focus stays on the control, and its visible indicator and exposed expanded state match each change

  Scenario: disclosure-panel-exposure — The person presses Tab, returns to the control, closes it and presses Tab again
    Given the open panel contains a link and focus is on the control
    When the person presses Tab, returns to the control, closes it and presses Tab again
    Then focus reaches the panel link on the first pass and skips it on the second

  Scenario: disclosure-baseline — The page loads with scripts blocked
    Given the page uses native disclosure or an enhanced button
    When the page loads with scripts blocked
    Then the native summary still toggles or the custom panel is readable directly without an inert trigger

  Scenario: DISC-P2-04 — The person focuses and opens the control in forced colours and again at 200% text size
    Given the trigger and panel use host styles
    When the person focuses and opens the control in forced colours and again at 200% text size
    Then the person can distinguish focus and state and read the full label and content

  Scenario: DISC-P2-05 — The person opens and closes the disclosure
    Given style sheets are disabled
    When the person opens and closes the disclosure
    Then the control remains identifiable and panel text is available when open

  Scenario: DISC-P2-06 — They find the trigger by its visible label and toggle it
    Given a person is using a supported screen reader or voice control
    When they find the trigger by its visible label and toggle it
    Then they can identify what will open and determine whether its content is available
```

Scenario IDs map to the following checks in scenario order. disclosure-state-sync → DISC-01 · disclosure-panel-exposure → CTL-163, CTL-164 · disclosure-baseline → DISC-03 · DISC-P2-04 → DISC-02, DISC-03 · DISC-P2-05 → Cosmos checkpoint 4 · DISC-P2-06 → Supplied Test Cases 2 and 4.

## B. Retrieval pack

| Retrieve | Question or decision |
| --- | --- |
| [Owning guide, recommended behaviour](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#recommended-behaviour) | Which focus, feedback and recovery decisions apply to this variant? |
| [Original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#reference-implementation) | Which details belong to the illustrative markup? The worked contract uses a paragraph. Its example cannot exercise the next-Tab requirement. Include an interactive descendant in a test fixture. The native relationship comes from containment. For the custom variant use a button, synchronise aria-expanded and hidden, and link aria-controls to the panel. Keep decorative state marks out of the name. |
| [Contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) | Which states, transitions and evidence fields must travel with the implementation? |
| [Pinned matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L17) | Which canonical IDs apply and which outcomes require the owning guide? |
| [Visually Hidden Content](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Visually%20Hidden%20Content.md#choose-the-intended-visibility) | Decide whether closed content should be read or reached. |
| [Component Semantics Contract](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#worked-example-custom-disclosure) | Identify the three existing check IDs for the enhanced button. |
| [DISC-01](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L249) | Exercise keyboard toggle and check spoken state. |
| [DISC-02](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L257) | Inspect visible focus. |
| [DISC-03](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L265) | Check forced colours, scaling and script fallback. |
| [Summary element has non-empty accessible name (2t702h)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/2t702h.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Button has non-empty accessible name (97a4e1)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/97a4e1.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/6cfa84.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/interactive-elements.html#the-details-element) | Checked 28 November 2026. Disclosure widget, open state and summary activation. |

## C. Verification plan

Run each row from its Given state using its When action. Compare with the complete Then outcome. Record method and executor separately. Each row starts as **not run** for the host.

| Scenario | Existing coverage | Applicability and method | Expected result | Evidence |
| --- | --- | --- | --- | --- |
| disclosure-state-sync | DISC-01 | Automated. Run when a named, collapsed control has keyboard focus. | The panel opens then closes, focus stays on the control, and its visible indicator and exposed expanded state match each change. | Browser assertion trace with before and after state. |
| disclosure-panel-exposure | CTL-163, CTL-164 | Automated. Run when the open panel contains a link and focus is on the control. | Focus reaches the panel link on the first pass and skips it on the second. | Browser assertion trace with before and after state. |
| disclosure-baseline | DISC-03 | Automated. Run when the page uses native disclosure or an enhanced button. | The native summary still toggles or the custom panel is readable directly without an inert trigger. | Browser assertion trace with before and after state. |
| DISC-P2-04 | DISC-02, DISC-03 | Human judgement. Run when the trigger and panel use host styles. | The person can distinguish focus and state and read the full label and content. | Participant or tester observation, task result and supported environment. |
| DISC-P2-05 | Cosmos checkpoint 4 | Automated. Run when style sheets are disabled. | The control remains identifiable and panel text is available when open. | Browser assertion trace with before and after state. |
| DISC-P2-06 | Supplied Test Cases 2 and 4 | Human judgement. Run when a person is using a supported screen reader or voice control. | They can identify what will open and determine whether its content is available. | Participant or tester observation, task result and supported environment. |

Use the [manual procedures](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/supplied/disclosure.md) to assess spoken state, voice activation by label and focus visibility. These are Cosmos or supplied checks. The proposed W3C ACT rules above cover narrower element-level expectations.

## D. Definition of done

Accept a variant when the rendered contract matches every applicable transition, the host tests pass and the human evaluations record successful task completion. Record build, input, browser, device, assistive technology, observed outcome and evidence per scenario. Give a reason for every not applicable or not run result. A fail needs correction and retest. An applicable not run result leaves acceptance open.

Confirm the hidden panel has no reachable descendants and observe the spoken state and label in the host.
