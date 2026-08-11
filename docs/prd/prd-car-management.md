# PRD - Car Management

## Document Information
- Product / Feature Name: Car Management
- Author: Copilot
- Date:
- Version:

## Overview

### Background
Our company is expanding from car sales into car rental, a new business line with no prior rental experience or dedicated systems. Car Management is the foundational capability of the rental platform: it governs how vehicles enter the fleet, how they are tracked across locations, how they are allocated to reservations, and how they are maintained, cleaned, inspected, and retired throughout their operational lifecycle. Without a dedicated Car Management capability, staff would have no consistent way to know which vehicles exist, where they are, whether they are available, or whether they are safe to rent.

### Objective
Provide a system that maintains an accurate, real-time, end-to-end record of every vehicle in the rental fleet — from onboarding through daily operations (pickup, delivery, return, maintenance, cleaning, damage handling) to retirement — so that operations staff can reliably determine vehicle availability, condition, and location at any time.

### Goals
- Maintain a single source of truth for vehicle master data and lifecycle status.
- Track vehicle location (home location) and location transfer history.
- Automatically compute vehicle availability for reservations (real-time and planned).
- Support pickup/delivery and return/check-in operations with standardized inspection and proof-of-handover.
- Schedule and enforce mileage-based maintenance, and automatically block availability during maintenance.
- Track damage/incident reporting, chargeable damage determination, and repair cost estimation.
- Track fuel-level policy compliance and charge customers for discrepancies.
- Provide utilization analytics (idle time, usage %) per location and globally.
- Support vehicle substitution when a promised vehicle is unavailable, and support rental extensions/early returns.
- Alert operations staff to critical conditions (geofence violation, maintenance overdue, insurance expiring).

## Problem Statement

### User pain point or business problem
The company has no existing system or prior experience to track vehicles as rental assets. Without such a system, staff cannot answer basic operational questions — is a vehicle available right now, is it due for maintenance, has it left its permitted area, is its insurance about to expire, is it clean and ready for the next customer. This creates risk of double-booking, renting out unsafe or uninsured vehicles, and inconsistent handover/damage processes.

### Key users affected
- Dispatchers/allocators who assign vehicles to reservations.
- Pickup/delivery crew who hand over and receive vehicles from customers.
- Maintenance crew who plan and execute servicing.
- Operations managers who monitor fleet utilization and alerts.
- Finance/back-office staff who need cost and charge data (transfers, damage, fuel, lost keys).

### Why it matters now
Car Management is the prerequisite capability for every other rental feature (reservation, pricing, customer management). The business is actively launching the rental line and requires this foundational tracking capability before any vehicle can be safely and reliably rented out.

## Functional Requirements

### 1. Vehicle Onboarding

**Title:** Register a new vehicle into the fleet

**Statement:** As a fleet administrator, I want to register a new vehicle with its full master data, so that the vehicle can be tracked and made available for rental.

**Requirement details:**
- Captured fields: VIN, license plate, purchase date, purchase cost, insurance details (including insurance expiry date), odometer reading, vehicle brand, model, manufacturing year, size type (small, medium), class (luxury, economy), number of seats, fuel type (gas, electric, hybrid), ownership information (owner must be the company).
- Vehicle categories are a combination of size type (small/medium) and class (economy/luxury). Example: Economy small (4-seat sedan), Economy medium (7-seat MPV/SUV), Luxury small (4-seat luxury sedan), Luxury medium (luxury MPV).
- Each vehicle must be assigned a home location upon onboarding.
- Fleet size per location/category is manually determined by business decision; the system does not auto-calculate optimal fleet size.

**Acceptance criteria:**
- Given a fleet administrator submits a new vehicle record with all required fields, When the record is saved, Then the vehicle appears in the fleet with lifecycle status "incoming".
- Given a required field is missing, When the administrator attempts to save, Then the system prevents saving until all mandatory fields are provided (exact mandatory field list to be finalized after UI design).

---

**Title:** Track vehicle lifecycle status

**Statement:** As a fleet administrator, I want each vehicle to have a staged lifecycle status, so that I can control what operations are valid for a vehicle at any point in time.

**Requirement details:**
- Lifecycle statuses: incoming, active, maintenance, decommissioning, sold.
- Transitions between statuses (including retirement/disposal) are manually triggered by staff; there is no automatic/rule-based retirement (e.g., no automatic trigger based on age, mileage, or depreciation).

**Acceptance criteria:**
- Given a vehicle is in "incoming" status, When a fleet administrator marks it ready for service, Then its status changes to "active".
- Given a vehicle is "active", When a fleet administrator manually initiates decommissioning, Then its status changes to "decommissioning" and it is no longer eligible for reservation.

### 2. Location & Inventory Management

**Title:** Assign and transfer vehicle home location

**Statement:** As a fleet administrator, I want each vehicle to have a home location with full transfer history, so that I can track where a vehicle belongs and what transfers have occurred.

**Requirement details:**
- Every vehicle belongs to exactly one "home location" at a time (no regional pooling).
- Transferring a vehicle means changing its home location; every change must be recorded in a home-location history log (previous location, new location, date/time, cost).
- Costs incurred by a transfer are charged to the company pool (i.e., not charged to any customer).

**Acceptance criteria:**
- Given a vehicle's home location is changed, When the transfer is confirmed, Then a new entry is appended to the vehicle's home-location history with the associated cost recorded against the company pool.

---

**Title:** Determine vehicle availability

**Statement:** As a dispatcher, I want to see real-time and planned vehicle availability, so that I can allocate vehicles to reservations without conflicts.

**Requirement details:**
- Real-time availability reflects vehicle state for the next 2 hours from the current time.
- Planned availability covers a window from the next day (H+1) through 30 days ahead (H+30), based on existing reservations and maintenance schedules.

**Acceptance criteria:**
- Given a vehicle has a reservation starting within the next 2 hours, When a dispatcher checks real-time availability, Then the vehicle is shown as unavailable.
- Given a vehicle has a maintenance schedule 10 days from now, When a dispatcher checks planned availability for that date, Then the vehicle is shown as unavailable on that date.

### 3. Reservation Allocation

**Title:** Automatically allocate a vehicle to a reservation

**Statement:** As a dispatcher, I want vehicles to be automatically allocated to reservations, so that manual assignment effort is minimized.

**Requirement details:**
- Allocation is automatic; no manual override is required (manual override may still be available operationally, but automatic allocation is the default and primary mechanism).
- Assignment is first-come-first-served; there is no prioritization by exact model promise vs. class guarantee.
- No overbooking is supported (zero-threshold; the target is zero double-booking incidents).

**Acceptance criteria:**
- Given multiple compatible vehicles are available in the requested class, When a reservation is created, Then the system assigns the first available vehicle matching the requested class without requiring manual selection.
- Given no vehicle of the requested class is available for the requested time window, When a reservation is attempted, Then the system does not allow overbooking and the reservation cannot be confirmed against a specific vehicle until one becomes available.

### 4. Vehicle Status & Telemetry

**Title:** Monitor vehicle GPS location and geofence compliance

**Statement:** As an operations manager, I want to monitor vehicle GPS location and receive geofence alerts, so that I can detect unauthorized vehicle movement.

**Requirement details:**
- Telemetry collected: GPS location only (no fuel level, battery %, or engine diagnostics telemetry).
- Geofencing alerts must be raised when a vehicle leaves its permitted area.
- Telemetry should update at a maximum interval of 5 minutes for operational dashboards, ideally in real time.

**Acceptance criteria:**
- Given a vehicle exits its assigned geofence boundary, When the system detects the exit, Then an alert is raised to operations staff within 5 minutes.
- Given a vehicle is operating normally, When the operational dashboard is viewed, Then the vehicle's GPS position is no more than 5 minutes stale.

### 5. Pickup & Delivery Operations

**Title:** Schedule and perform vehicle pickup/delivery

**Statement:** As a delivery crew member, I want to schedule and execute a vehicle drop-off/pickup with the customer, so that the customer can receive or return the vehicle at a convenient location.

**Requirement details:**
- Drop-off/pickup service is offered at additional cost, available daily between 06:00 and 19:00 local time.
- Route planning for delivery is performed manually (no optimization algorithm).
- Proof-of-handover artifacts required: photos, and an e-form with signature that includes timestamp and geolocation.
- Customer identity verification is required at pickup: driver's license scan and a selfie match against a national identifier or passport.
- No after-hours key drop is allowed; all pickup/drop-off must occur within the 06:00–19:00 local time window.

**Acceptance criteria:**
- Given a customer requests delivery outside 06:00–19:00 local time, When the request is submitted, Then the system rejects or requires rescheduling within the allowed window.
- Given a handover is completed, When the crew submits the e-form, Then the record includes photos, signature, timestamp, and geolocation, and the identity verification result (license + selfie match).

### 6. Return & Check-In Process

**Title:** Perform return inspection and check-in

**Statement:** As a pickup crew member, I want to perform a standardized return inspection, so that I can determine chargeable damage and fuel discrepancies consistently.

**Requirement details:**
- Standardized inspection captures: photos, damage checklist (exterior, interior, engine check, electrical instruments check), and fuel level.
- Chargeable damage is defined as visibly seen damage due to accident (e.g., visible body dent, visible torn leather seat). Engine and electrical instrument issues are considered normal wear and are not chargeable.
- The system does not auto-generate damage cases with severity scoring; determination is manual by the inspecting crew member.
- No after-hours key drop; return must occur within the 06:00–19:00 local time window.

**Acceptance criteria:**
- Given a returned vehicle has a visible body dent, When the crew completes the damage checklist, Then the case is flagged as chargeable damage.
- Given a returned vehicle has an engine fault with no visible external damage, When the crew completes the checklist, Then the case is treated as normal wear (not chargeable).

### 7. Maintenance & Service Scheduling

**Title:** Schedule mileage-based maintenance

**Statement:** As a maintenance crew member, I want maintenance to be scheduled based on mileage, so that vehicles are serviced consistently and unavailable during required servicing.

**Requirement details:**
- Maintenance is scheduled every 10,000–12,000 kilometers, based on mileage multiples of 10,000 km.
- Maintenance takes priority over reservations: if a maintenance schedule falls on day X, the vehicle is not available for reservation on that day.
- Maintenance blocks automatically remove the vehicle from availability (no manual step required to block availability).
- No predictive maintenance triggers (e.g., engine fault codes, abnormal usage) are required.

**Acceptance criteria:**
- Given a vehicle's odometer approaches a multiple of 10,000 km, When the maintenance crew creates a maintenance schedule for a date, Then the vehicle is automatically marked unavailable for that date.
- Given a vehicle has a scheduled maintenance date, When a dispatcher attempts to allocate the vehicle to a reservation on that date, Then the allocation is prevented.

### 8. Cleaning & Turnaround

**Title:** Enforce vehicle turnaround time

**Statement:** As an operations manager, I want a standard turnaround period after a return, so that vehicles are not re-rented before they are ready.

**Requirement details:**
- Standard turnaround time is 1 day after return, applied uniformly (no differentiation by vehicle class).
- Cleaning levels (basic, deep, disinfection) are not tracked.
- No SLA monitoring or overdue turnaround alerts are required.

**Acceptance criteria:**
- Given a vehicle is returned on a given day, When availability is checked for the next calendar day, Then the vehicle is marked unavailable until the turnaround period elapses.

### 9. Damage & Incident Handling

**Title:** Report and estimate vehicle damage

**Statement:** As a pickup crew member, I want to report vehicle damage found during return inspection, so that repair costs can be estimated and the vehicle's drivable status updated.

**Requirement details:**
- Damage incidents are reported by the pickup crew via a web form filled out during return inspection (not by customer self-report or telematics).
- Damage photos must support positional mapping onto a vehicle diagram.
- Repair cost is estimated using an internal price list (no integration with external body shops).
- No parts inventory linkage is required.
- Post-incident, vehicles are flagged only as "drivable" or "in-maintenance" (not available) — no intermediate "limited" state.

**Acceptance criteria:**
- Given a crew member records damage on the vehicle diagram with photos, When the form is submitted, Then a repair cost estimate is generated from the internal price list.
- Given a vehicle is flagged "in-maintenance" due to damage, When a dispatcher checks its availability, Then it is shown as not available for rent.

### 10. Fuel & Energy Management

**Title:** Enforce fuel-level return policy

**Statement:** As a pickup crew member, I want to compare the return fuel level against the delivery fuel level, so that customers are charged for any shortfall.

**Requirement details:**
- Fuel policy: the returned vehicle must have the same fuel level as at delivery ("same-to-same"). If lower, the customer is charged.
- EV charge-level targets on return are not tracked, and no EV charging scheduling/station integration is required.

**Acceptance criteria:**
- Given a vehicle was delivered with a recorded fuel level, When it is returned with a lower fuel level, Then a fuel charge is generated against the customer.

### 11. Utilization & Analytics

**Title:** Monitor vehicle utilization and idle time

**Statement:** As an operations manager, I want per-location and global utilization dashboards, so that I can identify underused vehicles.

**Requirement details:**
- Metrics tracked on a daily basis: usage and idle time.
- Dashboards must be available both per-location and globally (aggregated across all locations).
- Inefficiency is measured as consecutive idle time of 5 days in a row.

**Acceptance criteria:**
- Given a vehicle has been idle for 5 consecutive days, When the utilization dashboard is generated, Then the vehicle is flagged as an inefficiency.
- Given a manager selects a specific location, When viewing the utilization dashboard, Then metrics are scoped to that location; when viewing the global dashboard, metrics aggregate across all locations.

### 12. Vehicle Swaps & Substitutions

**Title:** Substitute a vehicle when the promised vehicle is unavailable

**Statement:** As a dispatcher, I want a defined substitution priority order, so that customers still receive a vehicle (or appropriate compensation) when their promised vehicle becomes unavailable.

**Requirement details:**
- Substitution priority: (1) offer another vehicle of the same type/class, (2) offer an upgrade to a better vehicle, (3) refund the customer.
- Customer impact history (counts of downgrades/upgrades) is not tracked.
- The system does not auto-offer compensation (e.g., discount); only the three substitution priorities above apply.

**Acceptance criteria:**
- Given a promised vehicle becomes unavailable and another vehicle of the same type/class is available, When substitution is triggered, Then that vehicle is offered first.
- Given no same-type or upgraded vehicle is available, When substitution is triggered, Then the customer is refunded.

### 13. Extensions & Early Returns

**Title:** Handle rental extension and early return

**Statement:** As a customer/dispatcher, I want extension requests to be auto-approved when possible and early returns to reposition the vehicle, so that fleet availability is maximized.

**Requirement details:**
- Extension requests are auto-approved when the vehicle is not already rebooked for the requested extension period; approval incurs base rental cost plus a daily late charge.
- If an extension request conflicts with an upcoming reservation, a warning must be raised.
- Early returns reposition the vehicle (i.e., make it available/open for new customers) rather than holding it idle.

**Acceptance criteria:**
- Given a vehicle has no upcoming reservation conflicting with the requested extension, When a customer requests an extension, Then it is auto-approved and base rental cost plus daily late charge is applied.
- Given a vehicle has an upcoming reservation that conflicts with the requested extension period, When the extension is requested, Then a warning is shown before proceeding.
- Given a customer returns a vehicle early, When the return is processed, Then the vehicle becomes available for new reservations (subject to standard turnaround and inspection).

### 14. Key & Access Management

**Title:** Track key type and lost-key charges

**Statement:** As a fleet administrator, I want to know whether a vehicle uses physical keys or smart locks, and charge customers for lost keys, so that key-related costs are recovered.

**Requirement details:**
- Vehicles may use physical keys or smart locks, depending on the vehicle.
- Chain-of-custody logging for keys is not required.
- If a key is lost, the customer is charged; the physical process of replacing the key is handled manually outside the system.

**Acceptance criteria:**
- Given a customer reports a lost key, When the incident is recorded, Then a charge is applied to the customer's account.

### 15. Workforce Task Management

**Title:** Assign post-return tasks to staff

**Statement:** As an operations manager, I want cleaning/inspection/delivery tasks to be assigned to staff based on return time, so that turnaround work is scheduled appropriately.

**Requirement details:**
- Task assignment (cleaning, inspection, delivery) is manual and performed on the same day as the vehicle's return.
- If a vehicle is returned after 15:00 local time, staff assignment occurs on the next day instead.
- Mobile app task lists, completion scanning, and task productivity metrics are not required at this time.

**Acceptance criteria:**
- Given a vehicle is returned at 14:00 local time, When tasks are assigned, Then assignment occurs the same day.
- Given a vehicle is returned at 16:00 local time, When tasks are assigned, Then assignment occurs the next day.

### 16. Alerts & Notifications

**Title:** Raise critical operational alerts

**Statement:** As an operations manager, I want to be alerted for critical fleet conditions, so that I can respond quickly.

**Requirement details:**
- Critical alerts: vehicle outside geofencing, maintenance overdue.
- Delivery channels: email, SMS, and dashboard.

**Acceptance criteria:**
- Given a vehicle exits its geofence or a maintenance schedule becomes overdue, When the condition is detected, Then an alert is sent via email, SMS, and shown on the dashboard.

### 17. Risk & Safeguards

**Title:** Prevent renting unsafe or under-insured vehicles

**Statement:** As an operations manager, I want the system to block assignment of vehicles with critical faults or expiring insurance, so that the company avoids operational and legal risk.

**Requirement details:**
- A vehicle with an active critical fault must be marked as not available for rent.
- Assignment must be blocked if the vehicle's insurance is due to expire within 1 week.

**Acceptance criteria:**
- Given a vehicle has an active critical fault, When a dispatcher attempts allocation, Then the allocation is blocked and the vehicle shows as unavailable.
- Given a vehicle's insurance expires within 7 days, When a dispatcher attempts allocation, Then the allocation is blocked.

## Non-Functional Requirements
(Leave blank)

## Dependencies & Constraints
- No usage-type differentiation (daily rental only); long-term lease and subscription pools are out of scope.
- Optimal fleet sizing per location is a manual business decision, not system-calculated.
- Vehicle retirement/disposal is manual only; no rule-based (age/mileage/depreciation) triggers.
- Vehicles are pooled by home location only; no regional pooling.
- No overbooking support of any kind.
- Telemetry is limited to GPS only; no fuel level, battery %, or engine diagnostic telemetry.
- Delivery route planning is manual; no route optimization algorithm.
- No after-hours key drop; pickup/drop-off restricted to 06:00–19:00 local time.
- No predictive maintenance; maintenance is mileage-based only.
- Cleaning level tracking and turnaround SLA monitoring are out of scope.
- No parts inventory linkage for damage repairs.
- EV charge-level tracking and charging station integration are out of scope.
- No customer-impact/downgrade-upgrade history tracking.
- No automatic compensation offers (e.g., discounts) beyond the defined substitution priority.
- No chain-of-custody logging for keys.
- No mobile app task lists, completion scanning, or task productivity metrics at this time.
- No mandated inspection/compliance scheduling, region-specific restrictions, or third-party integrations (DMS, telematics providers, repair shops, insurance adjusters) at this time.
- No AI-based damage detection or automated repositioning recommendations at this time.
- Minimum mandatory data fields will be finalized after UI design is completed.
- Audit log granularity and photo evidence retention duration are not yet defined (N/A).
- Stolen vehicle handling and wrong-location returns remain manual processes with no dedicated workflow at this time.
- Partial/draft inspection saving is out of scope at this time.

## Success Metrics
- Zero double-booking incidents across the fleet.
- Vehicle status updates (availability, GPS, geofence alerts) reflected within 5 minutes of the actual event.
- 100% of vehicles with expiring insurance (within 1 week) blocked from new allocation before expiry.
- 100% of returns with visible chargeable damage correctly flagged and charged.
- Reduce consecutive vehicle idle time occurrences (5+ days in a row) by 20% per location, per reporting period.
