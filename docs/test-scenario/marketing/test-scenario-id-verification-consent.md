# Test Scenario - ID Verification Consent

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: Capturing explicit customer consent for ID card validation before a booking can be completed (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 8).
- Preconditions:
  - A customer has reached the ID validation step of the booking process.

## Test Case 1
- Description: Completing a booking after providing consent for ID validation.
- Steps:
  1. As a customer, proceed through the booking process to the ID validation step.
  2. Provide explicit consent for ID card validation.
  3. Continue and complete the booking.
- Expected result: The booking proceeds and can be completed after consent is provided.

## Test Case 2
- Description: Blocking booking completion when consent is not given.
- Steps:
  1. As a customer, proceed through the booking process to the ID validation step.
  2. Decline to provide consent for ID card validation.
  3. Attempt to proceed with the booking.
- Expected result: The booking cannot be completed without explicit consent.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
