# Test Scenario - Workforce Task Management

## Document Information
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario - Workforce Task Management](#test-scenario---workforce-task-management)
  - [Document Information](#document-information)
  - [Table of Contents](#table-of-contents)
  - [Test Scenario](#test-scenario)
  - [Test Case 1](#test-case-1)
  - [Test Case 2](#test-case-2)

## Test Scenario
- Feature to test: assigning turnaround tasks to staff after vehicle return, per the [Workforce Task Management](https://github.com/timpamungkas/ai-sdlc/blob/main/docs/prd/prd-car-management.md#15-workforce-task-management) requirement of the PRD - Car Management.
- Preconditions:
  - A fleet operations staff member is logged in.
  - A vehicle has just been returned.

## Test Case 1
- Description: assigning turnaround tasks for a vehicle returned before 15:00 local time.
- Steps:
  1. Return a vehicle at 10:00 local time.
  2. Assign turnaround tasks (cleaning, inspection) to staff.
- Expected result: the tasks are assignable for the same day.

## Test Case 2
- Description: assigning turnaround tasks for a vehicle returned after 15:00 local time.
- Steps:
  1. Return a vehicle at 16:00 local time.
  2. Assign turnaround tasks (cleaning, inspection) to staff.
- Expected result: the tasks are assignable only starting the next day.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
