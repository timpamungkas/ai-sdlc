# Test Scenario - Vehicle Onboarding

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Vehicle Onboarding](#test-scenario---vehicle-onboarding)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)
  - [Test Case 3](#test-case-3)
  - [Test Case 4](#test-case-4)
  - [Test Case 5](#test-case-5)
  - [Test Case 6](#test-case-6)

## Test Scenario
- Feature to test: registering a new vehicle into the fleet and managing its lifecycle status, per the [Vehicle Onboarding](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#1-vehicle-onboarding) requirement of the PRD - Car Management.
- Preconditions:
  - The staff member is logged in with fleet operations staff permissions.
  - The vehicle onboarding form is accessible.

## Test Case 1
- Description: registering a new vehicle with all required fields.
- Steps:
  1. Open the vehicle registration form.
  2. Enter VIN, license plate, purchase date, purchase cost, insurance policy number and expiry date, odometer reading, vehicle type (brand, model, manufacturing year), size, class, number of seats, and fuel type.
  3. Submit the form.
- Expected result: the vehicle is created with status "Incoming", ownership set to the company, and an assigned home location.

## Test Case 2
- Description: submitting a vehicle registration with a missing required field.
- Steps:
  1. Open the vehicle registration form.
  2. Fill in all fields except the insurance expiry date.
  3. Submit the form.
- Expected result: the system rejects the submission and indicates that the insurance expiry date is missing.

## Test Case 3
- Description: verifying ownership is always the company.
- Steps:
  1. Register a new vehicle successfully.
  2. Open the vehicle record.
- Expected result: the ownership field displays the company as the owner.

## Test Case 4
- Description: transitioning a vehicle from "Incoming" to "Active".
- Steps:
  1. Locate a vehicle in "Incoming" status.
  2. Complete the onboarding checklist.
  3. Mark the vehicle as ready.
- Expected result: the vehicle status changes to "Active" and it becomes eligible for allocation.

## Test Case 5
- Description: transitioning an "Active" vehicle into "Maintenance".
- Steps:
  1. Locate a vehicle in "Active" status.
  2. Mark the vehicle for maintenance.
- Expected result: the vehicle status changes to "Maintenance" and it is removed from availability.

## Test Case 6
- Description: manually decommissioning and selling a vehicle.
- Steps:
  1. Locate an "Active" vehicle.
  2. Trigger decommissioning.
  3. Mark the vehicle as disposed.
- Expected result: the vehicle transitions to "Decommissioning" and then to "Sold", with no automatic rule-based trigger applied.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
