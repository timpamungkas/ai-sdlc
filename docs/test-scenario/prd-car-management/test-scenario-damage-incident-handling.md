# Test Scenario - Damage & Incident Handling

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Damage \& Incident Handling](#test-scenario---damage--incident-handling)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: reporting and recording vehicle damage incidents, per the [Damage & Incident Handling](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#9-damage--incident-handling) requirement of the PRD - Car Management.
- Preconditions:
  - The pickup crew member is logged in.
  - Damage is found during a vehicle's return inspection.

## Test Case 1
- Description: recording a damage incident via the web form.
- Steps:
  1. Open the damage incident web form for the vehicle.
  2. Mark the damage location(s) on the vehicle diagram.
  3. Attach supporting photos.
  4. Submit the form.
- Expected result: the damage location(s) and supporting photos are recorded against the vehicle diagram.

## Test Case 2
- Description: estimating repair cost from the internal price list.
- Steps:
  1. Submit a damage incident for a vehicle.
  2. Request a repair cost estimate.
- Expected result: the estimated cost is derived from the internal price list.

## Test Case 3
- Description: setting vehicle status after an unresolved incident.
- Steps:
  1. Record a damage incident for a vehicle.
  2. Attempt to set the vehicle's status.
- Expected result: the vehicle can only be set to "Drivable" or "In-maintenance" (not available).

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
