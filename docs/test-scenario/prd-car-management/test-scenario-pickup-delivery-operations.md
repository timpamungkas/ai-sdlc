# Test Scenario - Pickup & Delivery Operations

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Pickup \& Delivery Operations](#test-scenario---pickup--delivery-operations)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)
  - [Test Case 4](#test-case-4)

## Test Scenario
- Feature to test: scheduling vehicle pickup/delivery, and capturing proof-of-handover with identity verification, per the [Pickup & Delivery Operations](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#5-pickup--delivery-operations) requirement of the PRD - Car Management.
- Preconditions:
  - The delivery/pickup crew member is logged in.
  - A customer reservation exists and is ready for handover.

## Test Case 1
- Description: scheduling a delivery/pickup within permitted hours.
- Steps:
  1. Open the delivery/pickup scheduling screen for a reservation.
  2. Attempt to choose a time slot between 06:00 and 19:00 local time.
- Expected result: the slot is offered along with the applicable additional cost.

## Test Case 2
- Description: attempting to schedule a delivery/pickup outside permitted hours.
- Steps:
  1. Open the delivery/pickup scheduling screen for a reservation.
  2. Attempt to choose a time slot outside 06:00-19:00 local time.
- Expected result: the slot is not offered.

## Test Case 3
- Description: submitting a complete proof-of-handover record.
- Steps:
  1. Perform a vehicle handover for a reservation.
  2. Capture photos and complete the e-form with signature, timestamp, and geolocation.
  3. Submit the handover record.
- Expected result: the handover record is accepted.

## Test Case 4
- Description: verifying customer identity before confirming handover.
- Steps:
  1. Scan the customer's driver's license.
  2. Capture a live selfie.
  3. Match the selfie against the national identifier or passport.
- Expected result: the handover can only be confirmed after the driver's license and selfie are successfully matched.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
