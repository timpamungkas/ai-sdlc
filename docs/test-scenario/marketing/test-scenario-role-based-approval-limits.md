# Test Scenario - Role-Based Approval Limits

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)
- [Test Case 3](#test-case-3)
- [Test Case 4](#test-case-4)

## Test Scenario
- Feature to test: Configurable, role-based maximum rental approval amounts (staff, supervisor, marketing department head, marketing director) used to control approval authority and reduce non-payment risk (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirement 9).
- Preconditions:
  - Approval limits (X, Y, Z) are configured for the staff, supervisor, and marketing department head roles respectively.
  - A rental request with a known value is pending approval.

## Test Case 1
- Description: Approving a rental value at or below a role's configured limit.
- Steps:
  1. Configure the staff approval limit (X) to a specific amount.
  2. As a user with the "staff" role, attempt to approve a rental with a value equal to X.
- Expected result: The approval succeeds.

## Test Case 2
- Description: Rejecting and escalating a rental value above a role's configured limit.
- Steps:
  1. Configure the supervisor approval limit (Y) to a specific amount.
  2. As a user with the "supervisor" role, attempt to approve a rental with a value greater than Y.
- Expected result: The approval is rejected and the rental must be escalated to a higher role for approval.

## Test Case 3
- Description: Marketing director approving a rental of any value.
- Steps:
  1. As a user with the "marketing director" role, attempt to approve a rental with a very high value.
- Expected result: The approval succeeds regardless of the rental amount.

## Test Case 4
- Description: Updated approval limits are applied to subsequent approval checks.
- Steps:
  1. As an authorized administrator, update the marketing department head approval limit (Z) to a new, lower amount.
  2. Save the updated configuration.
  3. As a user with the "marketing department head" role, attempt to approve a rental with a value that was previously within the old limit but now exceeds the new limit.
- Expected result: The approval is rejected because the check uses the newly saved limit, not the previous one.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
