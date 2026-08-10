# Product Requirements Document: Accounting

## 1. Overview

### Purpose

Provide accounting capabilities for the car-rental business that maintain accurate,
immutable financial records, reconcile rental payments, calculate depreciation, and
produce operational and financial reports. The product integrates with Oracle General
Ledger through CSV exports.

### Business Objectives

- Maintain positive daily cash flow through visibility into revenue per day.
- Measure profitability as revenue less cost, including profit and loss per vehicle.
- Ensure every accountable transaction is represented in the ledger.
- Close accounting periods within T+2 days.

### Scope

The initial release supports a separate rental chart of accounts (CoA), USD
transactions, VAT, payment reconciliation, per-vehicle accounting, financial
approvals, and end-of-day Oracle GL exports.

### Out of Scope

- Multi-currency and foreign-exchange accounting.
- In-progress rental accruals and expected-damage or insurance-claim provisions.
- Corporate credit terms, split billing, aging reports, and doubtful-account provisions.
- Mid-rental vehicle swaps, unused prepaid-package refunds, forecasts, and scenario
  modeling.
- Automated compliance checklists and fleet-utilization financial dashboards.

## 2. Users and Roles

| Role | Responsibilities |
| --- | --- |
| Accounting poster | Creates financial entries and submits manual adjustments for approval. |
| Approver | The poster's direct reporting line; approves transactions within the applicable threshold. |
| Supervisor, department head, director | Provide hierarchical approval according to configured financial thresholds. |
| Marketing supervisor, department head, director | Approve refunds above the applicable threshold. |

Sensitive cost and margin access restrictions are not required in the initial release.

## 3. Functional Requirements

### 3.1 Chart of Accounts and Ledger

1. The system shall maintain a rental-specific CoA, extensible with additional accounts.
2. The initial CoA shall include base rental income, late-charge income, maintenance
   cost (workshop), tax, fuel cost, vehicle installment, vehicle insurance,
   depreciation, promotion cost, rental cancellation cost, customer retention cost,
   chargeback, and debit/credit manual adjustment accounts.
3. The system shall use separate CoA accounts for late-return, damage, and cleaning
   fees.
4. The ledger shall be immutable and append-only. A correction or reversal shall create
   a new entry referencing the corrected or reversed entry; it shall not alter the
   original entry.
5. Manual adjustments shall be posted as manual-entry ledger records with notes,
   creation date, and the person in charge (PIC) who updated the record.
6. All financial approvals shall follow configured hierarchical thresholds from
   supervisor through department head to director. An approver shall be the direct
   reporting line of the poster.

### 3.2 Revenue, Costs, and Assets

1. Revenue shall be segmented and reportable by location.
2. Base rental revenue shall be recognized when the booking payment is reconciled.
3. Late-charge revenue shall be recognized when the returned vehicle's late charge is
   paid and reconciled.
4. Customers shall pay before rental; the initial release shall not support deposits as
   liabilities or delayed payment.
5. Early returns shall not normally generate refunds. An approved exception shall be
   recorded as customer retention cost.
6. No-shows shall be recorded as rental cancellation cost.
7. Discounts and promotions shall be recorded as promotion cost, not contra-revenue.
8. Delivery and drop-off charges shall be added to the base rental for cross-region
   rentals.
9. Vehicle depreciation shall use straight-line depreciation over 60 months: vehicle
   purchase price divided by 60 each month.
10. The system shall use a single depreciation schedule for tax and management
    reporting, support annual asset revaluation or impairment, and calculate monthly
    overhead allocation.
11. Maintenance, cleaning, fuel, and other non-depreciation costs shall be recognized
    on demand using cash basis.
12. The system shall provide per-vehicle profitability.

### 3.3 Invoicing and Tax

1. The system shall generate simplified B2C invoices in USD.
2. Invoices shall not itemize taxes, surcharges, or environmental fees.
3. The system shall calculate VAT only. VAT rules and rates shall be configurable by
   Accounting and shall not vary by location in the initial release.
4. The system shall not maintain versioned invoice history.

### 3.4 Payment Reconciliation and Exceptions

1. The system shall reconcile online payment settlements to bookings in real time.
2. The system shall support daily manual reconciliation from bank-report CSV files,
   excluding holidays.
3. The system shall send email alerts for reconciliation amount mismatches and delays.
4. Payment-reconciliation ledger entries shall be available to Oracle GL in real time;
   other entries shall be exported at end of day.
5. Chargebacks shall be recorded in the chargeback CoA with notes. Chargeback fee
   journal entries shall not be automated.
6. Accumulated customer balances, such as unpaid late charges, may be referred to a
   third-party debt collector.

### 3.5 Oracle GL Integration

1. The system shall export accounting data to Oracle General Ledger using CSV files
   only.
2. CSV exports shall include sufficient CoA mapping, transaction, date, location,
   vehicle, debit/credit, and ledger-reference data for Oracle GL posting and
   reconciliation.
3. The system shall meet a maximum T+2-day reflection delay for accounting
   transactions other than real-time payment reconciliation.

### 3.6 Reporting and Retention

1. The system shall provide daily cash-flow (revenue-per-day) reporting and
   per-vehicle profit-and-loss reporting.
2. The system shall produce Rental Agreement/Contract, Income and Tax, Vehicle
   Maintenance and Safety, Fleet and Vehicle Availability, Accident/Damage, and
   Driver's License Verification reports.
3. Financial records and audit data shall be retained for 10 years.
4. The monthly close shall confirm that all accountable transactions are in the ledger,
   payment reconciliation is complete with no mismatches, depreciation is calculated,
   and manual adjustment or correction entries are less than 1% of all ledger entries.

## 4. Acceptance Criteria

- A reconciled booking payment creates base-rental revenue in the rental CoA; a
  reconciled late payment creates late-charge revenue in its separate CoA.
- Monthly depreciation for a USD 60,000 vehicle is USD 1,000 and appears in that
  vehicle's profitability report.
- A manual correction leaves the original ledger entry unchanged and creates a new,
  noted correcting or reversing entry with date and PIC.
- A bank CSV can be reconciled on a business day, and a mismatch produces an email
  alert.
- Oracle GL CSV exports contain the required mapped accounting records, with
  reconciliation records available in real time and other records at end of day.
- A period can be closed within T+2 days only after the required close controls pass.

## 5. Assumptions and Open Items

- Approval thresholds, VAT rates, holiday calendars, Oracle GL CSV layout, and email
  recipients require configuration before implementation.
- The definition of an approved early-return exception and refund thresholds requires
  confirmation from Marketing.
- The initial scope does not require automated control checks for negative revenue or
  duplicate invoices.
