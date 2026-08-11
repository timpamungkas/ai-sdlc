# PRD - Marketing

## Document Information
- Product / Feature Name: Marketing (Car Rental)
- Author: @copilot
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
The company is expanding from car sales into a new car rental business line. There is no prior rental experience or dedicated systems, so the marketing function needs a foundation for tracking rental performance, managing pricing by vehicle category and rental duration, ensuring customers pay on time, and controlling who can approve rented cars.

### Objective
Provide marketing with the capabilities needed to drive brand awareness and customer acquisition for the new car rental business, while giving visibility into fleet utilization, revenue, customer behavior, and on-time payment performance.

### Goals
- Achieve brand awareness and customer acquisition for the car rental business in its first 12 months.
- Track fleet utilization (car rented percentage) with a target of at least 75% per week.
- Support static pricing by vehicle category and rental duration (daily, weekly, monthly).
- Support seasonal/peak holiday pricing overlays and simulated pricing on the booking confirmation page.
- Provide configurable, role-based approval limits for rented cars to mitigate non-payment risk.
- Provide recurring reports (rented car percentage, income, new customers, repeat customers).

## Problem Statement
Marketing currently has no dedicated system or process to support the car rental business. Without it, the business cannot:
- Track whether the fleet is being rented out efficiently (utilization target of 75%+ per week).
- Segment and target the right customers (local, tourist, insurance replacement).
- Apply consistent, category-based pricing across daily/weekly/monthly rental terms, including seasonal overlays.
- Control approval of rentals to reduce the risk of customers renting or extending without paying.
- Produce the recurring performance reports marketing needs to run the business.

Key users affected: Marketing staff, supervisors, marketing department head, and marketing director, who are responsible for setting pricing, approving rentals, monitoring KPIs, and reporting on performance. This is important now because the rental business is launching without any of this tooling in place, and the primary business risk identified is customers renting or extending rentals without paying on time.

## Functional Requirements

### 1. Fleet Utilization Tracking
**Statement:** As a marketing staff member, I want to see the percentage of cars currently rented out of the total fleet, so that I can monitor whether we are meeting our utilization target.

**Requirement detail:** The system must calculate the "car rented percentage" as (number of rented cars / total number of cars) for a given period (daily, weekly, monthly). The target threshold is at least 75% per week.

**Acceptance Criteria:**
- Given the total fleet size and the number of currently rented cars, when the reporting period ends (day/week/month), then the system displays the car rented percentage for that period.
- Given the weekly car rented percentage is below 75%, when the report is generated, then the report highlights that the target was not met.

### 2. Customer Segmentation
**Statement:** As a marketing staff member, I want to classify customers into segments (local, tourist, insurance replacement), so that I can tailor acquisition and communication efforts.

**Requirement detail:** Segments are manually assigned/defined by marketing staff; no automated or rule-based segmentation is required at this stage.

**Acceptance Criteria:**
- Given a new customer record, when marketing staff review the customer, then they can manually assign the customer to one of the segments: local, tourist, or insurance replacement.
- Given a customer's segment is set, when viewing the customer record, then the assigned segment is displayed.

### 3. Booking Funnel Tracking
**Statement:** As a marketing staff member, I want to track customers through the booking funnel (search → vehicle selection → confirmation → payment), so that I can understand acquisition performance.

**Requirement detail:** The system should record which funnel stage a booking attempt has reached. Abandoned search/incomplete booking recovery is not required at this stage.

**Acceptance Criteria:**
- Given a customer begins a search, when they proceed through vehicle selection, confirmation, and payment, then each stage reached is recorded against the booking attempt.
- Given a booking attempt does not reach payment, when viewed by marketing, then the last completed stage is visible (no automated recovery action is triggered).

### 4. Static Pricing by Vehicle Category and Rental Term
**Statement:** As a marketing staff member, I want to define static daily, weekly, and monthly rates per vehicle category, so that pricing is consistent and reflects volume discounts for longer rentals.

**Requirement detail:** Vehicle categories are: small regular car, medium regular car, medium luxury car. Each category has its own daily (most expensive per-day rate), weekly (cheaper per-day rate), and monthly (cheapest per-day rate) pricing.

**Acceptance Criteria:**
- Given a vehicle category, when marketing configures pricing, then daily, weekly, and monthly rates can be set independently for that category.
- Given a customer selects a rental duration, when the price is calculated, then the applicable rate (daily, weekly, or monthly) for the vehicle's category is applied.

### 5. Seasonal/Peak Pricing Overlay
**Statement:** As a marketing staff member, I want to apply time-limited surge pricing during peak holiday seasons, so that pricing reflects periods of higher demand.

**Requirement detail:** Marketing must be able to define a date range during which an overlay adjusts the static rate for one or more vehicle categories.

**Acceptance Criteria:**
- Given a peak holiday date range and an adjusted rate, when a booking falls within that date range, then the overlay rate is applied instead of the standard static rate.
- Given a booking date is outside any configured peak period, when the price is calculated, then the standard static rate applies.

### 6. Simulated Pricing on Confirmation Page
**Statement:** As a customer, I want to see a simulated price breakdown on the booking confirmation page, so that I understand the total cost before completing payment.

**Requirement detail:** The confirmation page must display the calculated price (including any peak season overlay) prior to payment.

**Acceptance Criteria:**
- Given a customer reaches the confirmation page, when the page loads, then the simulated total price for the selected vehicle, category, and duration is displayed.
- Given a peak pricing overlay applies to the selected dates, when the confirmation page displays the price, then the overlay-adjusted price is shown.

### 7. Competitor Price Monitoring Input
**Statement:** As a marketing staff member, I want to record competitor pricing information regularly, so that I can review our pricing competitiveness.

**Requirement detail:** Competitor prices are captured as regular manual inputs; no automated competitor scraping or integration is required.

**Acceptance Criteria:**
- Given a competitor price observation, when marketing staff record it, then it is stored with the observation date and vehicle category for reference.

### 8. ID Verification Consent
**Statement:** As a customer, I want to provide consent for valid ID card validation, so that the rental company can verify my identity in compliance with data privacy requirements.

**Requirement detail:** The booking process must capture explicit customer consent for ID card validation before completing a booking.

**Acceptance Criteria:**
- Given a customer is completing a booking, when they reach the point of ID validation, then they must explicitly provide consent before the booking can proceed.
- Given consent is not given, when the customer attempts to proceed, then the booking cannot be completed.

### 9. Role-Based, Configurable Approval Limits
**Statement:** As a marketing department head, I want to configure maximum rental approval amounts per role, so that approval authority is controlled and non-payment risk is reduced.

**Requirement detail:** Roles are: staff, supervisor, marketing department head, and marketing director. Staff, supervisor, and marketing department head each have a configurable maximum approval amount (X, Y, Z respectively); the marketing director has unlimited approval authority. The specific configured values (X, Y, Z) are administrative settings, not fixed in this document.

**Acceptance Criteria:**
- Given a rental value at or below the configured limit for a role, when a user with that role attempts to approve, then the approval succeeds.
- Given a rental value above the configured limit for a role, when a user with that role attempts to approve, then the approval is rejected and must be escalated to a higher role.
- Given a marketing director attempts to approve any rental value, when they submit the approval, then it succeeds regardless of amount.
- Given an authorized administrator updates the configured limits (X, Y, Z), when the update is saved, then subsequent approval checks use the new limits.

### 10. Pricing Change Audit Trail
**Statement:** As a marketing department head, I want to see who changed a price tier and when, so that I can maintain accountability over pricing decisions.

**Requirement detail:** Every change to a price tier (static rate or seasonal overlay) must be logged with the user who made the change, the timestamp, and the previous value.

**Acceptance Criteria:**
- Given a price tier is changed, when the change is saved, then the system records who made the change, when it was made, and the previous value.
- Given a price tier's history, when viewed by an authorized user, then all prior changes are listed in chronological order.

### 11. Promotion Approval Workflow
**Statement:** As a marketing staff member, I want new promotions to require approval from the marketing department head before going live, so that promotional activity is controlled.

**Requirement detail:** A promotion (when introduced) cannot go live until approved by the marketing department head. No draft → review → publish workflow states are required beyond this single approval gate.

**Acceptance Criteria:**
- Given a new promotion is created, when it is submitted, then it cannot go live until the marketing department head approves it.
- Given the marketing department head approves a promotion, when the approval is recorded, then the promotion becomes active.

### 12. Recurring Marketing Reports
**Statement:** As a marketing staff member, I want recurring reports on rental performance, so that I can monitor business health and report to management.

**Requirement detail:** The system must produce the following reports on a daily, weekly, and monthly cadence (as applicable):
- Rented car percentage (number of rented cars in the period vs. total fleet size)
- Income report
- New customers within the period
- Customers with repeated orders

**Acceptance Criteria:**
- Given a reporting period (daily, weekly, or monthly) has ended, when the report is generated, then it includes rented car percentage, income, new customer count, and repeat customer count for that period.
- Given a user requests a report for a specific period, when the report is generated, then the figures reflect only data from that period.

## Non-Functional Requirements


## Dependency & Constraints
- Applies only to the car rental business line; not the existing car sales business.
- No pricing tiers or bundles beyond the three vehicle categories (small regular car, medium regular car, medium luxury car) are required at this stage.
- No dynamic/algorithmic pricing engine is required; pricing is static per category and rental term, with manual seasonal overlays.
- No promo codes, discounts, loyalty programs, or add-ons/upsells are required at this stage.
- No abandoned booking recovery, content personalization, A/B testing, or multivariate testing is required.
- No CRM/CDP integrations, affiliate/aggregator tracking parameters, or external inventory/pricing feeds are required at this stage.
- English language only; no multilingual support at launch.
- Historical marketing performance data must be retained for 3 years.
- Discovery channels supported at launch: web and marketplace aggregators only.

## Success Metrics
- Maintain a weekly car rented percentage of at least 75% of the total fleet.
- Achieve a positive month-over-month growth trend in new customers acquired, as tracked by the monthly "new customer" report.
- Achieve a positive month-over-month growth trend in customers with repeated rental orders, as tracked by the monthly "repeat customer" report.
- Reduce the rate of customers renting/extending rentals without paying on time to as close to 0% of active rentals as possible, as tracked via the approval workflow and payment records.
