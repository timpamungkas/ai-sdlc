# Test Scenario - Pricing Management

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)
- [Test Case 3](#test-case-3)
- [Test Case 4](#test-case-4)
- [Test Case 5](#test-case-5)
- [Test Case 6](#test-case-6)

## Test Scenario
- Feature to test: Static pricing per vehicle category and rental term, seasonal/peak pricing overlays, simulated pricing shown on the booking confirmation page, and the audit trail of price tier changes (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirements 4, 5, 6, and 10).
- Preconditions:
  - Marketing staff is logged in with permission to configure pricing.
  - Vehicle categories exist: small regular car, medium regular car, medium luxury car.

## Test Case 1
- Description: Configuring independent daily, weekly, and monthly rates for a vehicle category.
- Steps:
  1. Select the "medium regular car" category.
  2. Set the daily rate, weekly rate, and monthly rate to distinct values.
  3. Save the pricing configuration.
- Expected result: The daily, weekly, and monthly rates are saved independently for the "medium regular car" category.

## Test Case 2
- Description: Applying the correct static rate based on the selected rental duration.
- Steps:
  1. As a customer, select a "small regular car" for a weekly rental.
  2. View the calculated price.
- Expected result: The weekly rate configured for "small regular car" is applied to calculate the price.

## Test Case 3
- Description: Applying a seasonal/peak pricing overlay when the booking date falls within the configured peak period.
- Steps:
  1. Configure a peak holiday date range with an adjusted rate for the "medium luxury car" category.
  2. As a customer, select a "medium luxury car" for a rental date within that peak date range.
  3. View the calculated price.
- Expected result: The overlay rate is applied instead of the standard static rate for the "medium luxury car" category.

## Test Case 4
- Description: Applying the standard static rate when the booking date falls outside any peak period.
- Steps:
  1. Configure a peak holiday date range with an adjusted rate for the "medium luxury car" category.
  2. As a customer, select a "medium luxury car" for a rental date outside that peak date range.
  3. View the calculated price.
- Expected result: The standard static rate applies, with no overlay adjustment.

## Test Case 5
- Description: Displaying the simulated total price on the confirmation page, including a peak overlay when applicable.
- Steps:
  1. Configure a peak holiday date range with an adjusted rate for a vehicle category.
  2. As a customer, select a vehicle in that category, a duration, and dates within the peak period.
  3. Proceed to the booking confirmation page.
- Expected result: The confirmation page displays the simulated total price reflecting the overlay-adjusted rate, prior to payment.

## Test Case 6
- Description: Recording an audit trail entry when a price tier is changed.
- Steps:
  1. As marketing staff, change the static daily rate for the "small regular car" category from its current value to a new value.
  2. Save the change.
  3. As an authorized user, view the price tier's change history.
- Expected result: The history shows the user who made the change, the timestamp of the change, and the previous value, listed in chronological order among any other prior changes.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
