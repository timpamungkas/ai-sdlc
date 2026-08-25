# Test Scenario - Risk & Safeguards

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Risk \& Safeguards](#test-scenario---risk--safeguards)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: blocking allocation of vehicles with critical faults or soon-to-expire insurance, per the [Risk & Safeguards](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#17-risk--safeguards) requirement of the PRD - Car Management.
- Preconditions:
  - A fleet operations staff member is logged in.

## Test Case 1
- Description: excluding a vehicle with an active critical fault from allocation.
- Steps:
  1. Mark a vehicle as having an active critical fault.
  2. Attempt to allocate the vehicle to a reservation.
- Expected result: the vehicle is set unavailable for rent and excluded from allocation.

## Test Case 2
- Description: blocking assignment of a vehicle with insurance expiring within 7 days.
- Steps:
  1. Set a vehicle's insurance policy to expire within 7 days.
  2. Attempt to allocate the vehicle to a reservation starting before the expiry date.
- Expected result: the system blocks the assignment.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
