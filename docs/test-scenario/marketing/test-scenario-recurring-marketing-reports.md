# Test Scenario - Recurring Marketing Reports

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
- Feature to test: Recurring reports (rented car percentage, income, new customers, repeat customers) produced on a daily, weekly, and monthly cadence to help marketing monitor business health (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 13).
- Preconditions:
  - Fleet, rental, income, and customer data exist for at least one completed daily, weekly, and monthly reporting period.

## Test Case 1
- Description: Generating a complete report for a completed reporting period.
- Steps:
  1. Wait for a weekly reporting period to end.
  2. Generate the weekly marketing report.
- Expected result: The report includes the rented car percentage, income, new customer count, and repeat customer count for that week.

## Test Case 2
- Description: Report figures reflect only data from the requested period.
- Steps:
  1. Generate the monthly marketing report for a specific month.
  2. Compare the reported new customer count against the known number of new customers acquired only within that month.
- Expected result: The reported new customer count matches only customers acquired within the requested month, excluding data from other periods.

## Test Case 3
- Description: Generating reports for daily, weekly, and monthly cadences independently.
- Steps:
  1. Generate the daily marketing report for a specific day.
  2. Generate the weekly marketing report for the week containing that day.
  3. Generate the monthly marketing report for the month containing that day.
- Expected result: Each report (daily, weekly, monthly) is produced independently and reflects the figures for its own respective period.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
