# Payment System Checklist

## Functional
- [ ] Order is created
- [ ] PayPal order is created
- [ ] Redirect to PayPal works
- [ ] Capture is executed
- [ ] Order status changes to PAID
- [ ] Payment status changes to COMPLETED
- [ ] Stock decreases
- [ ] Email sent
- [ ] Cart cleared
- [ ] Success page loads

## Security
- [ ] JWT required
- [ ] Cannot pay twice
- [ ] Cannot pay another user's order
- [ ] Price cannot be modified from frontend

## Concurrency
- [ ] Two users cannot oversell stock
- [ ] Double capture prevented

## UX
- [ ] Button disabled during processing
- [ ] No duplicate clicks
- [ ] URL cleaned after capture