# Payment System – Detailed Test Cases

Project: Level Up Gaming E-Commerce  
Module: PayPal Payment Integration  
Environment: Windows 11 / Chrome / PayPal Sandbox

---

# 1. FUNCTIONAL TEST CASES

---

## TC-01 Successful Payment Flow

Precondition:
- User is logged in
- Product stock > 0

Steps:
1. Open shop page
2. Add product to cart
3. Open cart page
4. Click "Checkout"
5. Fill shipping form:
   - Full Name: Ivan Ivanov
   - Phone: 123456789
   - Country: Ukraine
   - City: Kyiv
   - Address: Test 1
   - Postal Code: 01001
6. Click "Pay with PayPal"
7. Complete payment in PayPal sandbox

Expected Result:
- Redirect to success.html
- Order.status = PAID
- Payment.status = COMPLETED
- Stock decreased by correct quantity
- Cart cleared
- Email notification sent

---

## TC-02 Cancel Payment

Precondition:
- User on PayPal payment page

Steps:
1. Click "Cancel" on PayPal
2. System redirects to cancel.html

Expected Result:
- Order.status = PENDING
- Payment.status = PENDING
- No stock decrease
- User sees cancellation message

---

## TC-03 Refresh After Payment

Precondition:
- Payment already completed

Steps:
1. Refresh checkout page after successful payment

Expected Result:
- Backend responds "Already paid"
- No additional stock decrement
- No duplicate email

---

## TC-04 Double Click Protection

Steps:
1. Click "Pay with PayPal" multiple times quickly

Expected Result:
- Button disabled after first click
- Only one order created

---

# 2. NEGATIVE TEST CASES

---

## TC-05 Empty Cart

Precondition:
- Cart empty

Steps:
1. Open checkout page

Expected Result:
- "Cart is empty" message shown
- No payment button available

---

## TC-06 Missing JWT

Steps:
1. Clear localStorage
2. Open checkout page

Expected Result:
- Redirect to login page

---

## TC-07 Invalid Order ID

Steps:
1. Manually send POST /payments/create with invalid orderId

Expected Result:
- 404 Order not found
- No PayPal order created

---

## TC-08 Insufficient Stock

Precondition:
- Product stock = 1

Steps:
1. User A purchases product
2. User B attempts to purchase same product simultaneously

Expected Result:
- One payment successful
- Second user receives stock error
- No overselling

---

# 3. SECURITY TEST CASES

---

## TC-09 Modify Price via DevTools

Steps:
1. Change product price in browser DevTools
2. Complete checkout

Expected Result:
- PayPal shows price from database
- Modified price ignored

---

## TC-10 Attempt Double Capture via Postman

Steps:
1. Send POST /payments/capture/{paypalOrderId} twice

Expected Result:
- First request succeeds
- Second returns "Already paid"
- No duplicate stock decrement

---

## TC-11 Unauthorized Order Access

Steps:
1. Login as User A
2. Try to pay order belonging to User B

Expected Result:
- Access denied or error
- Payment not processed

---

# 4. CONCURRENCY TEST CASES

---

## TC-12 Parallel Capture Requests

Steps:
1. Send two capture requests simultaneously

Expected Result:
- Only one succeeds
- Second safely returns "Already paid"

---

## TC-13 High Latency Simulation

Steps:
1. Enable Network throttling (Slow 3G)
2. Click payment

Expected Result:
- Button disabled
- No duplicate requests

---

# 5. UX TEST CASES

---

## TC-14 Loading State

Steps:
1. Click "Pay with PayPal"

Expected Result:
- Button text changes to "Processing..."
- Button disabled

---

## TC-15 URL Cleanup

Precondition:
- Successful payment

Steps:
1. Observe URL after capture

Expected Result:
- token removed from URL
- No duplicate capture on refresh

---

# 6. EMAIL TEST CASES

---

## TC-16 Email Sent After Payment

Steps:
1. Complete payment

Expected Result:
- Email contains:
  - Order ID
  - Items
  - Total price
  - Payment status

---

# 7. DATABASE VALIDATION TESTS

---

## TC-17 Order Status Transition

Steps:
1. Complete payment

Expected Result:
- PENDING → PAID only
- No invalid transitions allowed

---

## TC-18 Payment Status Transition

Steps:
1. Complete payment

Expected Result:
- PENDING → COMPLETED
- Cannot revert to PENDING