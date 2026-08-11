# PRD - Marketing

## Document Information

| Field | Value |
| --- | --- |
| Product / Feature Name | Marketing |
| Author | @copilot |
| Date | |
| Version | |

## Table of Contents

- [Document Information](#document-information)
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Functional Requirements](#functional-requirements)
  - [Booking Funnel and Segmentation](#booking-funnel-and-segmentation)
  - [Rental Pricing](#rental-pricing)
  - [Payment and Rental Approval](#payment-and-rental-approval)
  - [Reporting](#reporting)
  - [Access Control and Audit](#access-control-and-audit)
  - [Consent](#consent)
- [Non-Functional Requirements](#non-functional-requirements)
- [Dependencies & Constraints](#dependencies--constraints)
- [Success Metrics](#success-metrics)

## Overview

**Background:** The company is launching car rental as a new business line and needs marketing capabilities that support brand awareness, customer acquisition, and controlled rental operations.

**Objective:** Give marketing staff the information and controls needed to manage initial rental pricing, approve rentals within delegated limits, and measure acquisition and fleet utilization.

**Goals:**

- Support rental discovery through the web and marketplace aggregators.
- Achieve at least 75% rented-car percentage.
- Help customers complete rentals with on-time payment and return; enable paid extensions where available.
- Provide daily, weekly, and monthly marketing reports for income, utilization, new customers, and repeat customers.

## Problem Statement

The new rental business has no dedicated marketing system. Marketing needs a consistent way to present and price the initial rental offer, monitor customer acquisition and utilization, and control rental approvals. Without these capabilities, the business cannot measure whether awareness and acquisition efforts result in paid rentals, and incorrect or unaudited pricing changes can reduce revenue. Unpaid rentals or extensions are the primary operational risk.

The key users are prospective local customers, tourists, insurance-replacement customers, marketing staff, supervisors, the marketing department head, and the marketing director. This is needed for go-live so the business can acquire customers while maintaining payment-focused approval controls and reporting.

## Functional Requirements

### Booking Funnel and Segmentation

#### User Story: Record booking funnel progression

**Statement:** **As a** marketing analyst, **I want** rental funnel events recorded from search through payment, **so that** I can understand completed customer acquisition journeys.

**Requirement details:**

- Record the sequence: search, vehicle selection, confirmation, and payment.
- Identify the discovery channel as web or marketplace aggregator when it is available.
- Treat a paid booking as the minimum successful rental customer outcome; on-time return and paid extension are better outcomes.

**Acceptance criteria:**

- **Given** a customer searches for a rental, **when** they select a vehicle, confirm, and pay, **then** the system records each completed funnel stage in that order.
- **Given** a booking originates from a supported discovery channel, **when** its funnel activity is recorded, **then** the channel is associated with that activity.

#### User Story: Maintain customer segments manually

**Statement:** **As a** marketing staff member, **I want** to manually classify rental customers, **so that** I can assess the intended local, tourist, and insurance-replacement segments.

**Requirement details:**

- Provide the segment values Local, Tourist, and Insurance Replacement.
- Allow authorized marketing users to assign or update a customer’s segment manually.
- No demographic, behavioral, or automated segmentation rules are required at launch.

**Acceptance criteria:**

- **Given** an authorized marketing user views a customer, **when** they assign one of the launch segments, **then** the customer is saved with that segment.
- **Given** no segment has been assigned, **when** a customer is viewed, **then** the system does not infer a segment automatically.

### Rental Pricing

#### User Story: Manage static category rates

**Statement:** **As a** marketing user authorized to change prices, **I want** to maintain static rental rates by vehicle category and duration, **so that** customers receive the intended rental price.

**Requirement details:**

- Support the categories Small Regular Car, Medium Regular Car, and Medium Luxury Car.
- Maintain daily, weekly, and monthly rates for each category.
- The weekly daily-equivalent rate must be lower than the daily rate, and the monthly daily-equivalent rate must be lower than the weekly daily-equivalent rate.
- Dynamic pricing is not required at launch.

**Acceptance criteria:**

- **Given** rates have been configured for a vehicle category, **when** a customer selects a daily, weekly, or monthly rental, **then** the corresponding static rate is used.
- **Given** an authorized user enters duration rates whose daily-equivalent order is invalid, **when** they try to save, **then** the system prevents the change and explains the required order.

#### User Story: Apply peak-holiday seasonal pricing

**Statement:** **As a** marketing user authorized to change prices, **I want** to configure peak-holiday seasonal pricing, **so that** static rates can reflect seasonal demand.

**Requirement details:**

- Allow an authorized user to define a time-limited peak-holiday pricing overlay for a vehicle category and rental duration.
- Use the active seasonal rate for rentals whose applicable rental dates fall within the configured peak-holiday period.

**Acceptance criteria:**

- **Given** an active peak-holiday rate applies to a selected category and duration, **when** a customer confirms an applicable rental, **then** the seasonal rate is used.
- **Given** no active peak-holiday rate applies, **when** a customer confirms a rental, **then** the configured static rate is used.

#### User Story: Show price simulation at confirmation

**Statement:** **As a** prospective renter, **I want** to see the calculated rental price on the confirmation page, **so that** I can verify the price before payment.

**Requirement details:**

- Display the selected vehicle category, rental duration, applicable rate, and calculated rental price on the confirmation page.
- Reflect any applicable peak-holiday seasonal rate in the simulated price.

**Acceptance criteria:**

- **Given** I have selected a vehicle category and rental duration, **when** I open the confirmation page, **then** I see the calculated price and the rate used.

### Payment and Rental Approval

#### User Story: Protect rental and extension approval with payment status

**Statement:** **As a** rental approver, **I want** to see payment status before approving a rental or extension, **so that** I do not approve unpaid rental commitments.

**Requirement details:**

- Present the current payment status for each rental and requested extension to the approver.
- Require payment to be on time before an approval can be completed.
- A paid extension may be approved subject to the approver’s delegated limit.

**Acceptance criteria:**

- **Given** a rental or extension has an unpaid or overdue payment, **when** an approver attempts to approve it, **then** the system blocks approval.
- **Given** a rental or extension payment is on time and the approver has sufficient authority, **when** the approver approves it, **then** the approval is recorded.

### Reporting

#### User Story: View required marketing reports

**Statement:** **As a** marketing user, **I want** daily, weekly, and monthly rental reports, **so that** I can monitor utilization, income, and customer acquisition.

**Requirement details:**

- Provide reporting periods of daily, weekly, and monthly.
- Report rented-car percentage as the number of rented cars in the period divided by the total available fleet for the period.
- Report income, new customers, and customers with repeat orders for the selected period.
- Retain marketing performance data for three years.

**Acceptance criteria:**

- **Given** a marketing user selects a daily, weekly, or monthly period, **when** they view the report, **then** it shows rented-car percentage and income for that period.
- **Given** a marketing user selects a monthly period, **when** they view the report, **then** it shows new customers and customers with repeat orders for that month.
- **Given** a report needs data within the prior three years, **when** the user selects that period, **then** the data is available.

### Access Control and Audit

#### User Story: Enforce delegated rental approval limits

**Statement:** **As a** marketing department head, **I want** configurable approval limits by role, **so that** rental approvals follow delegated authority.

**Requirement details:**

- Support the roles Staff, Supervisor, Marketing Department Head, and Marketing Director.
- Configure the maximum rental approval amount for Staff, Supervisor, and Marketing Department Head (X, Y, and Z respectively).
- Allow the Marketing Director to approve rentals without an amount limit.

**Acceptance criteria:**

- **Given** a staff, supervisor, or marketing department head approval limit is configured, **when** that user attempts to approve an amount above their limit, **then** the system blocks the approval.
- **Given** a marketing director approves a rental, **when** the rental amount exceeds the configured role limits, **then** the system allows the approval.

#### User Story: Audit price-tier changes

**Statement:** **As a** marketing department head, **I want** a history of price-tier changes, **so that** I can identify who changed a price and what changed.

**Requirement details:**

- Record the user, timestamp, affected category and duration, previous value, and new value for every price-tier change.
- Make the audit history available to authorized users.

**Acceptance criteria:**

- **Given** an authorized user changes a price tier, **when** the change is saved, **then** an audit entry records the user, time, previous value, and new value.
- **Given** an authorized user reviews price-tier history, **when** they select a price tier, **then** they can see its recorded changes.

### Consent

#### User Story: Capture ID-validation consent

**Statement:** **As a** prospective renter, **I want** to provide consent for ID-card validation, **so that** my identity can be checked for the rental.

**Requirement details:**

- Request consent before performing ID-card validation.
- Do not perform ID-card validation if consent has not been provided.

**Acceptance criteria:**

- **Given** I have not provided ID-validation consent, **when** I continue with a rental requiring validation, **then** the system requests my consent and does not validate my ID until I provide it.
- **Given** I provide ID-validation consent, **when** ID validation is required, **then** the system may proceed with validation.

## Non-Functional Requirements

## Dependencies & Constraints

- Launch scope is English only.
- Supported rental discovery channels are web and marketplace aggregators; inventory or pricing feeds and affiliate tracking integrations are not required at launch.
- Pricing is static by vehicle category and duration, with only peak-holiday seasonal overlays; promotions, promo codes, bundles, add-ons, loyalty, dynamic pricing, pricing experiments, and personalized content are out of scope.
- Abandoned-search recovery, campaign management, campaign ROI attribution, analytics-tool integration, automated anomaly alerts, and automated segmentation are out of scope.
- Customer rental and extension approval requires an on-time payment status and is subject to configured approval limits.
- No draft, review, or publish workflow is required for price changes or other marketing changes at launch.

## Success Metrics

- Maintain rented-car percentage of at least 75%, measured daily, weekly, and monthly.
- Report daily, weekly, and monthly rental income.
- Report the number of new customers each month.
- Report the number of customers with repeat orders for each reporting period.
- Track paid bookings and on-time returns as successful rental outcomes; track paid rental extensions as an enhanced outcome.
