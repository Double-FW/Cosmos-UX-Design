---
title: "Customer Surveys and Feedback"
description: "Help every customer give feedback, including people who cannot start or finish the service."
---

# Customer Surveys and Feedback

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Give customers a way to tell you what worked, where they got stuck and what would help. A survey that appears only after success misses people who could not complete the task. Provide accessible routes for people who could not start, left partway through or used assisted or offline channels. Taking part is optional and leaves access to the service unchanged.

| Customer need | Pattern |
| --- | --- |
| Give feedback whenever a problem occurs | Keep a ‘Give feedback’ link in a consistent location. Make the route available before sign-in where barriers can occur there. |
| Rate a specific experience | Offer a short, optional question after a meaningful point in the task, including relevant failure and exit points. |
| Describe a barrier or suggest a change | Provide an open comment field and a separate route to report an accessibility problem if the service offers one. |
| Get help, make a complaint or request a reply | Provide a labelled route that explains what happens next. See [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md). |

## Recommended markup

Use an ordinary link for a separate feedback page. Give the form a descriptive heading. Use native radio buttons with a `fieldset` and `legend` for a single rating, checkboxes for a genuine multiple choice and a labelled textarea for comments. Use visible words to explain each rating. If a rating uses stars, follow [Rating Stars](Rating%20Stars.md) and keep its scale and meaning available in text.

Make optional questions identifiable. Where a question allows ‘Not applicable’ or ‘Prefer not to say’, use wording that reflects the intended meaning and treat those responses separately from a rating. Associate hints and errors with their fields. Use the [form grouping guidance](HTML%20Form%20Elements.md#grouping-related-form-elements) and [Forms](Forms.md#error-messages) for validation and recovery.

## Recommended layout

Keep the invitation easy to find without covering task content or competing with its main action. Place a persistent link in a consistent location, such as near the footer. At an endpoint, put the person’s result, reference and next steps before the invitation.

Put the question before its options. Let long labels wrap and keep focus visible. Prefer stacked choices when space is limited. Break up longer surveys by topic and make it clear how far someone has progressed where that helps them decide whether to continue. Keep any ‘Skip’ or ‘No thanks’ action easy to find.

## Recommended behaviour

### Invite without blocking the task

Explain why you are asking, how you will use answers and how much time the survey is likely to take. Show the invitation at a useful point. Keep it out of the way while someone is correcting an error or reading urgent information. Respect dismissal and avoid repeated prompts in the same journey. Let people continue or leave without answering. See [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md).

Provide a usable alternative when the feedback tool is unavailable or inaccessible to someone. Test the full path, including any third-party survey and the return route. Match questions across channels where you intend to compare responses. Include people who use assisted or offline channels, and record which routes and journey outcomes the invitations reached. Treat response counts and scores as evidence from respondents. Record missing coverage, skipped answers and possible self-selection when interpreting results.

### Ask only for what you need

Ask about one thing at a time. For example, ask separately whether someone completed the task and how easy it was to use the service. Use a balanced scale, keep its order stable and leave answers unselected. Keep comments or contact details optional unless the stated purpose needs them.

Explain who sees the feedback and whether it can be linked to an account or journey. Ask separately if the person wants to be contacted. Give them a supported way to report a barrier without disclosing a diagnosis. Avoid asking for account details or other sensitive information in a general comments box.

### Confirm receipt and support recovery

Submit only on an explicit action. Keep answers available while a request is pending and after a failure where it is safe to retain them. Provide a way to retry without creating duplicates. Confirm receipt after the service accepts the response. Say whether a reply will follow and give a route back to the service. For an in-place response, use [persistent inline feedback](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback) and keep focus usable.

## Reference implementation

### Give feedback about a task

This vanilla HTML and JavaScript fixture runs locally. Replace the example destination links with your service routes. The fixture simulates a save in memory and clears the response on reload. It sends nothing over the network. Change `data-demo-result` to `failure` to exercise recovery. In a service, post the form to a working server endpoint so submission also works without scripts. Add server-side validation and show confirmation only after acceptance.

```html
<!doctype html>
<html lang="en-GB">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Give feedback</title>
<main>
  <h1>Give feedback</h1>
  <p>Tell us how the service worked for you. This is optional and takes about a minute.</p>
  <p><a href="/support">Get help with your task</a></p>

  <form id="customer-feedback" data-demo-result="success">
    <fieldset>
      <legend>How easy was it to use this service? (optional)</legend>
      <p><label><input type="radio" name="ease" value="very-difficult"> Very difficult</label></p>
      <p><label><input type="radio" name="ease" value="difficult"> Difficult</label></p>
      <p><label><input type="radio" name="ease" value="neither"> Neither easy nor difficult</label></p>
      <p><label><input type="radio" name="ease" value="easy"> Easy</label></p>
      <p><label><input type="radio" name="ease" value="very-easy"> Very easy</label></p>
      <p><label><input type="radio" name="ease" value="not-applicable"> Not applicable</label></p>
    </fieldset>

    <p><label for="feedback-comment">What could we improve? (optional)</label></p>
    <p id="comment-hint">Leave out passwords and account details.</p>
    <textarea name="comment" id="feedback-comment" rows="4"
      aria-describedby="comment-hint"></textarea>

    <p><button type="submit" hidden>Submit feedback</button></p>
    <p id="feedback-result" role="status"></p>
  </form>
  <noscript><p>This local example needs JavaScript to simulate submission.</p></noscript>
  <p><a href="/service">Return to the service</a></p>
</main>
<script>
  const form = document.getElementById('customer-feedback');
  const result = document.getElementById('feedback-result');
  const submit = form.querySelector('button[type="submit"]');
  let received = null;

  form.addEventListener('submit', event => {
    event.preventDefault();
    if (received) return;

    const data = new FormData(form);
    const ease = data.get('ease');
    const comment = data.get('comment').trim();

    if (!ease && !comment) {
      result.textContent = 'Choose a rating or enter a comment to send feedback.';
      return;
    }
    if (form.dataset.demoResult === 'failure') {
      result.textContent = 'Feedback could not be sent. Your answers are still here. Try again.';
      return;
    }

    received = { ease, comment };
    submit.setAttribute('aria-disabled', 'true');
    result.textContent = 'Thank you. This example recorded your feedback for this visit only.';
  });

  submit.hidden = false;
</script>
```

A customer may answer either question or use the return link to skip the survey. The simulated submit control stays focusable after receipt so the page does not drop focus. The handler prevents a second submission. For a multi-page survey that needs draft recovery, follow [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md).

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="testing-notes"></span>

Use representative routes, including at least one where the person cannot finish. Record the task, channel, browser, assistive technology and observed result. Use fictitious responses when testing.

| Method and action | Expected result |
| --- | --- |
| Find feedback after success, from a failure or exit point and before sign-in where relevant. Decline an invitation and continue. | Feedback is reachable without completing the service. Declining leaves the task and its outcomes available. |
| Use a screen reader and keyboard to find the form, choose a rating and submit. Repeat with a comment only and then by touch. | The question, scale, selected value and optional status are clear. Either answer can be sent and the result is available in text. |
| Submit without an answer. Then enter a response, simulate failure, correct or retry, and attempt a second submission after success. | Instructions explain what to do. Failure preserves answers and exposes a retry. Success follows acceptance and repetition does not duplicate it. |
| Increase text size to 200%, then separately test reflow at a narrow width. Repeat in forced colours. | Questions, labels, controls, visible focus and feedback remain usable. |
| Try the real service with scripts blocked or the external survey unavailable. Follow the help or complaints route. | An accessible feedback route remains available. The service explains which route provides help or a reply. |
| Review invitations and responses from a sample across outcome and channel. | Reports show who was invited and whose experience is missing where that is known. Skips and non-applicable responses stay distinct from ratings. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](qa/w3c-act/e086e5.md)
- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)

## Sources

- [Double-FW Cognitive UX Design: Cognitive accessibility user research and data plan](https://github.com/Double-FW/Cognitive-UX-Design/blob/main/Cognitive_Accessibility_User_Research_and_Data_Plan.md)
- [GOV.UK Design System: Feedback component (trial)](https://design-system.service.gov.uk/components/feedback/)
- [GOV.UK Service Manual: Measuring user satisfaction](https://www.gov.uk/service-manual/measuring-success/measuring-user-satisfaction)
- [W3C WAI Forms Tutorial: Grouping controls](https://www.w3.org/WAI/tutorials/forms/grouping/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
