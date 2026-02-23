# Payment System Test Plan

## Objective
Verify correctness, reliability, and security of PayPal payment integration.

## Test Scope
- Order creation
- PayPal order creation
- Payment capture
- Order status updates
- Stock decrement
- Email notification
- Frontend behavior

## Test Types
- Functional
- Edge Case
- Concurrency
- Security
- UX

## Test Environment
- Windows 11
- Chrome (latest)
- Node.js backend
- PostgreSQL
- PayPal Sandbox

## Entry Criteria
- Backend running
- Database connected
- PayPal sandbox configured
- JWT authentication functional

## Exit Criteria
- All critical test cases passed
- No high severity bugs open