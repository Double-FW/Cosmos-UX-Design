---
title: "Baskets and Checkout"
description: "Help people review what they intend to buy, correct it and complete payment with a clear outcome."
---

# Baskets and Checkout

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people check what they are buying, what they will pay and when they will receive it. Someone may need to change quantities, compare delivery options or recover after a payment interruption. Keep their choices available and show the next action at each stage.

| Stage | What the person needs |
| --- | --- |
| Add and review | Confirmation that the intended item was added, with a way to reach the basket. |
| Edit the basket | The item, quantity, price and availability, with a way to change or remove it. |
| Choose delivery and payment | Available options, costs and timing, plus an accessible route through any third-party service. |
| Review and commit | A clear total, significant terms and a way to change details before placing the order. |
| Check the outcome | A persistent record of the confirmed order or a clear way to check an uncertain payment. |

## Recommended markup

Use a labelled list of items, with headings when they help people navigate a long basket. Use a link to a product or another step and a button to update or remove an item. Name repeated actions for their target, such as ‘Remove Travel mug’. Give quantity controls visible labels and a way to enter the quantity without repeated clicks. Use [Forms](Forms.md) for labels, validation and recovery.

Group delivery and payment choices with a `fieldset` and `legend`. Keep the price and timing with the option they describe. Use headings and a list of terms and values for the order summary. Give change links specific names, such as ‘Change delivery address’. Use appropriate autofill tokens for name, contact and address fields. Follow the [GOV.UK payment card details pattern](https://design-system.service.gov.uk/patterns/payment-card-details/) if the checkout collects card details.

## Recommended layout

Keep the order contents and total easy to find before commitment. Show item prices, discounts, delivery and other applicable charges. If a charge depends on a later selection, mark the current amount as an estimate and explain what remains to be calculated. Put the currency, final total and any recurring amount or interval near the order action. See [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md#show-what-the-choice-means).

Show the current step on a multi-step checkout and let people return to edit. Keep the route to guest checkout visible wherever the service supports it. Keep optional extras unselected and keep the discount field, account invitations and promotions from displacing the order review. At narrow widths, let the summary and controls reflow in reading order. Check that a sticky total or footer leaves the focused control visible.

## Recommended behaviour

### Update the basket

Let people add, change and remove items without moving them to an unexpected view. Show the outcome next to the action or in the basket, and announce a concise update where the person could otherwise miss it. When an item is removed, move focus to an appropriate remaining item, an undo action or the empty-basket heading if the focused control disappeared. Expose the empty state and a way to continue shopping. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

Keep the item quantities, costs and total in sync after edits. Apply a discount only when the person requests it and report whether it was accepted. Preserve existing items and valid entries when an update fails. If stock, price or delivery changes before commitment, identify what changed and let people review it again. See [Showing What Changed Before Confirmation](Showing%20What%20Changed%20Before%20Confirmation.md).

### Complete checkout

Offer guest checkout where an account is unnecessary. Explain why sign-in is needed where it is required. Keep valid information across steps and avoid asking for it again. WCAG 2.2 [3.3.7 Redundant Entry](https://www.w3.org/TR/WCAG22/#redundant-entry) (Level A) requires previously supplied information needed again in the same process to be populated or available to select, with exceptions for essential re-entry, security or information that is no longer valid. Let people edit the reused information where the task permits.

If payment opens another service, explain the transition and keep the basket recoverable on return. Include external payment and authentication steps in the supported journey. Let people review and correct the order before committing. This is one way to meet WCAG 2.2 [3.3.4 Error Prevention (Legal, Financial, Data)](https://www.w3.org/TR/WCAG22/#error-prevention-legal-financial-data) (Level AA), which also permits reversal or checking for errors with an opportunity to correct them.

### Confirm or recover

Explain the actual result. Give a confirmed order a reference, a summary and the next step. Distinguish payment that failed from a payment whose outcome is still unknown. If the answer is unknown, let people check the existing order before asking them to pay again. The service should recognise retries and prevent duplicate orders or charges. Preserve the basket and valid checkout details when people go back or return after interruption, subject to the service’s retention rules. See [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md).

## Reference implementation

### Edit a basket item

This vanilla HTML fragment shows a server-rendered basket with one item. Replace the sample routes and item identifier with service values, and include the service’s request verification fields. The server validates quantity and availability, recalculates the price and returns the updated basket or an error with the entered values preserved. Use separate submit actions for update and removal. The form and review link work without JavaScript once connected to those routes.

```html
<h1>Your basket</h1>

<section aria-labelledby="mug-heading">
  <h2 id="mug-heading">Travel mug</h2>
  <p>£12 each</p>
  <form method="post" action="/basket/items/travel-mug">
    <label for="mug-quantity">Quantity of Travel mug</label>
    <input id="mug-quantity" name="quantity" type="number"
      min="1" step="1" value="2" required>
    <button type="submit" name="action" value="update">
      Update Travel mug quantity
    </button>
    <button type="submit" name="action" value="remove" formnovalidate>
      Remove Travel mug
    </button>
  </form>
</section>

<h2>Basket summary</h2>
<dl>
  <dt>Items subtotal</dt><dd>£24</dd>
  <dt>Delivery</dt><dd>Calculated when you choose a delivery option</dd>
</dl>
<p><a href="/checkout/delivery">Continue to delivery options</a></p>
```

### Review the order before payment

This fragment shows the next relevant step after delivery has been chosen. Render the total and commitment text from the same server-side order record. Replace the example routes and review identifier. Add the service’s request verification fields. The payment handler verifies that the reviewed order still matches the current order and prevents duplicate commitments. Only render the final button at the step where it actually places the order.

```html
<h1>Review your order</h1>
<p>2 × Travel mug</p>

<dl>
  <dt>Items</dt><dd>£24</dd>
  <dt>Standard delivery</dt><dd>£4, within three to five working days</dd>
  <dt>Total including delivery</dt><dd>£28</dd>
</dl>

<p><a href="/basket">Change basket items</a></p>
<p><a href="/checkout/delivery">Change delivery option</a></p>

<form method="post" action="/checkout/place-order">
  <input type="hidden" name="review" value="example-review-id">
  <button type="submit">Place order and pay £28</button>
</form>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="testing-notes"></span>

Use a test payment environment and fictitious order data. Include the host checkout and any payment or authentication service. Record the route, starting state, device and assistive technology.

| Method and action | Expected result |
| --- | --- |
| Add two items. Use keyboard and a supported screen reader to identify and edit each quantity. Remove the focused item, then remove the last item. | Each action identifies its item. Totals reflect accepted changes. Focus stays usable and the empty basket gives a next step. |
| Try a discount that fails. Then change delivery or make an item unavailable between review and payment. | The basket stays intact after the discount error. Material changes are explained and the person can review the current order before committing. |
| Complete guest checkout where offered. Use Back and the review page’s change links to edit an address or delivery choice. | The guest route is findable. Valid details remain available and the person can return to review without re-entering them unnecessarily. |
| Complete payment using keyboard and a supported screen reader, including any external step. Then simulate a decline, timeout or uncertain result and retry through the offered route. | Payment outcomes are distinguishable. The result provides an appropriate next action and a retry creates no duplicate order or charge. |
| Enlarge text, then test narrow width, forced colours and blocked scripts in separate runs. | Item actions, summary and focus remain visible. The baseline basket and checkout routes work, or the service offers a working alternative. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Autocomplete attribute has valid value (73f2c2)](https://www.w3.org/WAI/standards-guidelines/act/rules/73f2c2/proposed/)

## Sources

- [Double-FW: Accessibility Across the E-Commerce Customer Lifecycle](https://github.com/Double-FW/Inclusive_and_Effective_Design/blob/main/E-Commerce%20Customer%20Lifecycle.md)
- [GOV.UK Design System: Check answers](https://design-system.service.gov.uk/patterns/check-answers/)
- [GOV.UK Design System: Payment card details](https://design-system.service.gov.uk/patterns/payment-card-details/)
- [W3C: Understanding Error Prevention (Legal, Financial, Data)](https://www.w3.org/WAI/WCAG22/Understanding/error-prevention-legal-financial-data.html)
- [W3C: Understanding Redundant Entry](https://www.w3.org/WAI/WCAG22/Understanding/redundant-entry.html)
- [3.3.7 Redundant Entry](https://www.w3.org/TR/WCAG22/#redundant-entry)
- [3.3.4 Error Prevention (Legal, Financial, Data)](https://www.w3.org/TR/WCAG22/#error-prevention-legal-financial-data)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Autocomplete attribute has valid value (73f2c2)](https://www.w3.org/WAI/standards-guidelines/act/rules/73f2c2/proposed/)
