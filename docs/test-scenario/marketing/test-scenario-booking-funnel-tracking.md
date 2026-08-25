# Test Scenario - Booking Funnel Tracking

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)
- [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: Tracking a booking attempt through the funnel stages (search, vehicle selection, confirmation, payment) so marketing can understand acquisition performance (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 3).
- Preconditions:
  - The booking system is available for customers to start a search.
  - A booking attempt can be uniquely identified and tracked.

## Test Case 1
- Description: Recording each funnel stage as a customer completes a full booking.
- Steps:
  1. Start a car search as a customer.
  2. Proceed to select a vehicle.
  3. Proceed to the booking confirmation page.
  4. Complete payment.
  5. View the booking attempt's recorded funnel stages.
- Expected result: All four stages (search, vehicle selection, confirmation, payment) are recorded against the booking attempt.

## Test Case 2
- Description: Viewing the last completed stage for an abandoned booking attempt.
- Steps:
  1. Start a car search as a customer.
  2. Proceed to select a vehicle.
  3. Proceed to the booking confirmation page.
  4. Do not complete payment; end the session.
  5. As marketing staff, view the booking attempt's recorded funnel stage.
- Expected result: The booking attempt shows "confirmation" as the last completed stage, with no payment stage recorded, and no automated recovery action is triggered.

## Test Case 3
- Description: Recording an early-abandoned booking attempt that only reaches the search stage.
- Steps:
  1. Start a car search as a customer.
  2. End the session without selecting a vehicle.
  3. As marketing staff, view the booking attempt's recorded funnel stage.
- Expected result: The booking attempt shows "search" as the only completed stage.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
