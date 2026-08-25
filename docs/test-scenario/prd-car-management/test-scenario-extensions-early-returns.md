# Test Scenario - Extensions & Early Returns

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Extensions \& Early Returns](#test-scenario---extensions--early-returns)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: auto-approving rental extension requests and repositioning early-returned vehicles, per the [Extensions & Early Returns](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#13-extensions--early-returns) requirement of the PRD - Car Management.
- Preconditions:
  - A customer has an active rental.

## Test Case 1
- Description: auto-approving an extension request with no conflicting reservation.
- Steps:
  1. Verify the rented vehicle has no upcoming reservation conflicting with the requested extension period.
  2. Submit an extension request for that period.
- Expected result: the extension is auto-approved and billed at base rental cost plus a daily late charge.

## Test Case 2
- Description: warning the customer of a conflicting extension request.
- Steps:
  1. Verify the rented vehicle has an upcoming reservation overlapping with the requested extension period.
  2. Submit an extension request for that period.
- Expected result: the system warns of the conflict.

## Test Case 3
- Description: repositioning a vehicle returned before its scheduled end date.
- Steps:
  1. Return a vehicle before its scheduled end date.
  2. Complete check-in.
  3. Search for available vehicles for the remaining period.
- Expected result: the vehicle becomes eligible for new reservations for the remaining period, subject to standard turnaround time.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
