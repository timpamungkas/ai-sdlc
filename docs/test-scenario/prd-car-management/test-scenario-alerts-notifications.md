# Test Scenario - Alerts & Notifications

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Alerts \& Notifications](#test-scenario---alerts--notifications)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: receiving critical operational alerts for geofence breaches and overdue maintenance, per the [Alerts & Notifications](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#16-alerts--notifications) requirement of the PRD - Car Management.
- Preconditions:
  - A fleet operations staff member is registered to receive dashboard, email, and SMS alerts.

## Test Case 1
- Description: alerting staff when a vehicle leaves its geofence.
- Steps:
  1. Move a monitored vehicle's position outside its geofence boundary.
- Expected result: an alert is sent via dashboard, email, and SMS.

## Test Case 2
- Description: alerting staff when a vehicle's maintenance becomes overdue.
- Steps:
  1. Allow a vehicle's scheduled maintenance date to pass without service being performed.
- Expected result: an alert is sent via dashboard, email, and SMS.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
