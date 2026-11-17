# Test Cases — Silver Plan Checkout (Happy Path)

| Field | Value |
| --- | --- |
| **Application** | mySeniorCareHub (Staging) |
| **Module** | Medical Alert Products → Silver Plan → Cart → Checkout → Payment |
| **Environment** | Staging |
| **Base URL** | https://staging.myseniorcarehub.com/medical-alert-products |
| **Cart URL** | https://staging.myseniorcarehub.com/ecommerce/cart |
| **Priority** | High |
| **Type** | Functional / E2E (Happy Path) |
| **Payment Gateway** | Stripe |
| **Document Version** | 1.0 |
| **Date** | 2026-11-17 |

---

## 1. Scope

End-to-end happy path for:

**Products page → Silver Plan → Add to Cart → (default Monthly) → Apply veteran coupon (`VETFREE`) → Create new account → Stripe payment → Order ID confirmation**

Out of scope for this document: negative / edge cases (invalid card, duplicate email, empty required fields, Gold plan, device SKUs).

---

## 2. Preconditions

1. Tester is on staging environment.
2. Browser session is **logged out** (guest / new user).
3. Cart is empty (or cleared before start).
4. Tester can generate a **unique** email for each run (no special email format rules beyond uniqueness).
5. No OTP / email verification is required during account creation.
6. Stripe test card is usable on staging.

---

## 3. Test Data

| # | Field | Value / Rule |
| --- | --- | --- |
| TD-01 | Product | Family Safety, Wellness & Sharing App Membership Pass |
| TD-02 | Plan | Silver Plan |
| TD-03 | Renewal subscription | **Monthly Plan - $9.99** (keep default; do not change) |
| TD-04 | Quantity | 1 (default) |
| TD-05 | Feature list | Keep **default** Silver feature list (do not change) |
| TD-06 | Promo / veteran coupon | `VETFREE` |
| TD-07 | Account | **New account** every run |
| TD-08 | Email | Unique per run (e.g. `qa.silver.<timestamp>@example.com`) — no extra format validation beyond uniqueness |
| TD-09 | First Name | *Provide valid test value* |
| TD-10 | Last Name | *Provide valid test value* |
| TD-11 | Phone Number | *Provide valid US test phone* |
| TD-12 | User Name | Unique (may match email local-part or separate unique value) |
| TD-13 | Password / Confirm Password | *Provide valid password meeting app rules* |
| TD-14 | Shipping / Service Location | Valid **US** address (Street, City, State, Zip) — *provide test address* |
| TD-15 | Apt / Ste | Optional |
| TD-16 | Billing address | Same as shipping (recommended for happy path) unless otherwise specified |
| TD-17 | Name on Card | *Provide test cardholder name* |
| TD-18 | Card Number | `4242424242424242` |
| TD-19 | Expiry | `01/27` |
| TD-20 | CVC | `900` |
| TD-21 | OTP | Not required |

> **Note:** Cart may show **Silver Membership Pass - 1 Month Free** at **$0.00** due now, with renewal **Monthly Plan - $9.99** after free trial. Assert against UI values shown at runtime.

---

## 4. Default Silver Feature List (assert as displayed — do not modify)

Keep and verify the default Silver plan feature set shown on the products / membership card, for example:

- Safety — SOS alerts and emergency notifications  
- Location Tracking — Real-time GPS tracking and safe zone alerts  
- Family Sharing — Keep loved one connected and informed  
- Medication Monitoring — Never miss a dose with reminders  
- Wellness Tracking — Track activity, sleep and overall well-being  
- Kiara AI Assistant  

Also visible in flow context: includes family member passes (e.g. **Includes 4 Family Member Passes** / free trial messaging as shown on UI).

---

## 5. Test Cases

### TC-SP-001 — Navigate to Medical Alert Products page

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Smoke |

**Steps**

1. Open browser.
2. Navigate to `https://staging.myseniorcarehub.com/medical-alert-products`.
3. Dismiss any newsletter / overlay if present (so page content is usable).

**Expected Result**

- Page loads successfully.
- Title / heading related to Medical Alert Devices / Membership Plans is visible.
- Membership card **Family Safety, Wellness & Sharing App Membership Pass** is displayed.
- **Gold Plan** and **Silver Plan** controls are visible.
- **Add to Cart** is visible on the membership card.

---

### TC-SP-002 — Select Silver Plan and keep default features

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. On the products page, locate **Family Safety, Wellness & Sharing App Membership Pass**.
2. Click **Silver Plan**.
3. Do **not** change the feature list (keep defaults).

**Expected Result**

- Silver Plan becomes the selected plan (selected state).
- Default Silver feature list remains displayed (see Section 4).
- Membership card still shows **Add to Cart**.
- Price / trial messaging for Silver is displayed as per staging UI (e.g. free trial / monthly messaging).

---

### TC-SP-003 — Add Silver Plan to cart

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. With **Silver Plan** selected, click **Add to Cart** on the membership card.
2. Open the cart (cart icon / cart badge / navigate to `https://staging.myseniorcarehub.com/ecommerce/cart`).

**Expected Result**

- Item is added successfully.
- Cart count increments (e.g. shows `1`).
- Shopping Cart page shows:
  - Product: **Family Safety, Wellness & Sharing App Membership Pass**
  - Silver membership line item (e.g. Silver Membership Pass / 1 Month Free messaging as shown)
  - Pass count messaging (e.g. Include 4 Member Passes) as shown
- Renewal subscription dropdown defaults to **Monthly Plan - $9.99** (do not change).

---

### TC-SP-004 — Verify default Monthly renewal option on cart

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. On Shopping Cart, locate renewal subscription plan control.
2. Confirm selected value without changing it.
3. Confirm available options are visible if opened (optional check only; leave Monthly selected).

**Expected Result**

- Default selected option is **Monthly Plan - $9.99**.
- Other options may include Quarterly / Yearly (do **not** select them for this happy path).
- Order summary reflects Monthly renewal (e.g. due now may be `$0.00` if free trial; billing note for `$9.99/month` after trial as shown).

---

### TC-SP-005 — Apply veteran promotional code `VETFREE`

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. On Shopping Cart / Order Summary, locate **Promotional Code** field (`Enter promo code`).
2. Enter `VETFREE`.
3. Click **Apply**.

**Expected Result**

- Promo applies successfully.
- UI shows success indication (e.g. **Promo Applied**).
- Order summary updates according to promo rules (discount / shipping / free messaging as implemented on staging).
- **Proceed to checkout** remains available.

---

### TC-SP-006 — Proceed to checkout

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. Click **Proceed to checkout**.

**Expected Result**

- Checkout sections become available / focus moves into checkout flow.
- Sections visible include (as applicable on page):
  - **Person ordering service** (account creation fields)
  - **Shipping / Delivery Information**
  - **Payment Information** (New Credit / Debit Card + Stripe card details)
  - **Billing / Payment address**
  - **Order Summary**
  - **Submit Order**

---

### TC-SP-007 — Create new account (Person ordering service)

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. In **Person ordering service**, enter:
   - First Name *
   - Last Name *
   - Phone Number *
   - Email Address * (unique)
   - User Name * (unique if required)
   - Password *
   - Confirm Password *
2. Do **not** expect or wait for OTP.

**Expected Result**

- All required account fields accept valid unique data.
- No OTP challenge is presented.
- User can continue to shipping / payment without email verification step.
- Validation errors are **not** shown for valid unique data.

---

### TC-SP-008 — Complete shipping / delivery information

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. Under **Delivery Information / Shipping Information**, enter a valid **US** service location:
   - First Name, Last Name, Phone, Email (or enable **Same as Ordering Person** if that is the intended happy-path option)
   - Service Location Address *
   - City *
   - State *
   - Zip Code *
   - Apt. Ste. (optional)
2. Use address search if available, or enter fields manually.

**Expected Result**

- Shipping fields accept valid US address.
- No geo-restriction error for a valid US address.
- User can proceed to payment section.

---

### TC-SP-009 — Enter Stripe payment details

| Item | Detail |
| --- | --- |
| **Priority** | High |
| **Type** | Functional |

**Steps**

1. Under **Payment Information**, select / keep **New Credit / Debit Card**.
2. Enter **Name on Card** *.
3. In Stripe **Card Details** *, enter:
   - Card number: `4242424242424242`
   - Expiry: `01/27`
   - CVC: `900`
4. For billing address, enable **Same as Shipping Address** (happy path) **or** enter the same valid US billing address.

**Expected Result**

- Stripe card element accepts the test card details.
- No card validation error for this Stripe test card.
- Billing address is complete.
- **Submit Order** is enabled / clickable.

---

### TC-SP-010 — Submit order and verify Order ID (end-to-end pass)

| Item | Detail |
| --- | --- |
| **Priority** | Critical |
| **Type** | E2E |

**Steps**

1. Review Order Summary (Silver, Monthly default, `VETFREE` applied, totals as shown).
2. Click **Submit Order**.
3. Wait for payment processing to complete.
4. Capture confirmation details.

**Expected Result**

- Payment succeeds via Stripe.
- Order is placed successfully.
- Confirmation screen / success state displays an **Order ID**.
- Order ID is non-empty and uniquely identifiable for the transaction.
- No payment failure / recurring setup failure modal is shown for this happy path.

---

## 6. End-to-End Happy Path Checklist (single run)

Use this as a condensed execution checklist covering TC-SP-001 → TC-SP-010.

| Step | Action | Expected |
| --- | --- | --- |
| 1 | Open products URL | Page loads; membership card visible |
| 2 | Click **Silver Plan** | Silver selected; default features unchanged |
| 3 | Click **Add to Cart** | Item in cart; cart count updates |
| 4 | Open cart | Silver membership + **Monthly Plan - $9.99** default |
| 5 | Apply promo `VETFREE` | **Promo Applied** |
| 6 | Click **Proceed to checkout** | Checkout / account + shipping + payment sections available |
| 7 | Create **new** account (unique email); no OTP | Account fields accepted |
| 8 | Enter valid US shipping (and billing) | Address accepted |
| 9 | Pay with `4242424242424242` / `01/27` / `900` | Card accepted |
| 10 | **Submit Order** | Success + **Order ID** displayed |

---

## 7. Traceability — Flow Map

```text
medical-alert-products
        │
        ▼
 Select Silver Plan (default features)
        │
        ▼
     Add to Cart
        │
        ▼
 ecommerce/cart
   • Verify Monthly Plan - $9.99 (default)
   • Apply VETFREE
   • Proceed to checkout
        │
        ▼
 Create new account (Person ordering service)  ← no OTP
        │
        ▼
 Shipping (US) + Billing
        │
        ▼
 Stripe payment (4242…4242 | 01/27 | 900)
        │
        ▼
 Submit Order → Order ID
```

---

## 8. Open Items / Confirm Before Automation

These were not fully specified by stakeholders; fill before scripting:

| ID | Question | Impact |
| --- | --- | --- |
| OI-01 | Exact First/Last/Phone/Password/User Name test values | Account creation steps |
| OI-02 | Exact US shipping & billing address to use on staging | Shipping / geo checks |
| OI-03 | Exact **Name on Card** value | Payment step |
| OI-04 | If **Register as Care Recipient or Member** modal appears, which option for happy path? | May block Submit Order |
| OI-05 | Exact post-payment confirmation URL / Order ID label text | Assertion wording |
| OI-06 | Exact monetary effect of `VETFREE` on Order Summary (beyond “Promo Applied”) | Promo assertion detail |

---

## 9. Execution Log (manual)

| TC ID | Status (Pass/Fail/Blocked) | Actual Result | Order ID | Tester | Date | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| TC-SP-001 |  |  |  |  |  |  |
| TC-SP-002 |  |  |  |  |  |  |
| TC-SP-003 |  |  |  |  |  |  |
| TC-SP-004 |  |  |  |  |  |  |
| TC-SP-005 |  |  |  |  |  |  |
| TC-SP-006 |  |  |  |  |  |  |
| TC-SP-007 |  |  |  |  |  |  |
| TC-SP-008 |  |  |  |  |  |  |
| TC-SP-009 |  |  |  |  |  |  |
| TC-SP-010 |  |  |  |  |  |  |
