# Test Scenario - Maintenance & Service Scheduling

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Maintenance \& Service Scheduling](#test-scenario---maintenance--service-scheduling)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: scheduling mileage-based maintenance and automatically blocking availability, per the [Maintenance & Service Scheduling](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#7-maintenance--service-scheduling) requirement of the PRD - Car Management.
- Preconditions:
  - The maintenance crew member is logged in.
  - A vehicle's odometer reading is tracked in the system.

## Test Case 1
- Description: scheduling maintenance when a vehicle reaches a 10,000 km multiple.
- Steps:
  1. Update a vehicle's odometer reading to a 10,000 km multiple.
  2. Schedule a maintenance date for the vehicle.
- Expected result: the vehicle is automatically marked unavailable for the scheduled maintenance day(s).

## Test Case 2
- Description: prioritizing maintenance over a conflicting reservation request.
- Steps:
  1. Schedule a maintenance day for a vehicle.
  2. Attempt to allocate the same vehicle to a reservation on the same day.
- Expected result: the vehicle is excluded from allocation for that day in favor of maintenance.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
