---
title: "Component Semantics Contract"
description: "Specify what a component exposes, how it behaves across states and how to verify each variant."
document_id: "cosmos-component-semantics-contract"
document_type: "design_handover_template"
schema_version: "1.0"
language: "en-GB"
platform: "web"
audiences: ["designers", "engineers", "QA testers", "AI agents"]
related_guides: ["ARIA and Screen Reader Usability", "Visually Hidden Content", "Focus", "Progressive Enhancement"]
---

# Component Semantics Contract

Use a contract to record what people can find, understand and operate in a component. Give designers, engineers and testers the same expected behaviour for each state. For example, when a person opens a disclosure, record what they can read, where keyboard focus stays and how they reach the newly revealed content.

## Contents

- [When to write a contract](#when-to-write-a-contract)
- [Record the behaviour](#record-the-behaviour)
- [Contract template](#contract-template)
- [Worked example: custom disclosure](#worked-example-custom-disclosure)
- [Use the contract in design, implementation and QA](#use-the-contract-in-design-implementation-and-qa)
- [Sources](#sources)

## When to write a contract

Write one record per variant when the native element, semantic relationships, focus movement, keyboard commands or fallback differ. Use stable component, variant and check IDs so teams can link a code change or test result back to the same expectation. A native disclosure and an enhanced button disclosure need separate records because the browser supplies state and operation in one case and the implementation supplies them in the other.

Read the owning pattern guide first. Use it for Cosmos expectations and use the applicable standard for standards requirements. Record project decisions and evidence gaps when adapting a pattern. If a project changes the interaction, describe the user impact and test the changed variant. Link shared practices instead of copying them into each record. [Design Patterns](README.md#design-patterns)

## Record the behaviour

| Field | What to specify |
| --- | --- |
| Purpose and applicability | The task, component variant, intended host, dependencies and supported ways of using it. Identify inputs, browsers or assistive technology that still need confirmation. |
| Elements and semantics | The trigger, content and related controls. Give each an element or platform equivalent, role where applicable, visible label, accessible name and optional description. State which semantics come from the browser and which an author sets. Record the source and target of each programmatic relationship. |
| States | The starting state and each reachable state that changes what people can see, hear or operate. For relevant content, record visual visibility, accessibility-tree exposure, Tab reachability and the state shown on its control. |
| Transitions | The starting state, action or event, resulting state, focus destination and feedback. Include effects of cancel, dismiss, errors, interruption or saved state where the variant uses them. Record whether a particular input is optional or has an equivalent route. |
| Fallback | What remains usable when scripts or styles are unavailable, where applicable. Check these conditions separately. Note how the host recovers from delayed or failed enhancement if it can affect access. |
| Verification | An observable expected result for each important state or transition. Link the owning test and name the executor, method and evidence. Use separate fields for product contract status, standards relationships and test results. |

Record programmatic state alongside the visible result. For example, a collapsed panel can be out of the reading and Tab sequences while its trigger stays available. A hidden element referenced as an accessible name or description can still contribute text to that name or description. Check the actual relationship before interpreting exposure. Describe meaningful announcements without assuming every browser and screen reader uses the same words. [Visually Hidden Content](Visually%20Hidden%20Content.md) · [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md) · [Focus](Focus.md)

## Contract template

Copy the record into the host’s design system or component documentation. Replace placeholders with the variant’s behaviour. Use `null` for an unresolved value and explain what would resolve it under `review_notes`. Use an empty list when a reviewed list has no applicable items. The fields form a reusable YAML structure. The host can add its own fields and validation.

```yaml
schema_version: "1.0"
component_id: "<stable component ID>"
variant_id: "<stable variant ID>"
status: "draft"
task: "<what the person needs to do>"
scope:
  platform: "web"
  applies_when: "<applicability condition>"
  dependencies: []
  supported_environments: null
sources:
  - kind: "cosmos_guidance"
    location: "<owning guide section>"
elements:
  - id: "<element ID within the contract>"
    native_element: "<element or host equivalent>"
    exposed_role: "<role interop expectation, where applicable>"
    visible_label: null
    accessible_name: null
    accessible_description: null
    relationships: []
initial_state: "<state ID>"
states:
  - id: "<state ID>"
    programmatic_state: {}
    visibility: "<what is visible>"
    accessibility_tree: "<what is exposed>"
    tab_sequence: "<what Tab and Shift+Tab can reach>"
transitions:
  - from: "<state ID>"
    action: "<trigger and input or event>"
    to: "<state ID>"
    focus: "<where focus remains or moves>"
    feedback: "<visible and programmatic outcome>"
fallback:
  scripts_unavailable: null
  css_unavailable: null
checks:
  - id: "<stable check ID>"
    action: "<starting state and repeatable steps>"
    expected: "<observable result>"
    method: "<automated, assisted, manual or participant research>"
    source: "<guide section, procedure or canonical QA ID>"
    standards_relationships: []
    result_status: "not run"
review_notes: []
```

## Worked example: custom disclosure

The record applies to the enhanced button variant in the [Disclosure guide](Disclosure.md#custom-html). Its panel contains a paragraph. Add a fixture with a link or control in the panel to verify entry to and exit from the revealed Tab sequence. The example specifies expected behaviour. Test the host’s implementation before recording a result.

```yaml
schema_version: "1.0"
component_id: "disclosure"
variant_id: "enhanced-button"
status: "reference example"
task: "Read delivery and collection options on the current page."
scope:
  platform: "web"
  applies_when: "The host uses the Cosmos enhanced button disclosure."
  dependencies: ["JavaScript for the enhanced interaction"]
  supported_environments: null
sources:
  - kind: "cosmos_guidance"
    location: "Disclosure.md#custom-html"
  - kind: "qa_mapping"
    location: "testing-matrix/Design Patterns.md#design-patterns-testing-matrix"
elements:
  - id: "delivery-toggle"
    native_element: "button"
    exposed_role: "button"
    visible_label: "Delivery and collection options"
    accessible_name: "Delivery and collection options"
    accessible_description: null
    relationships:
      - attribute: "aria-controls"
        target_id: "delivery-options"
  - id: "delivery-options"
    native_element: "div"
    exposed_role: null
    visible_label: null
    accessible_name: null
    accessible_description: null
    relationships: []
initial_state: "collapsed"
states:
  - id: "collapsed"
    programmatic_state:
      delivery-toggle: {aria-expanded: "false"}
      delivery-options: {hidden: true}
    visibility: "Trigger visible. Panel hidden."
    accessibility_tree: "Named button exposes collapsed state. Panel content is absent."
    tab_sequence: "Trigger reachable. No stops within the panel."
  - id: "expanded"
    programmatic_state:
      delivery-toggle: {aria-expanded: "true"}
      delivery-options: {hidden: false}
    visibility: "Trigger and panel visible."
    accessibility_tree: "Named button exposes expanded state. Panel content is available after it."
    tab_sequence: "Trigger reachable. If the panel has a control, its first control follows the trigger."
transitions:
  - from: "collapsed"
    action: "Activate the focused button with Enter or Space, or activate it using pointer or touch."
    to: "expanded"
    focus: "Keep keyboard focus on the button when it already has focus."
    feedback: "Show the panel, update the marker and expose expanded state."
  - from: "expanded"
    action: "Activate the button again."
    to: "collapsed"
    focus: "Keep keyboard focus on the button when it already has focus."
    feedback: "Hide the panel, update the marker and expose collapsed state."
fallback:
  scripts_unavailable: "The panel is available as ordinary content and the inactive enhancement control is hidden."
  css_unavailable: "The button still toggles the panel. The stylesheet marker is unavailable. Check the trigger remains understandable and the state is programmatically exposed."
checks:
  - id: "disclosure-state-sync"
    action: "After enhancement, Tab to the button, press Enter to open and Space to close. Inspect after each action."
    expected: "Focus stays on the button. The expanded attribute and panel hidden state agree at each step."
    method: "automated"
    source: "CTL-161 and CTL-163"
    standards_relationships: []
    result_status: "not run"
  - id: "disclosure-panel-exposure"
    action: "Read from the trigger with a supported screen reader in each state. Repeat Tab navigation with a link in the panel."
    expected: "The name and state are available. The closed link is skipped and the open link follows the trigger."
    method: "manual"
    source: "Disclosure.md#testing-approach-and-act-scripts"
    standards_relationships: []
    result_status: "not run"
  - id: "disclosure-baseline"
    action: "Load with scripts blocked. Run again with scripts enabled and CSS disabled."
    expected: "With scripts blocked, the panel remains readable. With CSS disabled, the control and content remain usable."
    method: "manual"
    source: "Disclosure.md#testing-approach-and-act-scripts"
    standards_relationships: []
    result_status: "not run"
review_notes:
  - "Confirm the host browser, input and assistive technology support matrix."
```

The automated check draws on [CTL-161](testing-matrix/Canonical%20References.md#ctl-161) and [CTL-163](testing-matrix/Canonical%20References.md#ctl-163). The record leaves standards relationships empty because it makes no standards assessment for the host product.

The worked example uses the guide’s plain panel. Document the name and purpose of a region if the host uses one. Keep the native variant separate, with its own `details` state, browser-provided interaction and no-script behaviour. [Disclosure native HTML](Disclosure.md#native-html)

## Use the contract in design, implementation and QA

1. **Design.** Identify the variant and agree its states and transitions before sign-off. Resolve ambiguous labels, focus targets and fallbacks with the people responsible for the design and implementation.
2. **Implementation.** Render the selected elements and make the visual, programmatic and operable states agree. When fixing a component with existing callers, check their supported variants and interaction dependencies.
3. **QA.** Use the contract to select assertions. Automate deterministic checks such as DOM state and focus in a known fixture. Use supported assistive technology and manual checks to verify reading order, state communication and practical operation. Use representative tasks when evaluating discoverability or recovery. Record the build, variant, configuration, observed result and evidence. [Accessibility testing matrix](testing-matrix/README.md)
4. **Agent retrieval.** Retrieve this template with the owning guide, shared practices and applicable tests. Select by variant and host first. Preserve source IDs and status, carry conditions and exceptions with the check, and record uncertainty. A contract and its worked example describe expectations. Product evidence comes from testing the implementation.
5. **Maintenance.** Update the record and affected checks together when behaviour changes. Keep the same ID while the behaviour remains the same. Use a new variant ID when two behaviours are supported in parallel.

## Sources

- [Disclosure](Disclosure.md) — owns the worked example’s trigger, states and fallback.
- [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md) — names, roles, state and accessible feedback.
- [Visually Hidden Content](Visually%20Hidden%20Content.md) — visual, accessibility-tree and focus exposure.
- [Focus](Focus.md) and [Progressive Enhancement](Progressive%20Enhancement.md) — focus behaviour and a usable baseline.
- [Design patterns testing matrix](testing-matrix/Design%20Patterns.md) and [canonical QA references](testing-matrix/Canonical%20References.md) — test selection, scope and standards relationships.
