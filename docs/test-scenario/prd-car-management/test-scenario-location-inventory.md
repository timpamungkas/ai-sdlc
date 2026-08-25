# Test Scenario - Location & Inventory Management

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Location \& Inventory Management](#test-scenario---location--inventory-management)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)
  - [Test Case 4](#test-case-4)

## Test Scenario
- Feature to test: tracking a vehicle's home location, transfer history, and its real-time and planned availability, per the [Location & Inventory Management](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#2-location--inventory-management) requirement of the PRD - Car Management.
- Preconditions:
  - The staff member is logged in with fleet operations or dispatch staff permissions.
  - A vehicle exists with a current home location.

## Test Case 1
- Description: transferring a vehicle to a new home location.
- Steps:
  1. Open the vehicle record for a vehicle with a current home location.
  2. Initiate a transfer to a new location.
  3. Confirm the transfer.
- Expected result: the vehicle's home location is updated, and a new entry (previous location, new location, date) is appended to its home-location history.

## Test Case 2
- Description: verifying transfer costs are attributed to the company pool.
- Steps:
  1. Record a transfer that incurs a cost.
- Expected result: the cost is attributed to the company pool, not to a customer or location.

## Test Case 3
- Description: viewing real-time availability for a vehicle with no upcoming blocks.
- Steps:
  1. Select a vehicle with no reservation or maintenance block in the next 2 hours.
  2. Query its real-time availability.
- Expected result: the vehicle is shown as available.

## Test Case 4
- Description: viewing planned availability for a vehicle with a reservation between H+1 and H+30.
- Steps:
  1. Select a vehicle with a reservation scheduled between H+1 and H+30.
  2. Query planned availability for that period.
- Expected result: the vehicle is shown as unavailable for the reserved dates.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
