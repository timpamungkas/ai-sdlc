# PRD - Accounting

## Document Information
- Product / Feature Name: Accounting
- Author: copilot
- Date:
- Version:

## Overview

### Background
The company is expanding from car sales into car rental, a new business line with no prior rental experience or dedicated systems. Rental operations introduce financial flows (recurring rental income, late charges, deposits, depreciation, insurance, taxes, chargebacks, reconciliation with payment gateways and bank statements) that are not covered by the existing sales-oriented accounting setup. A dedicated Accounting capability is required to record, recognize, and report rental-related financial transactions accurately and to keep the general ledger (Oracle GL) in sync with rental operations.

### Objective
Provide the Accounting function with a reliable, rule-based way to record rental revenue, costs, taxes, adjustments, and reconciliations so that the business has accurate daily cash flow visibility, per-vehicle profitability, and an auditable, immutable ledger that closes within the required SLA.

### Goals
- Recognize rental revenue and related costs at the correct point in time (payment reconciliation).
- Maintain a separate, granular Chart of Accounts (CoA) for the rental business line.
- Track per-vehicle profitability using straight-line depreciation (60 months) and on-demand cash-basis costs.
- Reconcile payments (CSV/bank report and real-time online gateway) against bookings and surface discrepancies automatically.
- Support manual adjustments, refunds, and chargebacks with a full, immutable audit trail.
- Sync ledger entries with Oracle General Ledger within the agreed latency (real-time for payment reconciliation, end-of-day otherwise) and close the accounting period by T+2 days.

## Problem Statement

### User pain point or business problem
Without a rental-specific accounting design, the business cannot recognize rental income/costs correctly, cannot see per-vehicle profitability, cannot reconcile payments reliably across manual (CSV) and automatic (online) channels, and has no defined process for adjustments, refunds, or chargebacks with an auditable trail. This creates risk of misstated revenue, undetected payment mismatches, and an inability to close the books on time.

### Key users affected
- Accounting / Finance staff (posters of ledger entries, reconciliation, month-end close)
- Approvers in the finance hierarchy (supervisor, department head, director)
- Marketing supervisor / department head / director (refund approval)
- Third-party debt collectors (for accumulated overdue balances such as unpaid late charges)

### Why it matters now
The car rental line is new and has no dedicated accounting process. Revenue is already flowing (bookings, payments, late charges) and must be recorded correctly from day one to keep cash flow visibility, support per-vehicle profitability decisions, and meet the 10-year financial record retention and T+2 close requirements.

## Functional Requirements

### User Story 1: Separate Chart of Accounts for Rental
- Statement: As an Accounting user, I want a dedicated rental Chart of Accounts, so that rental transactions are not mixed with car sales accounts.
- Requirement details:
  - Create a separate CoA branch for rental, including at minimum: base rental income, late charge income, maintenance cost (workshop), tax, fuel cost, vehicle installment, vehicle insurance, depreciation, promotion cost, manual adjustment (credit/debit), chargeback.
  - The CoA must be extensible so new accounts can be added later without restructuring existing accounts.
- Acceptance criteria:
  - Given the rental CoA is configured, When a rental transaction is posted, Then it must post only to rental-designated accounts.
  - Given a new cost/revenue type is identified, When Accounting adds a new account to the rental CoA, Then existing postings and reports remain unaffected.

### User Story 2: Revenue Recognition at Payment Reconciliation
- Statement: As an Accounting user, I want rental revenue recognized at payment reconciliation, so that recorded revenue reflects actual confirmed payments.
- Requirement details:
  - Base rental revenue is recognized when payment reconciliation is completed after booking.
  - Late charge revenue is recognized at payment reconciliation after the customer returns the vehicle and pays the late charge.
  - No revenue accrual is required for in-progress rentals spanning reporting periods.
- Acceptance criteria:
  - Given a booking payment has been reconciled, When the reconciliation is confirmed, Then base rental revenue is recognized in the ledger.
  - Given a customer returns a vehicle and pays a late charge, When that late charge payment is reconciled, Then late charge revenue is recognized.
  - Given a rental is still in progress at period end, When the period closes, Then no revenue accrual entry is created for that rental.

### User Story 3: Early Return, No-Show, and Cancellation Handling
- Statement: As an Accounting user, I want defined financial treatment for early returns, no-shows, and cancellations, so that these events are consistently accounted for.
- Requirement details:
  - Early returns generally do not refund the customer; in select cases the difference may be posted as a customer retention cost.
  - No-shows are accounted for as a rental cancellation cost.
- Acceptance criteria:
  - Given a customer returns a vehicle early, When no refund is issued, Then no adjusting entry is created; When a retention exception is approved, Then the amount is posted to customer retention cost.
  - Given a booking results in a no-show, When it is processed, Then the associated amount is posted to rental cancellation cost.

### User Story 4: Deposits and Prepaid Insurance
- Statement: As an Accounting user, I want deposits and insurance handled per defined rules, so that liabilities and prepaid costs are represented correctly.
- Requirement details:
  - Deposits are not held as liabilities until settlement (no deposit liability account required).
  - Vehicle insurance is purchased annually per vehicle; recognition follows the annual policy basis rather than proration per rental.
- Acceptance criteria:
  - Given a deposit is collected, When it is recorded, Then it is not posted as a liability account.
  - Given an annual insurance premium is paid for a vehicle, When it is recorded, Then it follows the annual per-vehicle insurance treatment.

### User Story 5: Cost Accounting and Depreciation
- Statement: As an Accounting user, I want vehicle depreciation and other operating costs tracked consistently, so that per-vehicle profitability can be calculated.
- Requirement details:
  - Depreciation is straight-line over 60 months per vehicle (vehicle purchase price divided by 60, recorded monthly).
  - Other costs (maintenance, workshop, fuel, etc.) are recorded on demand, on a cash basis, as incurred.
  - Overhead (facilities, fleet management) is allocated monthly, not per rental.
  - Per-vehicle profitability (profit and loss per vehicle) must be derivable from recorded revenue and costs.
  - Assets are revalued/impairment-assessed on a yearly basis; the same depreciation schedule is used for both tax and management reporting (no dual schedules).
- Acceptance criteria:
  - Given a vehicle's purchase price and start date, When the monthly depreciation run occurs, Then the system posts purchase price / 60 as the monthly depreciation expense for that vehicle.
  - Given operating costs are incurred, When they are recorded, Then they are posted on a cash basis at the time incurred.
  - Given a reporting period, When per-vehicle profitability is requested, Then revenue and cost postings for that vehicle can be aggregated into a profit/loss figure.

### User Story 6: Billing, Invoicing, and Tax
- Statement: As an Accounting user, I want simplified B2C invoicing with configurable VAT-only tax rules, so that invoices are correct and consistent across locations.
- Requirement details:
  - Invoices are B2C simplified format; no itemized taxes/surcharges/environmental fees breakdown required on the invoice.
  - Single currency: USD only; no multi-currency or FX handling required.
  - Only VAT applies; tax rates do not vary by location and there are no exemptions.
  - Cross-border rentals (pickup/drop-off in different regions) are billed via a delivery and/or drop-off fee added to the base rental charge, rather than special cross-border tax logic.
  - Tax logic must be rule-based and configurable by Accounting (e.g., VAT rate configuration) without requiring a code change.
- Acceptance criteria:
  - Given a B2C rental is invoiced, When the invoice is generated, Then it uses the simplified format without itemized tax/surcharge breakdown.
  - Given a cross-border pickup/drop-off, When the booking is billed, Then delivery/drop-off fees are added to the base rental charge.
  - Given Accounting updates the VAT rate configuration, When new invoices are generated, Then they use the updated rate without a system code change.

### User Story 7: Manual Adjustments and Immutable Ledger
- Statement: As an Accounting user, I want an immutable, append-only ledger with a defined manual adjustment process, so that all corrections are fully auditable.
- Requirement details:
  - The ledger/journal is immutable; corrections must be made via new reversing/correcting entries, never by editing or deleting existing entries.
  - Manual adjustments must be posted to a dedicated "manual adjustment" (credit/debit) account with accompanying notes.
  - Audit log must capture, at minimum, the creation date and the person (PIC) who made the entry/update.
  - Financial records must be retained for 10 years.
- Acceptance criteria:
  - Given a posted ledger entry needs correction, When Accounting corrects it, Then a new journal entry is created that reverses or adjusts the original; the original entry is never modified or deleted.
  - Given a manual adjustment is posted, When it is recorded, Then it uses the "manual adjustment" CoA with a note explaining the reason.
  - Given any ledger entry, When it is queried, Then its creation date and the PIC who created/updated it are available.

### User Story 8: Payment Reconciliation
- Statement: As an Accounting user, I want to reconcile both manual (CSV bank report) and automatic (online gateway) payments against bookings, so that all payments are matched and discrepancies are flagged.
- Requirement details:
  - Manual reconciliation: bank report ingested via CSV, processed daily (excluding holidays).
  - Automatic reconciliation: online gateway payments processed in real time.
  - Discrepancies (amount mismatches, delays) must trigger automated email alerts.
- Acceptance criteria:
  - Given a daily bank CSV report is uploaded, When it is processed, Then matched bookings are reconciled and any mismatches are flagged.
  - Given an online gateway payment is received, When it is processed, Then it is reconciled against the booking in real time.
  - Given a reconciliation discrepancy is detected, When it occurs, Then an automated email alert is sent.

### User Story 9: Outstanding Balances, Refunds, and Chargebacks
- Statement: As an Accounting user, I want defined rules for outstanding balances, refund approvals, and chargebacks, so that these exceptions are handled consistently and with proper authorization.
- Requirement details:
  - Customers pay upfront; delayed payment is not allowed. There should be no standard overdue AR; accumulated balances (e.g., unpaid late charges) may be escalated to a third-party debt collector.
  - No aging report (30/60/90+) is required.
  - Refunds above threshold require approval from Marketing supervisor, marketing department head, or marketing director (per hierarchical threshold).
  - Chargebacks are posted to a dedicated "chargeback" account with notes on the ledger; no automated journal entries for chargeback fees are required.
- Acceptance criteria:
  - Given accumulated unpaid late charges exist, When they meet the escalation criteria, Then they are referred to a third-party debt collector.
  - Given a refund exceeds the defined threshold, When it is requested, Then it requires approval from the appropriate marketing role based on the threshold level.
  - Given a chargeback occurs, When it is recorded, Then it is posted to the "chargeback" account with explanatory notes.

### User Story 10: Approval Hierarchy and Internal Controls
- Statement: As an Accounting user, I want financial approvals to follow the reporting hierarchy, so that postings and adjustments are properly authorized.
- Requirement details:
  - All financial approvals follow a hierarchical threshold model: supervisor → department head → director.
  - The approver for any posting must be the poster's direct reporting line manager.
  - Roles must distinguish between posters and approvers.
- Acceptance criteria:
  - Given a posting requires approval, When it is submitted, Then it is routed to the poster's direct reporting-line approver appropriate to the transaction's threshold level.
  - Given a transaction exceeds a given threshold, When it is submitted for approval, Then it escalates to the next hierarchy level as configured.

### User Story 11: Oracle GL Integration
- Statement: As an Accounting user, I want rental ledger data synced with Oracle General Ledger, so that rental financials are consolidated with company-wide accounting.
- Requirement details:
  - Integration target: Oracle General Ledger.
  - Sync latency: real-time for payment reconciliation-related ledger entries; end-of-day for all other transactions.
  - Data exchange method: CSV file-based ingestion/export only (no API-based integration required).
- Acceptance criteria:
  - Given a payment reconciliation posts a ledger entry, When it is posted, Then it is synced to Oracle GL in real time.
  - Given non-reconciliation transactions occur during the day, When end-of-day processing runs, Then they are exported as CSV and synced to Oracle GL.

### User Story 12: Financial Reporting and KPIs
- Statement: As an Accounting user, I want core rental financial reports and KPIs, so that I can monitor cash flow and profitability.
- Requirement details:
  - Required KPIs: daily cash flow (revenue per day) and profit/loss per vehicle.
  - Required statutory/operational reports: Rental Agreement/Contract Reports, Income and Tax Reports, Vehicle Maintenance and Safety Reports, Fleet and Vehicle Availability Reports, Accident/Damage Reports, Driver's License Verification Reports.
  - No IFRS vs. local GAAP dual reporting is required.
  - Fleet utilization does not need to be integrated with the financial dashboard.
- Acceptance criteria:
  - Given a reporting period, When the daily cash flow report is generated, Then it reflects reconciled revenue for that day.
  - Given a reporting period, When the per-vehicle P&L report is generated, Then it reflects revenue and allocated costs (including depreciation) for each vehicle.

### User Story 13: Month-End Close SLA
- Statement: As an Accounting user, I want transactions reflected in accounting within a defined SLA, so that the period can be closed on time.
- Requirement details:
  - Maximum delay between a transaction occurring and its reflection in accounting: T+2 days.
  - Period close target: T+2 days after period end.
  - Zero-friction close is defined as: all accountable transactions exist in the ledger, all payment reconciliations are complete with no mismatches, accruals/depreciation are auto-calculated, manual adjustments represent less than 1% of total ledger entries, and the close happens by end of month.
- Acceptance criteria:
  - Given a transaction occurs, When T+2 days elapse, Then the transaction must already be reflected in the accounting ledger.
  - Given the period-end close process runs, When it completes, Then it finishes within T+2 days of period end, with manual adjustments under 1% of total ledger entries for that period.

## Non-Functional Requirements

## Dependencies & Constraints
- Single currency (USD) only; no multi-currency or FX rate handling.
- Single tax regime (VAT) only; no location-based tax variation or exemptions.
- No deposit liability accounting; no invoice versioning history.
- No accrual accounting for in-progress rentals, expected damages, or insurance claims (cash basis only).
- No dual IFRS/local GAAP reporting; no separate tax vs. management depreciation schedules.
- No corporate delayed-payment/AR aging workflow (pay-first model); no automated compliance checklists.
- No split billing, mid-rental vehicle swap re-invoicing, or prepaid package proration in current scope.
- File-based (CSV) integration only with Oracle General Ledger; no API-based integration.
- Depends on the existing Oracle General Ledger system for consolidated financial reporting.
- Depends on payment gateway (online) and bank CSV report availability for reconciliation.

## Success Metrics
- Daily cash flow (revenue per day) is visible and accurate to reconciled payments.
- Per-vehicle profit/loss is available for 100% of active fleet vehicles.
- Payment reconciliation discrepancies are detected and alerted on the same day they occur.
- Manual adjustment/correction entries represent less than 1% of total ledger entries per period.
- Month-end close completed within T+2 days after period end.
- 100% of ledger entries retained and auditable for the required 10-year retention period.
