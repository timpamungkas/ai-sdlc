# Test Scenario - Cleaning & Turnaround

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Cleaning \& Turnaround](#test-scenario---cleaning--turnaround)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)

## Test Scenario
- Feature to test: enforcing the standard turnaround time between rentals, per the [Cleaning & Turnaround](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#8-cleaning--turnaround) requirement of the PRD - Car Management.
- Preconditions:
  - A vehicle has just been returned and checked in.

## Test Case 1
- Description: verifying the vehicle remains unavailable during the turnaround period.
- Steps:
  1. Complete check-in for a returned vehicle, noting the return date.
  2. Query the vehicle's availability for the day immediately following the return date.
  3. Query the vehicle's availability for the second day following the return date.
- Expected result: the vehicle is unavailable for 1 day following the return date and can be reserved again afterward.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
