# PRD - Car Management

## Document Information
- Product / Feature Name: Car Management
- Author: copilot
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
The company is expanding from car sales into car rental, a new business line with no prior rental experience or dedicated systems. To operate a rental fleet, the company needs a Car Management capability that covers the full lifecycle of a rental vehicle: onboarding, location and inventory tracking, reservation allocation, telemetry and geofencing, pickup/delivery operations, return and check-in, maintenance and cleaning turnaround, damage and incident handling, fuel policy enforcement, utilization analytics, vehicle swaps/substitutions, extensions/early returns, key/access management, workforce task assignment, and alerting.

### Objective
Provide a single system of record and operational workflow for managing rental vehicles across their lifecycle so that Service, Delivery, and Pickup staff (the "Car Management Role") can keep the fleet available, compliant, and in good condition while minimizing manual coordination and avoiding double-booking or unsafe vehicle releases.

### Goals
- Maintain accurate, real-time vehicle status (available, reserved, in-maintenance, decommissioned) so that reservation allocation never double-books a vehicle.
- Standardize vehicle onboarding, pickup/delivery, return/check-in, and incident-reporting workflows with consistent data capture (photos, signatures, timestamps, geolocation).
- Automate maintenance scheduling and availability blocking based on mileage, and enforce insurance-expiry safeguards.
- Provide utilization and idle-time analytics per location and globally.
- Support notification of critical events (geofence breach, maintenance overdue) via dashboard, email, and SMS.

## Problem Statement
The company has no existing system or established process for managing a rental vehicle fleet. Without dedicated Car Management capability:
- Staff cannot reliably track a vehicle's real-time and planned availability, risking double-booking.
- There is no standardized way to capture vehicle onboarding data, lifecycle status, pickup/delivery proof-of-handover, or return inspection results, leading to inconsistent records and disputes over damage/fuel charges.
- Maintenance is not proactively scheduled against mileage, risking vehicles being rented while in poor mechanical condition or with expiring insurance.
- Management has no visibility into fleet utilization or idle vehicles, making it difficult to make fleet-sizing and repositioning decisions.

The key users affected are:
- **Dispatch/Operations staff** who allocate vehicles to reservations and manage transfers between locations.
- **Pickup & Delivery crew** who hand over and receive vehicles from customers, capturing proof-of-handover and inspection data.
- **Maintenance/Service crew** who schedule and perform vehicle servicing.
- **Fleet/Operations managers** who monitor utilization, incidents, and fleet health.

This is important now because the car rental line of business is new; without these capabilities in place before go-live, the company risks unsafe vehicle releases, revenue leakage (unbilled damage/fuel/late charges), and an inability to scale fleet operations across locations.

## Functional Requirements

### 1. Vehicle Onboarding

**Title:** Register a new vehicle into the fleet

**Statement:** As a **fleet operations staff member**, I want **to register a new vehicle with its identifying, ownership, and classification data**, so that **the vehicle can be tracked and made available for rental**.

**Requirement detail:** When a vehicle enters the fleet, the system must capture: VIN, license plate, purchase date, purchase cost, insurance details (policy number, expiry date), odometer reading, vehicle type (brand, model, manufacturing year), size (small/medium), class (economy/luxury), number of seats, fuel type (gas/electric/hybrid), and ownership information (must always be the company). The vehicle must be assigned an initial lifecycle status and a home location.

**Acceptance Criteria:**
- **Given** a staff member is registering a new vehicle, **when** all required onboarding fields (VIN, license plate, purchase date, purchase cost, insurance details, odometer, vehicle type, size, class, seats, fuel type, ownership) are submitted, **then** the vehicle is created with status "Incoming" and an assigned home location.
- **Given** a vehicle registration is submitted with a missing required field, **when** the staff member attempts to save it, **then** the system rejects the submission and indicates which field is missing.
- **Given** a vehicle is successfully registered, **when** the record is viewed, **then** ownership is always shown as the company.

---

**Title:** Manage vehicle lifecycle status

**Statement:** As a **fleet operations staff member**, I want **to move a vehicle through staged lifecycle statuses**, so that **only vehicles that are truly ready are made available for rental**.

**Requirement detail:** The system must support the lifecycle statuses: Incoming, Active, Maintenance, Decommissioning, Sold. Transitions between statuses (except retirement) are manually triggered by staff.

**Acceptance Criteria:**
- **Given** a vehicle is in "Incoming" status, **when** onboarding is completed and staff mark it ready, **then** the vehicle transitions to "Active" and becomes eligible for allocation.
- **Given** an "Active" vehicle requires service, **when** staff mark it for maintenance, **then** its status changes to "Maintenance" and it is removed from availability.
- **Given** a staff member manually decides to retire a vehicle, **when** they trigger decommissioning, **then** the vehicle transitions to "Decommissioning" and then "Sold" once disposed, with no automatic rule-based triggers (age/mileage/depreciation) applied.

### 2. Location & Inventory Management

**Title:** Track vehicle home location and transfer history

**Statement:** As a **fleet operations staff member**, I want **to record each vehicle's home location and its transfer history**, so that **I know where a vehicle is normally based and can account for relocation costs**.

**Requirement detail:** Each vehicle belongs to a single home location at a time. Transferring a vehicle changes its home location and creates a historical record of the change. Costs incurred by a transfer are charged to the company pool (not to a customer or location).

**Acceptance Criteria:**
- **Given** a vehicle has a current home location, **when** staff initiate a transfer to a new location, **then** the vehicle's home location is updated and a new entry is appended to its home-location history (with the previous location, new location, and date).
- **Given** a transfer incurs a cost, **when** the transfer is recorded, **then** the cost is attributed to the company pool.

---

**Title:** View real-time and planned vehicle availability

**Statement:** As a **dispatch staff member**, I want **to see a vehicle's real-time and planned availability**, so that **I can allocate vehicles to reservations without conflicts**.

**Requirement detail:** Real-time availability reflects the vehicle's status for the next 2 hours from the current time. Planned availability covers a rolling window from the next day (H+1) through 30 days ahead (H+30), accounting for existing reservations, maintenance blocks, and transfers.

**Acceptance Criteria:**
- **Given** a vehicle has no reservation or maintenance block in the next 2 hours, **when** its real-time availability is queried, **then** it is shown as available.
- **Given** a vehicle has a reservation scheduled between H+1 and H+30, **when** planned availability is queried for that period, **then** the vehicle is shown as unavailable for the reserved dates.

### 3. Reservation Allocation

**Title:** Automatically allocate a vehicle to a reservation

**Statement:** As a **customer/reservation system**, I want **a vehicle to be automatically allocated when a reservation is made**, so that **staff do not need to manually assign vehicles for every booking**.

**Requirement detail:** Allocation is fully automatic, using a first-come-first-served approach with no prioritization rules (e.g., no exact-model guarantee over class guarantee) and no overbooking.

**Acceptance Criteria:**
- **Given** two reservations request the same vehicle class at overlapping times, **when** the first reservation is confirmed, **then** it is allocated a vehicle and the second reservation is allocated a different available vehicle of the same class or is declined if none is available.
- **Given** no available vehicle exists to satisfy a reservation, **when** the reservation is requested, **then** the system does not overbook and instead flags the reservation as unfulfillable at that time.

### 4. Vehicle Status & Telemetry

**Title:** Monitor vehicle GPS location and geofencing

**Statement:** As a **fleet operations staff member**, I want **to see each vehicle's live GPS location and receive alerts when it leaves its permitted area**, so that **I can respond to unauthorized vehicle movement**.

**Requirement detail:** The system collects GPS telemetry only (no fuel/battery/engine diagnostics). Telemetry updates at least every 5 minutes, in real time where possible. Geofencing alerts trigger when a vehicle exits its permitted operating area.

**Acceptance Criteria:**
- **Given** a vehicle is actively rented, **when** its GPS position updates, **then** the operational dashboard reflects the new position within 5 minutes.
- **Given** a vehicle exits its defined geofence boundary, **when** the breach is detected, **then** an alert is generated and delivered to staff.

### 5. Pickup & Delivery Operations

**Title:** Schedule and perform vehicle pickup/delivery

**Statement:** As a **delivery/pickup crew member**, I want **to schedule and execute vehicle drop-off or pickup at an additional cost**, so that **customers can receive or return vehicles at their preferred location**.

**Requirement detail:** Drop-off/pickup service is available daily between 06:00 and 19:00 local time, at an additional cost to the customer. Route planning is performed manually by the crew.

**Acceptance Criteria:**
- **Given** a customer requests delivery or pickup, **when** they choose a time, **then** only slots between 06:00 and 19:00 local time are offered, with the applicable additional cost shown.
- **Given** a delivery/pickup is scheduled, **when** the crew plans their route, **then** the system does not auto-optimize the route; route planning is manual.

---

**Title:** Capture proof-of-handover and verify customer identity

**Statement:** As a **delivery/pickup crew member**, I want **to capture proof-of-handover artifacts and verify the customer's identity**, so that **there is a verifiable record of the handover and the renter is confirmed to be who they claim**.

**Requirement detail:** Proof-of-handover consists of photos and an e-form with signature, timestamp, and geolocation. Identity verification requires a driver's license scan and a selfie match against a national identifier or passport.

**Acceptance Criteria:**
- **Given** a vehicle handover is being completed, **when** the crew submits the handover record, **then** it must include photos and a signed e-form containing a timestamp and geolocation, or the submission is rejected as incomplete.
- **Given** a customer is picking up a vehicle, **when** identity verification is performed, **then** the driver's license and a live selfie must be matched against the national identifier or passport before handover can be confirmed.

### 6. Return & Check-In Process

**Title:** Perform standardized return inspection

**Statement:** As a **pickup crew member**, I want **to perform a standardized inspection when a vehicle is returned**, so that **damage, fuel level, and mileage are consistently recorded**.

**Requirement detail:** Return inspection captures photos and a damage checklist covering exterior, interior, engine check, and electrical instruments check, plus fuel level. Chargeable damage is defined as visibly identifiable damage from an accident (e.g., visible body dents, visible torn upholstery); engine and electrical instrument wear is considered normal wear and is not chargeable. The system does not auto-generate damage severity scores. Vehicle drop-off/return is only accepted between 06:00 and 19:00 local time; there is no after-hours key drop.

**Acceptance Criteria:**
- **Given** a vehicle is being returned, **when** the crew completes the inspection, **then** photos, the damage checklist (exterior, interior, engine, electrical instruments), and fuel level must all be recorded before check-in can be finalized.
- **Given** damage is identified during inspection, **when** the crew classifies it, **then** only visibly identifiable accident damage (e.g., body dent, torn seat) can be marked chargeable; engine/electrical issues are recorded as normal wear and not charged.
- **Given** a customer attempts to return a vehicle outside 06:00–19:00 local time, **when** they arrive at the location, **then** the return is not accepted (no after-hours key drop is supported).

### 7. Maintenance & Service Scheduling

**Title:** Schedule mileage-based maintenance and block availability

**Statement:** As a **maintenance crew member**, I want **maintenance to be scheduled based on mileage and to automatically block vehicle availability**, so that **vehicles are serviced on time and are not rented while due for service**.

**Requirement detail:** Maintenance is scheduled every 10,000–12,000 kilometers. The crew sets a maintenance date at each 10,000 km multiplication threshold. Maintenance takes priority over reservations: if a vehicle has a maintenance schedule on a given day, it is unavailable that day, and maintenance blocks automatically remove the vehicle from availability.

**Acceptance Criteria:**
- **Given** a vehicle's odometer reaches a 10,000 km multiple, **when** the crew schedules maintenance, **then** the vehicle is automatically marked unavailable for the scheduled maintenance day(s).
- **Given** a vehicle has a scheduled maintenance day that conflicts with a pending reservation request, **when** allocation is attempted, **then** the vehicle is excluded from allocation for that day in favor of maintenance.

### 8. Cleaning & Turnaround

**Title:** Enforce standard turnaround time between rentals

**Statement:** As a **fleet operations staff member**, I want **each vehicle to have a standard turnaround block after return**, so that **vehicles are prepared before being rented again**.

**Requirement detail:** Standard turnaround time is 1 day after return, during which the vehicle is not available for a new reservation. Cleaning levels are not tracked, and no SLA/overdue turnaround monitoring is required.

**Acceptance Criteria:**
- **Given** a vehicle is returned and checked in, **when** availability is calculated, **then** the vehicle remains unavailable for 1 day following the return date before it can be reserved again.

### 9. Damage & Incident Handling

**Title:** Report and record vehicle damage incidents

**Statement:** As a **pickup crew member**, I want **to report a damage incident via a web form with positional photo mapping**, so that **damage is documented and repair costs can be estimated**.

**Requirement detail:** Damage incidents are reported by the pickup crew filling out a web form for the specific vehicle at return inspection. Damage photos are captured with positional mapping on a vehicle diagram. Repair cost is estimated using an internal price list (no parts inventory linkage, no external body shop integration). Post-incident, a vehicle can only be flagged as "Drivable" or "In-maintenance" (not available) — no intermediate "limited" state.

**Acceptance Criteria:**
- **Given** damage is found during return inspection, **when** the crew fills out the incident web form, **then** the damage location(s) are recorded against a vehicle diagram along with supporting photos.
- **Given** a damage incident is recorded, **when** repair cost is estimated, **then** the cost is derived from the internal price list.
- **Given** a vehicle has an unresolved incident, **when** its status is set, **then** it can only be "Drivable" or "In-maintenance" (not available).

### 10. Fuel Management

**Title:** Enforce same-to-same fuel policy

**Statement:** As a **pickup crew member**, I want **the system to compare return fuel level against delivery fuel level**, so that **customers are charged when they return the vehicle with less fuel**.

**Requirement detail:** The fuel policy is same-to-same: the returned vehicle's fuel level must match the level recorded at delivery. If it is lower, the customer is charged. EV charge-level targets and EV charging scheduling are out of scope.

**Acceptance Criteria:**
- **Given** a vehicle's fuel level at delivery is recorded, **when** the vehicle is returned with a lower fuel level, **then** a fuel charge is applied to the customer.
- **Given** the returned fuel level matches the delivery fuel level, **when** check-in is completed, **then** no fuel charge is applied.

### 11. Utilization & Analytics

**Title:** View fleet utilization and idle-time analytics

**Statement:** As a **fleet/operations manager**, I want **to view daily utilization and idle-time metrics per location and globally**, so that **I can identify underused vehicles and make fleet-sizing decisions**.

**Requirement detail:** Utilization is measured on a daily basis (usage and idle time). Both per-location and global (cross-location) dashboards are required. A vehicle idle for 5 consecutive days is flagged as inefficient.

**Acceptance Criteria:**
- **Given** vehicle usage data is recorded daily, **when** a manager views the utilization dashboard, **then** both per-location and global utilization/idle metrics are displayed.
- **Given** a vehicle has been idle for 5 consecutive days, **when** the analytics are refreshed, **then** the vehicle is flagged as an inefficiency.

### 12. Vehicle Swaps & Substitutions

**Title:** Substitute a vehicle when the promised one becomes unavailable

**Statement:** As a **dispatch staff member**, I want **a defined substitution order to follow when a promised vehicle becomes unavailable**, so that **customer impact is minimized in a consistent way**.

**Requirement detail:** Substitution follows this priority: (1) replace with another vehicle of the same type, (2) replace with a better vehicle, (3) refund. No automatic compensation (upgrade/discount) is offered beyond this substitution order, and downgrade/upgrade history is not tracked.

**Acceptance Criteria:**
- **Given** a promised vehicle becomes unavailable, **when** staff seek a substitute, **then** the system first attempts to find a same-type vehicle, then a better vehicle, and only offers a refund if neither is available.

### 13. Extensions & Early Returns

**Title:** Auto-approve rental extension requests

**Statement:** As a **customer**, I want **my extension request to be automatically approved when my vehicle is not already rebooked**, so that **I can continue using the vehicle without waiting for manual approval**.

**Requirement detail:** Extensions are auto-approved when the vehicle is not rebooked for the requested period, incurring base rental cost plus a daily late charge. If the extension conflicts with an upcoming reservation, the requester is warned.

**Acceptance Criteria:**
- **Given** a vehicle has no upcoming reservation conflicting with the requested extension, **when** the customer requests an extension, **then** it is auto-approved and billed at base rental cost plus daily late charge.
- **Given** a requested extension would overlap with an upcoming reservation, **when** the customer submits the request, **then** the system warns of the conflict.

---

**Title:** Reposition early-returned vehicles

**Statement:** As a **fleet operations staff member**, I want **an early-returned vehicle to be repositioned and opened for new reservations**, so that **fleet capacity is not wasted**.

**Requirement detail:** When a vehicle is returned earlier than its scheduled end date, it becomes available for new customer allocation (repositioned) rather than held idle.

**Acceptance Criteria:**
- **Given** a customer returns a vehicle before its scheduled end date, **when** check-in is completed, **then** the vehicle becomes eligible for new reservations for the remaining period, subject to standard turnaround time.

### 14. Key & Access Management

**Title:** Charge customer for lost keys

**Statement:** As a **fleet operations staff member**, I want **to charge a customer when they lose a vehicle key**, so that **the cost of the lost key is recovered**.

**Requirement detail:** Vehicles use either physical keys or smart locks depending on the vehicle. Chain-of-custody tracking is not required. When a key is lost, the customer is charged for it; the physical key replacement process itself is handled manually outside the system.

**Acceptance Criteria:**
- **Given** a customer reports a lost key, **when** staff record the incident, **then** a charge for the lost key is applied to the customer's account.

### 15. Workforce Task Management

**Title:** Assign turnaround tasks to staff after vehicle return

**Statement:** As a **fleet operations staff member**, I want **tasks such as cleaning and inspection to be assigned to staff following defined timing rules**, so that **vehicle turnaround happens promptly**.

**Requirement detail:** Task assignment is manual and occurs on the same day the vehicle is returned. If a vehicle is returned after 15:00 local time, the task is instead assigned for the next day. Mobile task lists, completion scanning, and productivity tracking are not required.

**Acceptance Criteria:**
- **Given** a vehicle is returned before 15:00 local time, **when** staff assign turnaround tasks, **then** the tasks are assignable for the same day.
- **Given** a vehicle is returned after 15:00 local time, **when** staff assign turnaround tasks, **then** the tasks are assignable only starting the next day.

### 16. Alerts & Notifications

**Title:** Receive critical operational alerts

**Statement:** As a **fleet operations staff member**, I want **to be alerted about geofence breaches and overdue maintenance**, so that **I can respond quickly to critical operational issues**.

**Requirement detail:** Critical alerts cover a vehicle leaving its geofence and overdue maintenance. Alerts are delivered via dashboard, email, and SMS.

**Acceptance Criteria:**
- **Given** a vehicle leaves its geofence, **when** the breach is detected, **then** an alert is sent via dashboard, email, and SMS.
- **Given** a vehicle's maintenance becomes overdue, **when** the overdue condition is detected, **then** an alert is sent via dashboard, email, and SMS.

### 17. Risk & Safeguards

**Title:** Block allocation for vehicles with critical faults or expiring insurance

**Statement:** As a **fleet operations staff member**, I want **the system to prevent allocation of vehicles with a known critical fault or soon-to-expire insurance**, so that **the company never releases an unsafe or non-compliant vehicle**.

**Requirement detail:** A vehicle with an active critical fault must be manually marked unavailable for rent. Additionally, the system must block new assignment of a vehicle whose insurance policy will expire within 1 week.

**Acceptance Criteria:**
- **Given** a vehicle has an active critical fault, **when** staff mark it, **then** the vehicle is set unavailable for rent and excluded from allocation.
- **Given** a vehicle's insurance policy expires within 7 days, **when** allocation is attempted for a reservation starting before the expiry, **then** the system blocks the assignment.

## Non-Functional Requirements


## Dependency & Constraints
- No differentiation by usage type (daily rental, long-term lease, subscription pool) is required — all vehicles are managed under a single daily-rental model.
- Vehicle retirement/disposal is manual only; no rule-based triggers (age, mileage, depreciation threshold) are required.
- No overbooking strategy is required; allocation must target zero double-booking incidents.
- Telemetry is limited to GPS only; fuel level, battery %, and engine diagnostics telemetry are out of scope.
- Delivery/pickup route optimization is manual; no route-optimization algorithm is required.
- No automatic damage severity scoring is required.
- After-hours key drop is not supported; pickup/return is restricted to 06:00–19:00 local time.
- Predictive maintenance (fault codes, abnormal usage triggers) is out of scope.
- Cleaning-level tracking (basic/deep/disinfection) and turnaround SLA monitoring are out of scope.
- Parts inventory linkage and external body shop/insurance adjuster integrations are out of scope.
- EV charge-level targets and EV charging scheduling/station integration are out of scope.
- Customer downgrade/upgrade impact history and automatic compensation offers are out of scope.
- Key custody chain-of-custody logging is out of scope; lost-key replacement process (beyond charging the customer) is manual.
- Staff mobile task lists, completion scanning, and task productivity metrics are out of scope for now.
- No mandated regulatory inspection scheduling, region-specific restrictions, or external DMS/telematics/repair-shop integrations are required at this stage.
- Automated repositioning recommendations, AI-based damage detection, and peer-to-peer car sharing support are out of scope for now.
- Minimum required data fields (never-blank validation) will be finalized after UI design; audit log granularity and photo evidence retention duration are not yet defined (N/A).
- Edge cases such as stolen vehicle handling, vehicle returned to the wrong location, and partial/draft inspection saving remain manual processes for now.

## Success Metrics
- Zero double-booking incidents across all locations.
- Vehicle status updates (including telemetry-driven status) reflected on dashboards within 5 minutes of the underlying event.
- 100% of returned vehicles have complete inspection records (photos, damage checklist, fuel level) before check-in is finalized.
- 100% of vehicles with insurance expiring within 7 days are blocked from new assignment.
- Reduction in vehicles idle for 5+ consecutive days, tracked per location and globally.
