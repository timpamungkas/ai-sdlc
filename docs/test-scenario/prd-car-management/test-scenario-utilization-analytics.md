# Test Scenario - Utilization & Analytics

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Utilization \& Analytics](#test-scenario---utilization--analytics)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: viewing fleet utilization and idle-time analytics, per the [Utilization & Analytics](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#11-utilization--analytics) requirement of the PRD - Car Management.
- Preconditions:
  - The fleet/operations manager is logged in.
  - Daily vehicle usage data has been recorded for at least one location.

## Test Case 1
- Description: viewing per-location and global utilization dashboards.
- Steps:
  1. Open the utilization dashboard.
  2. Select a specific location view.
  3. Select the global (cross-location) view.
- Expected result: both per-location and global utilization/idle metrics are displayed.

## Test Case 2
- Description: flagging a vehicle idle for 5 consecutive days.
- Steps:
  1. Ensure a vehicle has no usage recorded for 5 consecutive days.
  2. Refresh the analytics.
- Expected result: the vehicle is flagged as an inefficiency.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
