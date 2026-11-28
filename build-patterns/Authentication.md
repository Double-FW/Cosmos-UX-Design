# Build Authentication

## A. Standalone build prompt

Prove access, recover when a method fails and return to the authorised task. Agree enrolment, sign-in, additional verification, session expiry, recovery and sign-out with the identity owner. Confirm approved methods, assurance, devices, third-party returns, retained work, supported password managers and no-script route. Use fictitious accounts in tests.

The example suggests an email address and sample routes. The host owns its identifier and endpoints. Use username, current-password, new-password and one-time-code tokens for their respective purposes. Use numeric inputmode only for digits-only text codes. WCAG 2.2 SC 3.3.8 applies at each authentication step. Document the applicable alternative, assistance mechanism or specified exception. A passkey is a recommendation conditional on assurance, platform and recovery.

Use native HTML where it supplies the behaviour. Use host components and brand tokens where they preserve these outcomes. Before coding, copy the [Cosmos semantics contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) for each materially different variant. Record names, roles, states, relationships, focus, keyboard commands, feedback, fallback, recovery and the supported environments. Mark unresolved host decisions. Deliver the implementation, the contract reconciled against the rendered interface, and evidence against the [Authentication matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L38). Accept each applicable scenario only with observed evidence. Record pass, fail, not applicable or not run and explain exclusions and gaps.

```gherkin
Feature: Authentication

  Scenario: AUTH-P2-01 — The person uses a password manager or pastes credentials and submits
    Given the sign-in form offers a labelled identifier and initially masked password
    When the person uses a password manager or pastes credentials and submits
    Then assisted entry works and an approved sign-in proceeds without forced recall or transcription

  Scenario: AUTH-P2-02 — The person activates Show password then Hide password by keyboard
    Given a visibility button is available beside a filled masked password
    When the person activates Show password then Hide password by keyboard
    Then masking and the next-action name change together, the value survives and focus stays on the button

  Scenario: AUTH-P2-03 — The person pastes a complete code beginning with zero
    Given a single labelled text field requests a one-time code
    When the person pastes a complete code beginning with zero
    Then the entire code including the zero remains available for submission

  Scenario: AUTH-P2-04 — The person submits it and requests a replacement
    Given a code sent to the indicated destination has expired
    When the person submits it and requests a replacement
    Then feedback explains expiry and the reachable resend route lets them continue without repeating unrelated details

  Scenario: AUTH-P2-05 — The person submits the form
    Given a required field is empty or the identity service declines sign-in
    When the person submits the form
    Then a field error identifies what to correct, while a service refusal gives an actionable form-level message without revealing account state

  Scenario: AUTH-P2-06 — They request recovery for a known and an unknown identifier in separate test runs
    Given the customer cannot use their normal authenticator
    When they request recovery for a known and an unknown identifier in separate test runs
    Then both receive consistent public responses and an eligible customer can complete the approved recovery route without sharing secrets with support

  Scenario: AUTH-P2-07 — The person cancels the platform prompt or lacks their usual device
    Given the service offers passkey enrolment or sign-in
    When the person cancels the platform prompt or lacks their usual device
    Then they return to an identifiable step with an approved usable alternative

  Scenario: AUTH-P2-08 — The provider returns an error or the person cancels
    Given sign-in delegates verification to another provider
    When the provider returns an error or the person cancels
    Then the service identifies the current step and offers a reachable retry or alternative

  Scenario: AUTH-P2-09 — They encounter the warning and then reauthenticate after expiry
    Given the person has eligible unfinished work in an expiring session
    When they encounter the warning and then reauthenticate after expiry
    Then they understand the available time and retained work and return to the authorised task with eligible progress and no credentials saved in the draft

  Scenario: AUTH-P2-10 — They follow the baseline form or offered accessible sign-in route
    Given the person needs sign-in with scripts blocked
    When they follow the baseline form or offered accessible sign-in route
    Then they can authenticate and return to the intended task

  Scenario: AUTH-P2-11 — A person enrols using a supported method and later signs out
    Given the service offers enrolment and sign-out
    When a person enrols using a supported method and later signs out
    Then they can identify completion at each step and protected content requires fresh authorisation after sign-out

  Scenario: AUTH-P2-12 — The person enlarges text, narrows the viewport and enables forced colours in separate runs
    Given a sign-in error appears beside the form
    When the person enlarges text, narrows the viewport and enables forced colours in separate runs
    Then the error, next step and focused control remain readable and reachable
```

Scenario IDs map to the following checks in scenario order. AUTH-P2-01 → CTL-118, CTL-134 · AUTH-P2-02 → Authentication check 1 · AUTH-P2-03 → CTL-116, Authentication check 2 · AUTH-P2-04 → Authentication check 2 · AUTH-P2-05 → PRACTICE-FORM-02 · AUTH-P2-06 → Authentication check 4 · AUTH-P2-07 → Authentication check 3 · AUTH-P2-08 → Authentication check 4 · AUTH-P2-09 → CTL-197, CTL-198, CTL-199 · AUTH-P2-10 → Authentication check 6 · AUTH-P2-11 → PRACTICE-MULTI-01 · AUTH-P2-12 → Authentication check 6.

## B. Retrieval pack

| Retrieve | Question or decision |
| --- | --- |
| [Owning guide, recommended behaviour](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#recommended-behaviour) | Which focus, feedback and recovery decisions apply to this variant? |
| [Original implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#reference-implementation) | Which details belong to the illustrative markup? The example suggests an email address and sample routes. The host owns its identifier and endpoints. Use username, current-password, new-password and one-time-code tokens for their respective purposes. Use numeric inputmode only for digits-only text codes. WCAG 2.2 SC 3.3.8 applies at each authentication step. Document the applicable alternative, assistance mechanism or specified exception. A passkey is a recommendation conditional on assurance, platform and recovery. |
| [Contract template](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Component%20Semantics%20Contract.md#contract-template) | Which states, transitions and evidence fields must travel with the implementation? |
| [Pinned matrix row](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/testing-matrix/Design%20Patterns.md#L38) | Which canonical IDs apply and which outcomes require the owning guide? |
| [HTML Form Elements](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/HTML%20Form%20Elements.md#input-purpose-and-autofill) | Select the correct credential token. |
| [Forms](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Forms.md#error-messages) | Route a correctable field error to its field. |
| [Saving and Resuming a Task](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Saving%20and%20Resuming%20a%20Task.md#return-and-reauthenticate) | Decide which task data may survive expiry. |
| [PRACTICE-FORM-02](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L603) | Assess correction after invalid submission. |
| [PRACTICE-MULTI-01](https://github.com/Double-FW/QA_Accessibility_Testing/blob/9cddf3a1ed3def130ff78bcc4592d5cc49b0c2ca/Accessibility_ACT-Style_Test_System.md#L687) | Compare task completion across input modes. |
| [Form field has non-empty accessible name (e086e5)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/e086e5.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Autocomplete attribute has valid value (73f2c2)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/73f2c2.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [Button has non-empty accessible name (97a4e1)](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/qa/w3c-act/97a4e1.md) | Formal W3C ACT rule, proposed in the baseline. Apply its applicability and expectation only to matching elements. |
| [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill) | Checked 28 November 2026. Autofill field names and their purposes. A valid token alone cannot demonstrate password-manager interoperability. |
| [WCAG 2.2 SC 3.3.8](https://www.w3.org/TR/WCAG22/#accessible-authentication-minimum) | Assess every authentication step including a delegated step against the criterion and exceptions. |

## C. Verification plan

Run each row from its Given state using its When action. Compare with the complete Then outcome. Record method and executor separately. Each row starts as **not run** for the host.

| Scenario | Existing coverage | Applicability and method | Expected result | Evidence |
| --- | --- | --- | --- | --- |
| AUTH-P2-01 | CTL-118, CTL-134 | Human judgement. Run when the sign-in form offers a labelled identifier and initially masked password. | Assisted entry works and an approved sign-in proceeds without forced recall or transcription. | Participant or tester observation, task result and supported environment. |
| AUTH-P2-02 | Authentication check 1 | Automated. Run when a visibility button is available beside a filled masked password. | Masking and the next-action name change together, the value survives and focus stays on the button. | Browser assertion trace with before and after state. |
| AUTH-P2-03 | CTL-116, Authentication check 2 | Automated. Run when a single labelled text field requests a one-time code. | The entire code including the zero remains available for submission. | Browser assertion trace with before and after state. |
| AUTH-P2-04 | Authentication check 2 | Agent-assisted. Run when a code sent to the indicated destination has expired. | Feedback explains expiry and the reachable resend route lets them continue without repeating unrelated details. | Redacted step trace, environment and service-owner review. |
| AUTH-P2-05 | PRACTICE-FORM-02 | Automated. Run when a required field is empty or the identity service declines sign-in. | A field error identifies what to correct, while a service refusal gives an actionable form-level message without revealing account state. | Browser assertion trace with before and after state. |
| AUTH-P2-06 | Authentication check 4 | Agent-assisted. Run when the customer cannot use their normal authenticator. | Both receive consistent public responses and an eligible customer can complete the approved recovery route without sharing secrets with support. | Redacted step trace, environment and service-owner review. |
| AUTH-P2-07 | Authentication check 3 | Human judgement. Run when the service offers passkey enrolment or sign-in. | They return to an identifiable step with an approved usable alternative. | Participant or tester observation, task result and supported environment. |
| AUTH-P2-08 | Authentication check 4 | Agent-assisted. Run when sign-in delegates verification to another provider. | The service identifies the current step and offers a reachable retry or alternative. | Redacted step trace, environment and service-owner review. |
| AUTH-P2-09 | CTL-197, CTL-198, CTL-199 | Human judgement. Run when the person has eligible unfinished work in an expiring session. | They understand the available time and retained work and return to the authorised task with eligible progress and no credentials saved in the draft. | Participant or tester observation, task result and supported environment. |
| AUTH-P2-10 | Authentication check 6 | Agent-assisted. Run when the person needs sign-in with scripts blocked. | They can authenticate and return to the intended task. | Redacted step trace, environment and service-owner review. |
| AUTH-P2-11 | PRACTICE-MULTI-01 | Agent-assisted. Run when the service offers enrolment and sign-out. | They can identify completion at each step and protected content requires fresh authorisation after sign-out. | Redacted step trace, environment and service-owner review. |
| AUTH-P2-12 | Authentication check 6 | Human judgement. Run when a sign-in error appears beside the form. | The error, next step and focused control remain readable and reachable. | Participant or tester observation, task result and supported environment. |

Use the [manual procedures](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#testing-approach-and-act-scripts) to assess real assisted entry, platform cancellation, lost-device recovery and return to the task. The matrix has no dedicated end-to-end test for authenticator enrolment and sign-out. These are Cosmos or supplied checks. The proposed W3C ACT rules above cover narrower element-level expectations.

## D. Definition of done

Accept a variant when the rendered contract matches every applicable transition, the host tests pass and the human evaluations record successful task completion. Record build, input, browser, device, assistive technology, observed outcome and evidence per scenario. Give a reason for every not applicable or not run result. A fail needs correction and retest. An applicable not run result leaves acceptance open.

Use approved test accounts for provider returns, recovery, session expiry and server-side sign-out. Retain redacted evidence. Ask people to complete recovery without their usual device and explain what happened to their work.
