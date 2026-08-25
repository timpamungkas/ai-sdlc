# Test Scenario - Return & Check-In Process

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Return \& Check-In Process](#test-scenario---return--check-in-process)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: performing the standardized return inspection and enforcing return hours, per the [Return & Check-In Process](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#6-return--check-in-process) requirement of the PRD - Car Management.
- Preconditions:
  - The pickup crew member is logged in.
  - A vehicle is being returned by a customer.

## Test Case 1
- Description: finalizing check-in with a complete return inspection.
- Steps:
  1. Start the return inspection for the vehicle.
  2. Capture photos and complete the damage checklist (exterior, interior, engine, electrical instruments).
  3. Record the fuel level.
  4. Attempt to finalize check-in.
- Expected result: check-in is finalized because photos, the damage checklist, and fuel level are all recorded.

## Test Case 2
- Description: classifying damage found during inspection.
- Steps:
  1. Identify a visible body dent on the vehicle during inspection.
  2. Identify normal engine wear during inspection.
  3. Classify each finding.
- Expected result: the visible body dent can be marked chargeable; the engine wear is recorded as normal wear and not charged.

## Test Case 3
- Description: attempting to return a vehicle outside permitted hours.
- Steps:
  1. Have a customer arrive to return a vehicle outside 06:00-19:00 local time.
- Expected result: the return is not accepted, and no after-hours key drop is offered.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
