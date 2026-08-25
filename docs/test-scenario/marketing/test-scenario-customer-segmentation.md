# Test Scenario - Customer Segmentation

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
- Feature to test: Manual classification of customers into segments (local, tourist, insurance replacement) so marketing staff can tailor acquisition and communication efforts (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 2).
- Preconditions:
  - Marketing staff is logged in with permission to edit customer records.
  - A customer record exists in the system.

## Test Case 1
- Description: Assigning a segment to a new customer record.
- Steps:
  1. Open a new customer record with no segment assigned.
  2. Select the segment "local".
  3. Save the customer record.
- Expected result: The customer record is saved with the segment "local" assigned.

## Test Case 2
- Description: Viewing the assigned segment on a customer record.
- Steps:
  1. Open a customer record whose segment is set to "tourist".
- Expected result: The customer record displays "tourist" as the assigned segment.

## Test Case 3
- Description: Changing a customer's segment to a different valid value.
- Steps:
  1. Open a customer record whose segment is currently set to "local".
  2. Change the segment to "insurance replacement".
  3. Save the customer record.
- Expected result: The customer record is updated and displays "insurance replacement" as the assigned segment.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
