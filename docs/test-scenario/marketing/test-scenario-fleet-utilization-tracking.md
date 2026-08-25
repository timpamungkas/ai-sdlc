# Test Scenario - Fleet Utilization Tracking

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)
- [Test Case 3](#test-case-3)

## Test Scenario
- Feature to test: Fleet utilization tracking, which calculates and displays the "car rented percentage" (rented cars / total fleet) for a given reporting period, and highlights when the weekly target of 75% is not met (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 1).
- Preconditions:
  - The system contains a known total fleet size.
  - The system contains a known number of currently rented cars for the reporting period being tested.
  - The reporting period (day/week/month) has ended.

## Test Case 1
- Description: Calculating the car rented percentage for a completed reporting period.
- Steps:
  1. Set the total fleet size to 100 cars.
  2. Set the number of currently rented cars to 80 for the week.
  3. Wait for the weekly reporting period to end.
  4. View the fleet utilization report for that week.
- Expected result: The system displays a car rented percentage of 80% for that week.

## Test Case 2
- Description: Highlighting when the weekly car rented percentage is below the 75% target.
- Steps:
  1. Set the total fleet size to 100 cars.
  2. Set the number of currently rented cars to 60 for the week.
  3. Wait for the weekly reporting period to end.
  4. Generate the weekly fleet utilization report.
- Expected result: The report displays a car rented percentage of 60% and highlights that the 75% weekly target was not met.

## Test Case 3
- Description: Displaying the car rented percentage for different reporting periods (daily and monthly).
- Steps:
  1. Set the total fleet size to 100 cars.
  2. Set the number of currently rented cars for a single day to 90.
  3. Set the number of currently rented cars for the month to 75.
  4. Request the fleet utilization report for the daily period.
  5. Request the fleet utilization report for the monthly period.
- Expected result: The daily report displays a car rented percentage of 90%, and the monthly report displays a car rented percentage of 75%, each independently calculated for its own period.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
