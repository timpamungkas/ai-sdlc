# Test Scenario - Fuel Management

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Fuel Management](#test-scenario---fuel-management)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: enforcing the same-to-same fuel policy at return, per the [Fuel Management](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#10-fuel-management) requirement of the PRD - Car Management.
- Preconditions:
  - The vehicle's fuel level at delivery has been recorded.
  - The vehicle is being returned by the customer.

## Test Case 1
- Description: charging the customer when the vehicle is returned with less fuel.
- Steps:
  1. Record the vehicle's fuel level at delivery.
  2. Record a lower fuel level at return.
  3. Complete check-in.
- Expected result: a fuel charge is applied to the customer.

## Test Case 2
- Description: not charging the customer when fuel levels match.
- Steps:
  1. Record the vehicle's fuel level at delivery.
  2. Record the same fuel level at return.
  3. Complete check-in.
- Expected result: no fuel charge is applied.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
