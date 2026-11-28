---
title: "Cosmos accessibility testing matrix"
description: "Map Cosmos design patterns and practices to canonical QA tests, reproducible checks and usability evidence."
document_type: "qa_cross_reference"
language: "en-GB"
scope: "Indexed Cosmos web patterns and practices"
status: "Planning reference"
---

# Accessibility testing matrix

Use this cross-reference to choose verification for a Cosmos guide and turn it into reproducible tests for a specific build. It covers 38 design patterns and 28 design practices. The selections link to 191 canonical QA tests and 66 supporting procedures. The matrix describes planned evidence. Record results only after execution in the product.

| Use | Open |
| --- | --- |
| Select checks for components and service journeys | [Design patterns](Design%20Patterns.md) |
| Select checks for shared principles and techniques | [Design practices](Design%20Practices.md) |
| Resolve an ID to its title and exact standards relationship | [Canonical references](Canonical%20References.md) |
| Resolve overlaps, conflicting expectations and missing mappings | [Review and coverage gaps](Review.md) |
| Find Cosmos ACT-style cases and imported W3C rule text | [Targeted accessibility QA](../qa/README.md) |

## Choose the evidence

| Question | Evidence and decision |
| --- | --- |
| Does the build satisfy an applicable WCAG 2.2 A/AA requirement? | Use the pinned canonical relationship register to select relevant checks. Read the normative criterion and test its entire applicable scope and exceptions. A partial relationship supplies evidence for part of the criterion. A related technique or organisation-policy mapping supplies design context. |
| Does the build meet a Cosmos or product best practice? | Use a reproducible assertion with the guide’s variant, expected outcome and any approved product policy. Record the source and the impact of failing it. |
| Can people use the pattern or complete the journey comfortably and independently? | Use representative tasks with a defined completion condition. Record observed completion, wrong turns, time and assistance where useful. Record participant reports about effort, confidence or confusion separately from the measurements and state the sample and context. |

## Allocate execution

| Executor | Use it for | Evidence boundary |
| --- | --- | --- |
| Automated tool or browser test | Explicit assertions such as computed name existence, role, focus destination, attribute state, known data, target geometry in a defined fixture, and changes after keyboard actions. Use an accessibility scanner only where an exact rule has verified applicability. Use browser assertions for scripted transitions. | Save tool and ruleset version, rule or assertion ID, fixture and state, raw result and known exclusions. Geometry or a screenshot can flag overlap but a human may still need to judge whether meaning or operation was lost. A missing or unverified tool mapping is not coverage. |
| AI agent | Retrieve and cross-reference tests, draft fixture steps, run authorised browser assertions, compare structured outputs, group possible duplicates and flag discrepancies. Ask it to quote the observed evidence or link to raw results, identify uninspected states and separate observation from inference. | A result produced by an agent executing a deterministic assertion is automated evidence with the same limits. An agent’s interpretation is assisted review. A text simulation cannot verify spoken announcements, experienced difficulty or the actual behaviour of an untested build. |
| Human technical tester | Check relevance of names, text alternatives and feedback, logical reading and focus order, practical recovery and actual screen reader, speech input, touch or magnification behaviour. Inspect states where automation only supplies signals. | Use the technical action and expected result, the supported environment and direct observations. Compare announced information and timing rather than requiring exact speech. Report uncertain results for review. |
| Human evaluator or research participant | Assess discoverability, comprehension, effort, independence and confidence. Use the matrix’s usability question to define a task prompt and success measure, then test with appropriate participants and safeguards. | Use neutral task prompts, comparable task conditions and an agreed rubric. Record both what happened and what the participant reports. Avoid turning one preference or small sample into a general conclusion. |

The **Candidate scripted evidence** column describes assertions a team could design and validate. No coverage is assigned to a named scanner. For human technical review, start with the **Technical check** and inspect whether the interaction remains meaningful, operable and recoverable in the supported modes.

## Record and reuse evidence

1. Choose the guide and variant. Use a representative route or fixture, test account and supported browser, device, input and assistive technology matrix.
2. Record the guide section, canonical ID and title, procedure or ACT rule ID where applicable, standards relationship, initial state, action, concrete expected outcome, executor and observed result. Keep WCAG, best-practice and usability results in separate fields.
3. Use **pass**, **fail**, **not applicable**, **not run**, **blocked** or **cannot tell**, with the reason and evidence. Record the build, tool configuration and run date. Keep baseline, change and recovery states separate.
4. Reuse evidence when the assertion, variant, fixture/state, environment and expected outcome are equivalent. Keep distinct IDs and record their evidence relationship. Run separate checks when an exception, scope or interaction changes the outcome.
5. For usability, define success and assistance before testing. For example, in a search with zero results, record whether a person reaches relevant results without a hint, what they change, and what they report about why the first search failed. Keep participant observations separate from the technical zero-results announcement check.

## Source baseline

- [Cosmos revision](https://github.com/Double-FW/Cosmos-UX-Design/tree/2ea4fcbfab8b559e1172b26232cd668a79806167) — guide index, pattern guidance and local QA procedures.
- [QA canonical test library](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Canonical%20Test%20Library.md) and [WCAG A/AA relationship register](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Canonical_WCAG_AA_Relationship_Register.md) — stable IDs and standards classifications.
- [QA ACT-style test system](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md) and [Mobile Application and Website Test Library](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Mobile_Application_and_Website_Test_Library.md) — procedure design and detailed supplementary guidance.
- [QA automated-to-manual cross-reference](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Automated-to-Manual%20Cross-Reference%20for%20the%20Canonical%20Accessibility%20Tests.md) — tool evidence limits and validation fields.
- [QA test management approach](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Testing_Approach.md) and [comparative evaluation evidence map](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/QA_and_Comparative_Evaluation_Evidence_Map.md) — technical and task evidence.
- [Rolling Research and A/B Testing](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Persona_Led_Reporting/Rolling_Research_and_AB_Testing.md) — interpreting participant and comparative evidence.
- [WCAG 2.2 Recommendation](https://www.w3.org/TR/WCAG22/) — normative requirements.

On update, compare the pinned revisions, confirm guide and test identities and review changed applicability. The QA register owns standards relationships. Cosmos owns pattern variants and their expected behaviour. Keep conflicts visible until the relevant authority or product owner resolves them.
