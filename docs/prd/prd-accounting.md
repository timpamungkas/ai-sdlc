# PRD - Accounting

## Document Information
- Product / Feature Name: Accounting 
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
The company is expanding from car sales into car rental, a brand-new business line with no prior rental experience or dedicated systems. Rental transactions introduce financial flows (booking payments, late charges, damage fees, delivery/drop-off fees, vehicle depreciation, insurance, chargebacks) that are fundamentally different from a one-time vehicle sale. Preliminary requirement gathering interviews with the Accounting role identified how the business wants to recognize revenue, track costs, reconcile payments, and report financial performance for the rental line.

### Objective
Provide the Accounting team with a dedicated, rule-based accounting capability for the car rental business so that every rental-related financial event (rental income, late charges, maintenance, fuel, depreciation, tax, refunds, chargebacks, manual adjustments) is captured accurately, reconciled against payment gateways/bank statements, and closed within the required timeline, without relying on manual spreadsheets.

### Goals
- Maintain a separate Chart of Accounts (CoA) branch dedicated to the rental business.
- Recognize rental revenue and related costs at the correct point in time (payment reconciliation).
- Track per-vehicle profitability (revenue minus cost, including static straight-line depreciation).
- Reconcile payments from both CSV bank reports (daily) and automatic online gateways (real-time).
- Support a hierarchical, threshold-based approval workflow for adjustments, refunds, and chargebacks.
- Maintain an immutable, append-only ledger with full audit trail for manual entries.
- Sync ledger data with the existing Oracle General Ledger via CSV export/import.
- Close the accounting period within T+2 days of transaction occurrence.

## Problem Statement
Accounting currently has no system to record, recognize, and reconcile financial transactions specific to car rental (e.g., base rental income, late charges, promotions, depreciation, delivery/drop-off fees, chargebacks). Without a dedicated rental CoA and automated recognition/reconciliation rules, the Accounting team would need to manually track these transactions in spreadsheets, which is error-prone, does not scale, and cannot guarantee a T+2 month-end close.

Key users affected:
- Accounting staff (posters) who record rental transactions and reconcile payments.
- Accounting supervisors, department heads, and directors (approvers) who approve adjustments, refunds, and write-offs above defined thresholds.
- Marketing supervisor / department head / director, who approve customer refunds above threshold amounts.
- Finance/Management stakeholders who consume per-vehicle profitability and cash flow reports.

This is important now because the rental business line is launching without any existing accounting process or system tailored to its recognition rules, cost structure, and reconciliation cadence, creating risk of revenue leakage, reconciliation mismatches, and delayed financial closes from day one.

## Functional Requirements

### 1. Maintain a Dedicated Rental Chart of Accounts
**As an** Accounting staff member, **I want** a separate Chart of Accounts branch dedicated to car rental, **so that** rental transactions do not mix with the existing car sales CoA and can be reported independently.

Requirement detail: The rental CoA must, at minimum, include accounts for base rental income, late charge income, promotion cost, maintenance cost (workshop), tax, fuel cost, vehicle installment, vehicle insurance, depreciation, manual adjustment (credit/debit), and chargeback. The CoA must be extensible so new accounts can be added later without restructuring existing accounts.

Acceptance Criteria:
- **Given** the rental CoA does not yet exist, **when** it is set up, **then** it contains a distinct branch separate from the car sales CoA with the minimum accounts listed above.
- **Given** the rental CoA exists, **when** Accounting needs to track a new cost or revenue type, **then** a new account can be added under the rental branch without impacting existing account balances or mappings.

### 2. Recognize Rental Revenue at Payment Reconciliation
**As an** Accounting staff member, **I want** rental revenue to be recognized only after payment reconciliation, **so that** the ledger reflects confirmed cash received rather than unconfirmed bookings.

Requirement detail: Base rental income is recognized when the booking payment is reconciled. Late charge income is recognized separately, at the point the customer returns the vehicle and the late charge payment is reconciled. No accrual is required for in-progress rentals straddling reporting periods.

Acceptance Criteria:
- **Given** a customer has completed a booking payment, **when** the payment is reconciled, **then** base rental income is recognized in the ledger.
- **Given** a customer returns a vehicle late and pays the late charge, **when** the late charge payment is reconciled, **then** late charge income is recognized in the ledger as a separate transaction from base rental income.
- **Given** a rental is still in progress at the end of a reporting period, **when** the period closes, **then** no accrual entry is created for that in-progress rental.

### 3. Account for Discounts, Early Returns, No-Shows, and Cancellations
**As an** Accounting staff member, **I want** discounts, early returns, and no-shows to be posted to defined cost accounts, **so that** the financial impact of these business scenarios is visible and correctly categorized.

Requirement detail: Discounts and promotions post to a "promotion cost" account rather than reducing revenue directly. Early returns generally do not trigger a refund; in select cases where a partial amount is returned, it is posted as a "customer retention cost". No-shows are posted as a "rental cancellation cost".

Acceptance Criteria:
- **Given** a promotion or discount is applied to a booking, **when** the transaction is recorded, **then** the discount amount is posted to the promotion cost account.
- **Given** a customer returns a vehicle early and a partial amount is granted back, **when** the exception is recorded, **then** the amount is posted to a customer retention cost account.
- **Given** a customer does not show up for a confirmed booking, **when** the no-show is recorded, **then** the amount is posted to a rental cancellation cost account.

### 4. Track Vehicle Depreciation on a Straight-Line Basis
**As an** Accounting staff member, **I want** each vehicle to depreciate on a static straight-line schedule, **so that** monthly depreciation cost is predictable and consistent across the fleet.

Requirement detail: Each vehicle depreciates over a fixed 60-month period. Monthly depreciation is calculated as the vehicle purchase price divided by 60. The same depreciation schedule is used for both tax and management reporting (no separate schedules). Assets are revalued/impaired on a yearly basis.

Acceptance Criteria:
- **Given** a vehicle is added to the fleet with a purchase price, **when** the monthly depreciation is calculated, **then** it equals the purchase price divided by 60, posted monthly to the depreciation account.
- **Given** 60 months have elapsed since a vehicle's purchase, **when** depreciation is calculated for month 61 onward, **then** no further monthly depreciation is posted for that vehicle.
- **Given** a fiscal year ends, **when** the annual asset revaluation/impairment review runs, **then** any impairment is recorded for the applicable vehicles.

### 5. Track Per-Vehicle Profitability
**As an** Accounting or Finance stakeholder, **I want** to see profitability per individual vehicle, **so that** I can evaluate the financial performance of each asset in the fleet.

Requirement detail: Per-vehicle profitability is calculated as revenue attributable to the vehicle minus all associated costs (maintenance, fuel, vehicle installment, insurance, depreciation, and other on-demand cash-basis costs) allocated at the vehicle level; overhead costs (facilities, fleet management) are allocated monthly, not per rental.

Acceptance Criteria:
- **Given** a vehicle has recorded revenue and cost transactions for a period, **when** a per-vehicle profitability report is generated, **then** it shows revenue minus cost (including allocated monthly overhead and depreciation) for that vehicle.
- **Given** overhead costs are incurred in a month, **when** they are allocated, **then** they are distributed across vehicles on a monthly basis rather than per individual rental.

### 6. Apply Rule-Based, Configurable Tax Logic
**As an** Accounting staff member, **I want** VAT to be applied via configurable rules, **so that** tax logic can be adjusted without requiring engineering changes.

Requirement detail: Only VAT applies (no other tax regimes), at a single rate with no location-based variance and no exemptions. Cross-border rentals (pickup in one region, drop-off in another) are handled by charging the customer a delivery and/or drop-off fee as an addition to the base rental, not through differentiated tax rates. Tax rules must be configurable by Accounting without engineering involvement.

Acceptance Criteria:
- **Given** a rental transaction is recorded, **when** VAT is applied, **then** the same VAT rate is used regardless of the customer's or vehicle's location.
- **Given** a rental involves pickup in one region and drop-off in another, **when** the transaction is billed, **then** a delivery and/or drop-off fee is added to the base rental charge.
- **Given** Accounting needs to change the VAT rate, **when** the change is made through configuration, **then** it applies to subsequent transactions without requiring a code deployment.

### 7. Record Auditable Manual Adjustments
**As an** Accounting staff member, **I want** every manual adjustment recorded as an immutable ledger entry with notes, **so that** there is a complete audit trail of corrections.

Requirement detail: All manual adjustments post to a "manual adjustment" account (credit or debit) with mandatory notes explaining the reason. The ledger is append-only/immutable: corrections or reversals must create a new ledger entry referencing the original entry, rather than modifying it. Each ledger entry records its creation date and the person (PIC) who made the entry/update. Invoice history versioning is not required.

Acceptance Criteria:
- **Given** Accounting needs to correct a posted transaction, **when** the correction is made, **then** a new ledger entry is created referencing the original entry, and the original entry remains unchanged.
- **Given** a manual adjustment is posted, **when** it is recorded, **then** it includes a mandatory note, is tagged to the "manual adjustment" CoA, and captures the creation date and the PIC who made it.

### 8. Reconcile Payments from CSV and Real-Time Sources
**As an** Accounting staff member, **I want** to reconcile payments from both manual bank CSV reports and automatic online gateways, **so that** booking records match actual settled funds.

Requirement detail: CSV-based bank reconciliation runs on a daily basis (excluding holidays). Online/automatic payment reconciliation happens in real time. Discrepancies (amount mismatches, delays) trigger automated email alerts.

Acceptance Criteria:
- **Given** a daily bank CSV report is uploaded, **when** it is processed, **then** matching booking records are reconciled and any mismatches generate an email alert.
- **Given** an online payment gateway settles a transaction, **when** the settlement event is received, **then** the corresponding booking record is reconciled in real time.
- **Given** a reconciliation discrepancy is detected (amount mismatch or delay), **when** it occurs, **then** an automated email alert is sent to Accounting.

### 9. Enforce Hierarchical Approval for Adjustments and Refunds
**As an** Accounting or Marketing approver, **I want** adjustments, refunds, and chargebacks to follow a threshold-based hierarchical approval flow, **so that** financial controls prevent unauthorized postings.

Requirement detail: All financial approvals follow a hierarchy based on threshold per level: supervisor → department head → director. The approver is always the direct reporting line of the poster (segregation of duties between poster and approver). Refunds above a defined threshold require approval from Marketing supervisor, Marketing department head, or Marketing director depending on the amount. Chargebacks are posted to a "chargeback" account with notes on the ledger; no automated journal entries are created for chargeback fees.

Acceptance Criteria:
- **Given** a financial adjustment exceeds a defined threshold, **when** it is submitted, **then** it routes for approval to the poster's direct reporting line at the appropriate hierarchy level.
- **Given** a refund request exceeds a defined threshold, **when** it is submitted, **then** it requires approval from the applicable Marketing supervisor, department head, or director based on the amount.
- **Given** a chargeback occurs, **when** it is recorded, **then** it is posted to the chargeback account with descriptive notes, and no automated journal entry is generated for any chargeback fee.

### 10. Sync Ledger Data with Oracle General Ledger via CSV
**As an** Accounting staff member, **I want** rental ledger data to sync with Oracle General Ledger, **so that** rental financials are reflected in the company's existing ERP.

Requirement detail: Integration is CSV-based only (no API). Payment reconciliation and its related ledger postings must sync in real time; all other transactions sync end-of-day.

Acceptance Criteria:
- **Given** payment reconciliation occurs, **when** it is completed, **then** the corresponding ledger entries are made available for real-time sync to Oracle General Ledger.
- **Given** non-reconciliation transactions are posted during the day, **when** the end-of-day sync job runs, **then** a CSV export is generated for ingestion into Oracle General Ledger.

### 11. Produce Rental-Specific Statutory and Financial Reports
**As a** Finance/Management stakeholder, **I want** rental-specific statutory and financial reports, **so that** I can monitor compliance and financial performance of the rental business.

Requirement detail: Required reports include Rental Agreement/Contract Reports, Income and Tax Reports, Vehicle Maintenance and Safety Reports, Fleet and Vehicle Availability Reports, Accident/Damage Reports, and Driver's License Verification Reports. Key financial KPIs are cash flow (revenue per day) and profit-loss per vehicle. No IFRS/local GAAP dual reporting or fleet utilization dashboard integration is required at this time.

Acceptance Criteria:
- **Given** a reporting period has closed, **when** statutory reports are generated, **then** they include Rental Agreement/Contract, Income and Tax, Vehicle Maintenance and Safety, Fleet and Vehicle Availability, Accident/Damage, and Driver's License Verification reports.
- **Given** Accounting or Finance requests financial KPIs, **when** the dashboard/report is generated, **then** it shows cash flow (revenue per day) and profit-loss per vehicle.

### 12. Retain Financial Records and Support Month-End Close within T+2
**As an** Accounting staff member, **I want** financial records retained for the required period and the books closed quickly, **so that** the business meets audit and reporting timelines.

Requirement detail: Financial records must be retained for 10 years. Transactions must be reflected in accounting no later than T+2 days after occurrence, and the accounting period must close by T+2 days.

Acceptance Criteria:
- **Given** a financial record is created, **when** the retention period is evaluated, **then** the record remains retrievable for at least 10 years.
- **Given** a transaction occurs, **when** T+2 days have passed, **then** the transaction is reflected in the accounting ledger.
- **Given** a reporting period ends, **when** T+2 days have passed, **then** the period is closed.

## Non-Functional Requirements

## Dependency & Constraints
- Single currency only: all billing and settlement is in USD; no multi-currency or FX handling is required.
- Single tax regime: VAT only; no location-based tax variance or exemptions.
- No deposit liability tracking; deposits are not held as liabilities until settlement.
- No accrual accounting for in-progress rentals, expected damages, or insurance claims; costs other than depreciation are recorded on a cash basis, on demand.
- No corporate/delayed payment terms; customers must pay before service (pay first).
- No aging (30/60/90+) reports required.
- Invoices are B2C simplified only; no itemized taxes/surcharges/environmental fees, and no versioned invoice history.
- Integration with Oracle General Ledger is CSV-based only; no API-based integration.
- No split billing, mid-rental vehicle swaps, or prepaid package unused-portion refunds are supported at this time.
- No automated compliance checklists or provisioning for doubtful accounts at this time.
- No dual IFRS/local GAAP reporting and no rolling forecasts or scenario modeling at this time.

## Success Metrics
- All accountable transactions exist in the ledger (100% capture).
- All payment reconciliations are completed with no unresolved mismatches.
- Depreciation is auto-calculated for 100% of active vehicles each month.
- Manual adjustment/correction entries represent less than 1% of total ledger entries.
- Accounting period closes by T+2 days after period end, every period.
- Reconciliation discrepancy email alerts are delivered within the same day they are detected.

## AI usage disclaimer
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
