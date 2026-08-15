# PRD - Damage & Incident Handling

## Document Information
- Product / Feature Name: Damage & Incident Handling
- Author: Copilot
- Date:
- Version:

## Table of Contents
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [Dependency & Constraints](#dependency--constraints)
- [Success Metrics](#success-metrics)

## Overview

### Background
The company is expanding from car sales into car rental, a new business line with no prior rental experience or dedicated systems. This PRD expands on the [Damage & Incident Handling](./prd-car-management.md#9-damage--incident-handling) functional requirement defined in the [PRD - Car Management](./prd-car-management.md). Rental vehicles are returned and inspected at high frequency, and today there is no standardized way for the pickup crew to document damage found during return inspection, estimate the associated repair cost, or ensure the vehicle's status accurately reflects that it can no longer be released for rent as-is.

### Objective
Provide the pickup crew with a simple, consistent way to report vehicle damage incidents found at return inspection, capturing the damage location and supporting photos, estimating repair cost from an internal price list, and ensuring the vehicle's status is correctly restricted while the incident is unresolved.

### Goals
- Standardize how damage incidents are reported, using a web form filled out by the pickup crew during return inspection.
- Record damage location(s) against a vehicle diagram, together with supporting photos, for every reported incident.
- Estimate repair cost automatically from an internal price list, without relying on parts inventory or external body shop systems.
- Prevent a vehicle with an unresolved incident from being marked available for rent, restricting its status to "Drivable" or "In-maintenance" only.

## Problem Statement
The company has no existing process or system for documenting vehicle damage found during return inspection. Without standardized damage and incident handling:
- Damage found at return is not consistently documented, leading to disputes with customers over responsibility and repair charges.
- There is no standard way to estimate repair cost, causing delays and inconsistent charges to customers.
- A damaged vehicle could be mistakenly released for rent again if its status is not correctly restricted while the incident is unresolved.

The key users affected are:
- **Pickup crew members** who inspect returned vehicles and report damage incidents.
- **Fleet/Operations managers** who rely on accurate incident records and vehicle status to keep the fleet safe and available.

This is important now because the car rental line of business is new; without this capability in place before go-live, the company risks unbilled repair costs, customer disputes, and unsafe vehicles being re-released for rent.

## Functional Requirements

### 1. Report and Record Vehicle Damage Incidents

**Title:** Report and record vehicle damage incidents

**Statement:** As a **pickup crew member**, I want **to report a damage incident via a web form with positional photo mapping**, so that **damage is documented and repair costs can be estimated**.

**Requirement detail:** Damage incidents are reported by the pickup crew filling out a web form for the specific vehicle at return inspection. Damage photos are captured with positional mapping on a vehicle diagram. Repair cost is estimated using an internal price list (no parts inventory linkage, no external body shop integration). Post-incident, a vehicle can only be flagged as "Drivable" or "In-maintenance" (not available) — no intermediate "limited" state.

**Acceptance Criteria:**
- **Given** damage is found during return inspection, **when** the crew fills out the incident web form, **then** the damage location(s) are recorded against a vehicle diagram along with supporting photos.
- **Given** a damage incident is recorded, **when** repair cost is estimated, **then** the cost is derived from the internal price list.
- **Given** a vehicle has an unresolved incident, **when** its status is set, **then** it can only be "Drivable" or "In-maintenance" (not available).

## Non-Functional Requirements


## Dependency & Constraints
- No automatic damage severity scoring is required.
- Parts inventory linkage and external body shop/insurance adjuster integrations are out of scope.
- No intermediate "limited availability" vehicle status is supported; a vehicle with an unresolved incident is either "Drivable" or "In-maintenance".
- Minimum required data fields (never-blank validation) will be finalized after UI design; photo evidence retention duration is not yet defined (N/A).

## Success Metrics
- 100% of returned vehicles with damage found at inspection have a recorded incident with photos and a damage location before check-in is finalized.
- 100% of recorded damage incidents have a repair cost estimate derived from the internal price list.
- Zero vehicles with an unresolved incident released as available for rent.

## AI usage disclaimer
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
