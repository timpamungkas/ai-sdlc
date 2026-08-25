# Test Scenario - Vehicle Swaps & Substitutions

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Vehicle Swaps \& Substitutions](#test-scenario---vehicle-swaps--substitutions)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: substituting a vehicle when the promised one becomes unavailable, per the [Vehicle Swaps & Substitutions](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#12-vehicle-swaps--substitutions) requirement of the PRD - Car Management.
- Preconditions:
  - A dispatch staff member is logged in.
  - A promised vehicle for a reservation becomes unavailable.

## Test Case 1
- Description: substituting with a same-type vehicle when available.
- Steps:
  1. Identify a reservation whose promised vehicle is unavailable.
  2. Search for a substitute where a same-type vehicle is available.
- Expected result: the system offers the same-type vehicle as the substitute.

## Test Case 2
- Description: substituting with a better vehicle when no same-type vehicle is available.
- Steps:
  1. Identify a reservation whose promised vehicle is unavailable.
  2. Ensure no same-type vehicle is available, but a better vehicle is available.
  3. Search for a substitute.
- Expected result: the system offers the better vehicle as the substitute.

## Test Case 3
- Description: offering a refund when no substitute vehicle is available.
- Steps:
  1. Identify a reservation whose promised vehicle is unavailable.
  2. Ensure neither a same-type nor a better vehicle is available.
  3. Search for a substitute.
- Expected result: the system offers a refund.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
