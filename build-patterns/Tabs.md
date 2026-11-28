# Build Tabs

## A. Standalone build prompt

Choose a related section and read its content. For a new set assume horizontal manual activation. Confirm orientation, activation policy, fragment and history policy, first meaningful panel content and any maintained legacy keyboard model. Use ordinary links to headed sections until enhancement succeeds. Four or five labels is a design heuristic.

The supplied focus procedure expects focus in the panel. Applying it to standard tabs would make arrow exploration stop after activation. Use that procedure only for the existing panel-focus model. Standard horizontal tabs use Left and Right, vertical tabs Up and Down. Home and End are optional. Automatic activation needs confirmation with actual panel latency.

Use native HTML where it supplies the behaviour. Use host components and brand tokens where they preserve these outcomes. Before coding, copy the [Cosmos semantics contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) for each materially different variant. Record names, roles, states, relationships, focus, keyboard commands, feedback, fallback, recovery and the supported environments. Mark unresolved host decisions. Deliver the implementation, the contract reconciled against the rendered interface, and evidence against the [Tabs matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L21). Accept each applicable scenario only with observed evidence. Record pass, fail, not applicable or not run and explain exclusions and gaps.

```gherkin
Feature: Tabs

  Scenario: TABS-P2-01 — The person selects a tab by click or tap
    Given an enhanced named tablist has two or more labelled tabs
    When the person selects a tab by click or tap
    Then only its associated labelled panel is exposed, only that tab is selected and the other panels and their controls leave the reading and Tab sequences

  Scenario: TABS-P2-02 — The person arrows past the last tab then activates the focused tab with Enter or Space
    Given focus has entered a standard manual tablist on its selected tab
    When the person arrows past the last tab then activates the focused tab with Enter or Space
    Then focus wraps to the first tab, selection waits for activation and focus stays on that tab

  Scenario: TABS-P2-03 — The person moves focus with an orientation arrow
    Given a standard automatically activated tablist has immediate panels
    When the person moves focus with an orientation arrow
    Then selection follows focus and the matching content appears without interrupting exploration

  Scenario: TABS-P2-04 — The person uses Down and Up across its ends
    Given a vertical tablist exposes vertical orientation
    When the person uses Down and Up across its ends
    Then focus wraps in the corresponding direction under the declared activation policy

  Scenario: TABS-P2-05 — The person presses Tab then Shift+Tab
    Given manual arrow navigation has left focus on an unselected tab
    When the person presses Tab then Shift+Tab
    Then Tab reaches the active panel start or its first meaningful control and reverse entry returns to the selected tab

  Scenario: TABS-P2-06 — The person activates a tab with Enter, Space, click or tap
    Given an existing panel-focus variant exposes each tab in the Tab sequence
    When the person activates a tab with Enter, Space, click or tap
    Then focus moves to its named panel outside the Tab sequence, Tab continues into or after the content and Shift+Tab from the panel returns to the list

  Scenario: TABS-P2-07 — The person follows a section link
    Given scripts are blocked or enhancement fails before initialisation
    When the person follows a section link
    Then all headed sections remain visible and each link reaches its destination without tab roles

  Scenario: TABS-P2-08 — The page loads
    Given a URL identifies a panel or a descendant in it
    When the page loads
    Then the matching panel opens before its target is reached, an unrelated fragment keeps its own destination and an unmatched target leaves the first panel selected

  Scenario: TABS-P2-09 — The person selects another panel twice then uses Back and Forward
    Given selection updates history and the initial URL has no fragment
    When the person selects another panel twice then uses Back and Forward
    Then one Back restores the default panel, Forward restores the selected panel and focus remains usable even if its former panel closes

  Scenario: TABS-P2-10 — The person uses forced colours, narrow width and enlarged text in separate runs
    Given focus and selection can occupy different tabs
    When the person uses forced colours, narrow width and enlarged text in separate runs
    Then labels remain readable, focus and selection distinguishable and the visible orientation matches the keyboard commands
```

Scenario IDs map to the following checks in scenario order. TABS-P2-01 → CTL-167, CTL-169 · TABS-P2-02 → PRACTICE-KEY-01 · TABS-P2-03 → Cosmos standard tabs · TABS-P2-04 → Cosmos standard tabs · TABS-P2-05 → PRACTICE-KEY-01 · TABS-P2-06 → Supplied Test Case 1 · TABS-P2-07 → CTL-168, PRACTICE-PE-01 · TABS-P2-08 → Cosmos direct links · TABS-P2-09 → Cosmos history check · TABS-P2-10 → CTL-004, Supplied Test Cases 4 and 5.

## B. Retrieval pack

| Retrieve | Question or decision |
| --- | --- |
| [Owning guide, recommended behaviour](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Tabs.md#recommended-behaviour) | Which focus, feedback and recovery decisions apply to this variant? |
| [Original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Tabs.md#reference-implementation) | Which details belong to the illustrative markup? The supplied focus procedure expects focus in the panel. Applying it to standard tabs would make arrow exploration stop after activation. Use that procedure only for the existing panel-focus model. Standard horizontal tabs use Left and Right, vertical tabs Up and Down. Home and End are optional. Automatic activation needs confirmation with actual panel latency. |
| [Contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) | Which states, transitions and evidence fields must travel with the implementation? |
| [Pinned matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L21) | Which canonical IDs apply and which outcomes require the owning guide? |
| [Client-side Routing](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Client-side%20Routing.md#back-and-forward) | Restore selection when browser history changes. |
| [Progressive Enhancement](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Progressive%20Enhancement.md#implementation) | Choose when to replace ordinary links with widget semantics. |
| [PRACTICE-KEY-01](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L651) | Exercise the complete keyboard route. |
| [PRACTICE-PE-01](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L699) | Check the core task without enhancement. |
| [ARIA required owned elements (bc4a75)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/bc4a75.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Element with role attribute has required states and properties (4e8ab6)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/4e8ab6.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/6cfa84.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/browsing-the-web.html#scroll-to-fragid) | Checked 28 November 2026. HTML fragment navigation. The tab keyboard model is a Cosmos choice based on APG. |
| [APG tabs keyboard interaction](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) | Compare automatic and manual interaction when choosing the standard variant. |

## C. Verification plan

Run each row from its Given state using its When action. Compare with the complete Then outcome. Record method and executor separately. Each row starts as **not run** for the host.

| Scenario | Existing coverage | Applicability and method | Expected result | Evidence |
| --- | --- | --- | --- | --- |
| TABS-P2-01 | CTL-167, CTL-169 | Automated. Run when an enhanced named tablist has two or more labelled tabs. | Only its associated labelled panel is exposed, only that tab is selected and the other panels and their controls leave the reading and tab sequences. | Browser assertion trace with before and after state. |
| TABS-P2-02 | PRACTICE-KEY-01 | Automated. Run when focus has entered a standard manual tablist on its selected tab. | Focus wraps to the first tab, selection waits for activation and focus stays on that tab. | Browser assertion trace with before and after state. |
| TABS-P2-03 | Cosmos standard tabs | Human judgement. Run when a standard automatically activated tablist has immediate panels. | Selection follows focus and the matching content appears without interrupting exploration. | Participant or tester observation, task result and supported environment. |
| TABS-P2-04 | Cosmos standard tabs | Automated. Run when a vertical tablist exposes vertical orientation. | Focus wraps in the corresponding direction under the declared activation policy. | Browser assertion trace with before and after state. |
| TABS-P2-05 | PRACTICE-KEY-01 | Automated. Run when manual arrow navigation has left focus on an unselected tab. | Tab reaches the active panel start or its first meaningful control and reverse entry returns to the selected tab. | Browser assertion trace with before and after state. |
| TABS-P2-06 | Supplied Test Case 1 | Automated. Run when an existing panel-focus variant exposes each tab in the Tab sequence. | Focus moves to its named panel outside the tab sequence, tab continues into or after the content and shift+tab from the panel returns to the list. | Browser assertion trace with before and after state. |
| TABS-P2-07 | CTL-168, PRACTICE-PE-01 | Automated. Run when scripts are blocked or enhancement fails before initialisation. | All headed sections remain visible and each link reaches its destination without tab roles. | Browser assertion trace with before and after state. |
| TABS-P2-08 | Cosmos direct links | Automated. Run when a URL identifies a panel or a descendant in it. | The matching panel opens before its target is reached, an unrelated fragment keeps its own destination and an unmatched target leaves the first panel selected. | Browser assertion trace with before and after state. |
| TABS-P2-09 | Cosmos history check | Automated. Run when selection updates history and the initial URL has no fragment. | One back restores the default panel, forward restores the selected panel and focus remains usable even if its former panel closes. | Browser assertion trace with before and after state. |
| TABS-P2-10 | CTL-004, Supplied Test Cases 4 and 5 | Human judgement. Run when focus and selection can occupy different tabs. | Labels remain readable, focus and selection distinguishable and the visible orientation matches the keyboard commands. | Participant or tester observation, task result and supported environment. |

Use the [manual procedures](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/supplied/tabs.md) to assess panel discovery by speech, screen reader browsing, forced colours and layout. Apply panel-focus expectations only to the maintained variant. These are Cosmos or supplied checks. The proposed W3C ACT rules above cover narrower element-level expectations.

## D. Definition of done

Accept a variant when the rendered contract matches every applicable transition, the host tests pass and the human evaluations record successful task completion. Record build, input, browser, device, assistive technology, observed outcome and evidence per scenario. Give a reason for every not applicable or not run result. A fail needs correction and retest. An applicable not run result leaves acceptance open.

Confirm the chosen keyboard model with real panel content, history integration and real latency for automatic activation. Ask a person using a supported screen reader to find a panel and return.
