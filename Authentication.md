---
title: "Authentication"
description: "Help people sign in, verify their identity and recover access without avoidable barriers."
---

# Authentication

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Build this pattern](#build-this-pattern)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people prove they can access an account and return to the task they came to do. Design enrolment, sign-in, additional verification, recovery and sign-out as one journey. Someone may lose a device, be unable to use a particular method or need to switch devices midway. Choose methods that meet the service's security needs and give people an accessible route at each step.

WCAG 2.2 [3.3.8 Accessible Authentication (Minimum)](https://www.w3.org/TR/WCAG22/#accessible-authentication-minimum) (Level AA) covers every step of the authentication process. If a step requires a cognitive function test such as recalling a password or solving a puzzle, it must provide a method that avoids the test, a mechanism that helps the person complete it, or meet the criterion's object-recognition or user-provided non-text content exception. Password-manager support and copy and paste can provide assistance. Prefer a usable route that avoids puzzles and forced transcription.

## Recommended markup

- Use a native form with visible labels and an explicit submit button. Use `autocomplete="username"` for the sign-in identifier, `current-password` for the existing password and `new-password` for a new one. Allow password managers, autofill and paste. See [HTML Form Elements](HTML%20Form%20Elements.md#input-purpose-and-autofill).
- Mask passwords initially. If you offer visibility, use a native button labelled ‘Show password’ or ‘Hide password’ for its next action. Keep focus on the button and preserve the entered value.
- Use one labelled text input for a one-time code. Allow the whole code to be pasted and use `autocomplete="one-time-code"`. Add `inputmode="numeric"` only for digits-only codes. A code can begin with zero, so keep it as text. Make the destination, expiry and resend options clear.
- Use the [Forms](Forms.md#error-messages) pattern for missing fields and errors. Distinguish a field the person can correct from a sign-in failure that affects the whole form.

## Recommended layout

Put the method, instructions and primary action in reading order. Keep recovery and ‘Try another way’ routes near the step where someone might need them. Explain what will happen before asking someone to switch to another device, open an app or follow a third-party sign-in route. On return, show the current step and what remains to do.

Keep inputs, verification controls, errors and visible focus usable at high zoom and on small screens. Let controls wrap with enlarged text. Keep warnings available long enough to read and act on. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

## Recommended behaviour

### Choose accessible methods that meet the security need

Offer passkeys where supported and suitable for the service. Explain the device prompt in plain language and test enrolment, sign-in and recovery with the supported platform. Include a usable route when a device or authenticator is unavailable. For additional verification, offer approved alternatives where feasible and explain what each requires. Consider people who cannot use a smartphone, receive a call, scan a QR code or complete a biometric prompt.

Choose verification and recovery methods according to their actual security properties. For example, a code entered manually is susceptible to phishing. Use [NIST authentication guidance](https://pages.nist.gov/800-63-4/sp800-63b.html) to decide which authenticators and recovery methods suit the service's assurance needs. Carry out credential verification, rate limiting and session management on the server or through the approved identity provider. Protect transport and keep passwords, codes and recovery secrets out of logs and analytics.

### Explain failures and support recovery

For failed sign-in, give a useful next step without disclosing whether an account exists or is locked. For reset requests, use a consistent response such as ‘If these details match an account, we’ll send reset instructions’. Apply that consistency across the actual service response as well as the wording. For a code sent within a verification step, explain if the service knows it has expired and provide a way to request another. Describe when the next attempt is available where the service can safely do so. See [OWASP Authentication guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html).

Keep the approved recovery route accessible if a customer loses their usual authenticator. Avoid asking people to remember security-question answers as their only route. Explain what support can do and what proof is needed, without asking people to share a password or verification code with support. See [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md).

### Return to the task

If sign-in expires, explain what happened and what work is still available. Where permitted, return the person to the intended task after verifying access and preserve eligible progress. Keep credentials and one-time codes out of saved drafts. Apply the same checks to any identity provider or payment verification step. See [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md#return-and-reauthenticate) and [Baskets and Checkout](Baskets%20and%20Checkout.md).

## Build this pattern

<span id="reference-implementation"></span>
<span id="implementation"></span>

Use the [standalone authentication build brief and Gherkin outcomes](build-patterns/Authentication.md) to choose a variant, write a component semantics contract and verify the implementation. The companion includes a retrieval pack, source-check mapping and manual test links.

The [original reference implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#reference-implementation) remains available at its source revision.

<span id="sign-in-with-a-password"></span>
- [Original sign in with a password example](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#sign-in-with-a-password)
<span id="enter-a-digits-only-verification-code"></span>
- [Original enter a digits only verification code example](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Authentication.md#enter-a-digits-only-verification-code)

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="testing-notes"></span>

Use test accounts and supported browsers, devices and assistive technologies. Cover the whole journey, including third-party steps, with fictitious credentials.

| Method and action | Expected result |
| --- | --- |
| Use a keyboard and supported screen reader to sign in. Try autofill, a password manager and paste. Toggle password visibility with Enter and Space. | Labels are clear. Assisted entry works. The visible and accessible button names match the action, masking follows the control and focus stays usable. |
| Paste a complete code starting with zero. Then try an expired code and request another. | The input retains the whole code. Feedback explains what to do next, and resend is reachable without re-entering unrelated details. |
| Use a passkey if offered. Cancel the device prompt, then repeat without the usual device or method. | The person can return to sign-in and reach an approved accessible alternative or recovery route. |
| Simulate an unsuccessful sign-in, a reset request and a failed identity-provider return. | Feedback offers a next step without exposing account state. Recovery can be completed with supported input and assistive technology. |
| Expire the session during a task and sign in again. | The person understands what was retained and can return to an authorised task with eligible progress intact. |
| Enlarge text, test narrow width and forced colours, then block scripts in a separate run. | Fields, errors and focus remain usable. The baseline works or an accessible sign-in route remains available. |

Review authenticator choice, server controls and recovery separately with the service's security team. Use the accessibility checks to evaluate whether people can complete the supported journey.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](qa/w3c-act/e086e5.md)
- [Autocomplete attribute has valid value (73f2c2)](qa/w3c-act/73f2c2.md)
- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)

## Sources

- [W3C: Understanding Accessible Authentication (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum.html)
- [GOV.UK Design System: Password input](https://design-system.service.gov.uk/components/password-input/)
- [Atomic A11y: Web password input guidance](https://github.com/charlietriplett/atomica11y-skills/blob/main/atomica11y-skills/accessible-web/password-input.md)
- [NIST SP 800-63B-4: Authentication and Authenticator Management](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [3.3.8 Accessible Authentication (Minimum)](https://www.w3.org/TR/WCAG22/#accessible-authentication-minimum)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Autocomplete attribute has valid value (73f2c2)](https://www.w3.org/WAI/standards-guidelines/act/rules/73f2c2/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
