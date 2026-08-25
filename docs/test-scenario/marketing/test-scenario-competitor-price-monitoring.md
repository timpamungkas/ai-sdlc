# Test Scenario - Competitor Price Monitoring

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: Recording manual competitor pricing observations so marketing staff can review pricing competitiveness (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 7).
- Preconditions:
  - Marketing staff is logged in with permission to record competitor price observations.

## Test Case 1
- Description: Recording a competitor price observation with the required details.
- Steps:
  1. As marketing staff, create a new competitor price observation.
  2. Enter the competitor's price, the observation date, and the vehicle category.
  3. Save the observation.
- Expected result: The observation is stored with the entered price, observation date, and vehicle category for reference.

## Test Case 2
- Description: Viewing previously recorded competitor price observations.
- Steps:
  1. As marketing staff, record two competitor price observations for the same vehicle category on different dates.
  2. Open the list of competitor price observations for that vehicle category.
- Expected result: Both observations are listed, each showing its own observation date, price, and vehicle category.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
