# Cross-reference review

Use this review with the [design patterns matrix](Design%20Patterns.md), [design practices matrix](Design%20Practices.md) and [Cosmos ACT-script review](../qa/Review.md). Findings describe relationships between documentation at the pinned revisions. No product, scanner or assistive technology runs were performed.

## Variant and expectation differences

| Area | Difference | How to score or resolve |
| --- | --- | --- |
| Accordion and disclosure | CTL-165 expects a heading and button. Cosmos also permits native `details` and `summary`. CTL-166 applies to exclusive accordions. | Run the custom markup check only for the custom variant. Test exposed role, state and behaviour for native disclosures. Apply single-open handling only where configured. |
| Drawer and information panels | PANEL-01 expects focus to move inside on opening and to return on close. Cosmos allows focus to stay on a simple non-modal trigger and preserves a usable outside target on dismissal. | Use separate modal and non-modal fixtures. Assert the declared destination and avoid treating an intentional trigger or outside target as a failure. |
| Mega menu | NAV-01 expects Escape to return focus to the trigger. Cosmos keeps focus on another usable navigation control when it is already there. | Run Escape from inside the panel and from another navigation control. Compare the two destinations against the guide. |
| Modal dialogs | MODAL-03 includes background scroll locking. CTL-173 and Cosmos prioritise inoperability of background content. CHAIN-01 assumes an existing dialog sequence. | Verify background keyboard, pointer and assistive technology interaction independently of scroll behaviour. Test scroll lock as a separate product assertion where required. Prefer one modal and scope chain checks to an implemented chain. |
| Carousels | CTL-179 prohibits auto-advance. CAR-01 checks pause/stop when auto-advance exists. Cosmos permits explicit opt-in rotation with limits. | Record CTL-179 as a stricter best-practice outcome. Assess the implemented opt-in variant against Cosmos and apply the actual conditions of [WCAG 2.2.2](https://www.w3.org/TR/WCAG22/#pause-stop-hide) separately. |
| Tabs and routing | The imported Tabs procedure can expect panel focus. Standard Cosmos tabs use roving focus. CTL-148 expects focus at the start of the new view, while Cosmos permits return to the previous usable results position. | Split tabs by focus model and routing by first load, in-app change and history restoration. Use the variant’s expected focus and test meaningful sequence separately. |
| Cards and promos | CTL-189 expects card identity first. Cosmos linked promos can put an informative image before the headline. CTL-185 and PROMO-01 focus on one primary navigation target, while Cosmos also documents playable previews and independently operable secondary actions. | Use the headline-first contract for the applicable card variant. Review meaningful image-first reading order against the promo guide. Apply linked-headline checks to link-led promos and use the Cosmos interaction tests for previews and secondary controls. |
| Focus, contrast and targets | Some Cosmos supplied scripts expect 4.5:1 for focus or icons or a blanket minimum target. CTL-010 and CTL-110 distinguish applicability. CTL-105 can be misread as banning focusable scroll containers. | Check [focus contrast](Canonical%20References.md#ctl-010) and [target size](Canonical%20References.md#ctl-110) with the register’s scope. Assess ordinary/large text and non-text cues separately. Treat meaningful keyboard scrolling as a function. The larger target in CTL-110 is a design recommendation. |
| Labels and errors | CTL-073 says names must match or closely align. Cosmos expects the visible wording in the computed accessible name. The imported form suite also contains an unrelated promo check. | Use the actual [Label in Name requirement](https://www.w3.org/TR/WCAG22/#label-in-name) where applicable. Use visible text and computed names as evidence. Exclude Form Expectation 7 and use the linked promo test on promos. |
| Video | CTL-183 is phrased around speech. CTL-184 includes equivalents and is partial against both 1.2.3 and 1.2.5. | Verify meaningful non-speech sounds in captions where applicable. Evaluate the applicable media alternative and audio-description requirements individually, with the actual media fixture. |
| Tooltips | General naming, contrast and focus checks support the guide. The QA ACT-style library has no dedicated tooltip procedure. A hidden tooltip can still contribute its description to the trigger. | Use the [Cosmos tooltip cases](../Tooltips.md#testing-approach-and-act-scripts) for persistence, hover, Escape with focus or pointer, touch and trigger description. Score separate-item exposure separately from description. |

## Link purpose and external navigation

- **CTL-136** is marked direct for 2.4.4 in the relationship register, while the library’s test requires link purpose in isolation. [WCAG 2.2 SC 2.4.4](https://www.w3.org/TR/WCAG22/#link-purpose-in-context) allows programmatically determined context subject to the criterion’s ambiguity exception. Treat isolated clarity as a stronger usability or best-practice assertion. Use the criterion’s complete applicability for A/AA. Ask the QA source owner to qualify the register’s scope note.
- **CTL-084 and CTL-139** substantially overlap for external-destination signalling. EXT-01 specifies a visible icon. Cosmos also allows descriptive text or shared context where appropriate. Use one destination-awareness procedure and record the implemented cue. Add a separate **CTL-085** check when the link opens a new tab or window. The register treats these relationships as organisation policy.

## Reuse overlapping checks

| Families | Shared evidence | Keep separate |
| --- | --- | --- |
| CTA, links, cards, promos and share tools | Roles, names and destinations for the same rendered control | Context-change warnings, label clarity in a link list, play and secondary actions |
| Accordion, disclosure, preview and menu | Expanded state, visible panel and hidden descendants for a matching control | Native versus custom semantics, exclusive state, focus continuation and navigation dismissal |
| Landmarks, page structure, menus and routing | Repeated-block bypass, current item and title for the same route/state | Browser history, route title changes, post-skip focus and landmark naming |
| Forms, search, surveys, authentication, checkout and comments | Field labels, hint/error association and preserved input in a shared fixture | Payment and verification outcomes, data remembered across steps, comment posting and voluntary participation |
| Dynamic content, notifications, load more and chat | Raw live-region mutations and focus for the same update | Actual announcements, repeat suppression, urgency, persistence and session recovery |
| Focus, layout and typography | Defined contrast pairs, focus assertions and viewport fixtures | Visible focus, focus not obscured, enlarged text, reflow and text spacing are separate assertions |
| Alternative text, cards, icons and data graphics | Presence, roles and names for the same rendered assets | Equivalent purpose, data accuracy, alternate reading routes and independent control access |

## Coverage gaps

The pinned QA A/AA register contains no relationship for the criteria below. Keep dedicated checks in the local plan and preserve their primary source. Add a canonical relationship only after the QA repository owner reviews the proposal.

| Cosmos scope | Gap and recommendation |
| --- | --- |
| [Tooltips](../Tooltips.md) | [1.4.13 Content on Hover or Focus](https://www.w3.org/TR/WCAG22/#content-on-hover-or-focus). Keep hover, focus, dismissal and persistence assertions on the owning guide. |
| [Drag and Drop](../Drag%20and%20Drop.md) | [2.5.7 Dragging Movements](https://www.w3.org/TR/WCAG22/#dragging-movements). Test the required single-pointer alternative separately from keyboard operation. |
| [Authentication](../Authentication.md) | [3.3.8 Accessible Authentication (Minimum)](https://www.w3.org/TR/WCAG22/#accessible-authentication-minimum). Exercise every in-scope verification and recovery step with its applicable assistance, alternative or exception. |
| [Baskets and Checkout](../Baskets%20and%20Checkout.md) | [3.3.7 Redundant Entry](https://www.w3.org/TR/WCAG22/#redundant-entry). Test previously entered data across the process and record applicable exceptions. Check the full 3.3.4 scope separately when relevant. |
| [Focus](../Focus.md) and patterns with overlays or sticky content | [2.4.11 Focus Not Obscured (Minimum)](https://www.w3.org/TR/WCAG22/#focus-not-obscured-minimum). Test focused controls around drawers, dialogs, sticky headers and other author-created content through the real task. |

Generic QA checks do not cover complete service journeys. Prioritise fixtures for checkout and reauthentication, losing and resuming a draft, chatbot escalation, feedback and notification review, and refusal and cancellation. Record the end-to-end outcome and recovery path separately from component results. For complex controls, prioritise calendar text entry versus grid interaction, range slider steps, and input alternatives for moving or reordering. A QA link is evidence of source coverage for the stated assertion and variant only.
