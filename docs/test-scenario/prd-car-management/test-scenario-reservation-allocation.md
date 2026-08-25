# Test Scenario - Reservation Allocation

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Reservation Allocation](#test-scenario---reservation-allocation)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: automatic vehicle allocation for reservations, per the [Reservation Allocation](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#3-reservation-allocation) requirement of the PRD - Car Management.
- Preconditions:
  - At least two vehicles of the same class are available in the fleet.
  - The reservation system is able to submit reservation requests.

## Test Case 1
- Description: allocating vehicles to two overlapping reservations of the same class.
- Steps:
  1. Submit a reservation request for a given vehicle class and time window.
  2. Confirm the reservation.
  3. Submit a second reservation request for the same class and an overlapping time window.
- Expected result: the first reservation is allocated an available vehicle; the second reservation is allocated a different available vehicle of the same class, or is declined if none is available.

## Test Case 2
- Description: attempting to allocate a vehicle when no vehicle is available.
- Steps:
  1. Ensure no vehicle of the requested class is available for the requested time window.
  2. Submit a reservation request for that class and time window.
- Expected result: the system does not overbook and flags the reservation as unfulfillable at that time.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
