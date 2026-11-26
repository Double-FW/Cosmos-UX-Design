---
title: Designing the Screen Reader Journey
description: Design and review how people using screen readers find, complete and recover a task.
---

# Designing the Screen Reader Journey

Review a whole task from entry to completion and recovery. Someone may use a heading list, jump to a control or return after an interruption. Give them enough context to understand the current step, compare choices and correct a problem without reconstructing earlier screens.

## Scope

Use this guide to annotate designs, review a semantic prototype and test a journey with a screen reader. Apply the owning Cosmos guides for the components and behaviours involved.

## Guidance

### Make the task easy to find and revisit

- Name the view and important sections. Use [Headings](Headings.md) to give the task an outline and [Landmarks](Landmarks.md) where they help people find a major page area.
- Keep instructions, choices and consequences near the decision they support. Give repeated actions enough context to make sense when someone reaches them through a list of links or controls. See [Labels](Labels.md).
- Make the current choice and its important conditions easy to retrieve. For example, keep the selected delivery method, cost and timing together in a review summary. Let people revisit or change it at their own pace.

### Specify reading order, focus and feedback together

Document these separately for each consequential state. Reading order describes how content is encountered when someone reads through the view. Keyboard focus identifies the control or element that receives keyboard input. A screen reader’s reading cursor can be elsewhere. See [Layout](Layout.md#visual-order-versus-source-order) and [Focus](Focus.md#tab-order).

For each transition, record what the person did, what changes, where focus goes and what is announced. Keep focus in place for routine in-page updates. For a new route, follow [Client-side Routing](Client-side%20Routing.md#first-load-and-route-changes). When a meaningful update could be missed, choose the appropriate [status message](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback). Avoid repeating the same message through both a focus change and a live region when either one communicates it.

## Implementation

In the design record, write a short example of how the task might be encountered through speech. Pair it with the actual heading, accessible name, role, state, value or description that conveys the meaning. Mark expected focus, announcements and what remains available to reread. Use the script to review the sequence and context. Let the screen reader determine the wording and order of its output, and check braille separately where supported.

### Example: choose and review a delivery method

The person needs to compare delivery options, check the method they chose and choose again if it becomes unavailable.

| State and action | What the design specifies |
| --- | --- |
| Open the delivery step from the basket. | The view is titled ‘Choose a delivery method’. Use a labelled group of native radio buttons. Name each option and associate its price and timing with it. Use [HTML Form Elements](HTML%20Form%20Elements.md#grouping-related-form-elements) for grouping and descriptions. |
| Select home delivery. | The option’s checked state reflects the choice. Focus stays in the group. The cost and timing remain available to reread. Selection alone leaves the person at this step. |
| Continue to review. | The new view introduces the ‘Review your order’ heading. A delivery summary names the selected method, cost and timing. A ‘Change delivery method’ action returns to the choice with the selection still available. |
| Try to continue after the selected method becomes unavailable. | A persistent message identifies the unavailable method and offers a way to choose again. On a form submission that returns an error summary, use the [Forms error pattern](Forms.md#error-messages). Keep other valid answers and explain what needs to change. |
| Return after a pause. | Show the current step and the saved choice if still valid. Explain any outstanding action. Follow [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md). |

For an example speech review, write ‘Home delivery. £4. Within three working days.’ Then verify that the control exposes the option name, radio role and checked state, and that the price and timing can be found from that option. The phrasing is illustrative. Use actual output in the test record.

## Checkpoints for the tester

Use a representative implementation with a successful path, an error and a return path. Record the browser, device, screen reader, relevant settings and commands used. Run the supported combinations and include touch exploration and braille output where relevant.

| Method and action | Expected result |
| --- | --- |
| Inspect the DOM and accessibility tree. Read the screen in order, then use the screen reader’s headings and landmarks navigation. | Reading order preserves relationships. People can find the task and important sections from their labels and move into the relevant content. |
| On a fresh run, jump into the task using the controls or links list. Compare the delivery options and make a choice with the keyboard or supported touch gestures. | Names identify each action or choice in context. The person can find its terms and verify the selected state. |
| Start a routine background update while reading or editing. Then trigger an update that needs action. | Routine updates leave focus and reading usable. The meaningful change is available to review without an unexpected focus move or duplicate announcement. |
| Submit with an unavailable choice, correct it, go back and resume after a pause. Check keyboard focus and screen reader navigation at each transition. | The person can locate the issue, recover valid work and find the next action. Focus lands at a usable location and the reading sequence supports recovery. |
| Ask people using their own supported access methods to complete the task, explain their choice and recover from the error. | Record what people found, understood and completed, including where they needed assistance. Agree the outcome criteria before the evaluation. |

Use inspection to check structure and state. Use assistive technology to observe navigation and output. Use task research to understand whether people can make the decision and recover. Record expected and observed results separately.

## Sources

- [Vision, memory and cognitive screen-reader UX: script review](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Vision_Memory_and_Cognitive_Screen_Reader_UX.md#7-design-the-logic-as-an-audio-script)
- [Vision, memory and cognitive screen-reader UX: review and test](https://github.com/Double-FW/Cognitive-UX-Design/blob/523df2fe9e47f8dd75d30b7920a9179d57a89bba/Vision_Memory_and_Cognitive_Screen_Reader_UX.md#11-review-and-test-the-cognitive-screen-reader-experience)
