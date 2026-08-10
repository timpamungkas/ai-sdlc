# Product Requirement Document (PRD) — Marketing (Car Rental System)

## 1. Document Purpose
This PRD defines the product requirements for the Marketing function of the new Car Rental System. It is derived from the "Marketing Role – Preliminary Requirement Gathering Questions (Car Rental System)" interview and translates the answers into scoped requirements for the initial (launch) release, along with explicitly deferred items for future consideration.

## 2. Background
The business is expanding from car sales into car rental. Marketing needs to support brand awareness, customer acquisition, and operational visibility into rental utilization and revenue, while a lightweight approval workflow governs how staff can approve rentals.

## 3. Goals & Success Metrics

### 3.1 Primary Objectives (first 12 months)
- Brand awareness
- Customer acquisition

### 3.2 Definition of a Successful Rental Customer
A customer is considered "successful" when they:
1. Rent a car,
2. Pay on time (primary concern), and
3. Return the rented car on time.

Extending the rental period is a positive secondary signal but not required for success.

### 3.3 Key Performance Indicators (KPIs)
- **Car Rented Percentage**: number of rented cars in a week ÷ total fleet size. Target: **≥ 75%**.
  - Must be reportable **daily, weekly, and monthly**.
- Income report (daily/weekly/monthly).
- New customers acquired within a month.
- Customers with repeated orders (repeat bookings).

No rental-specific funnel KPIs (e.g., booking conversion rate, promotion redemption rate, abandoned reservation funnel) are required at launch.

## 4. Customer Segmentation & Targeting

### 4.1 Segments (launch)
- Local
- Tourist
- Insurance replacement

### 4.2 Segmentation Rules
- No differentiated pricing tiers or bundles per segment at launch.
- No specific demographic/behavioral/usage attributes are required to be captured for segmentation at launch.
- Segments are defined and reassessed **manually** (no rules engine or ML-driven segmentation at launch).

## 5. Customer Journey & Touchpoints

### 5.1 Discovery Channels
- Web
- Marketplace aggregator(s)

(Mobile app, partner portals, walk-in, and call center are out of scope for launch.)

### 5.2 Booking Funnel (to be instrumented)
The following steps must be trackable:

```
Search → Vehicle Selection → Confirmation → Payment
```

Note this differs from a typical funnel in that **Confirmation precedes Payment**.

### 5.3 Out of Scope for Launch
- Abandoned search / incomplete booking tracking and recovery.
- On-site content personalization (recommended vehicles, dynamic banners).

## 6. Promotions & Campaign Management
Not required for launch. No promo codes, discounts, eligibility rules, stackability/exclusivity rules, A/B testing, campaign ROI attribution, or advance campaign scheduling are in scope. These may be revisited in a future phase.

## 7. Pricing & Yield

### 7.1 Pricing Model
- **Static rate card** by vehicle category:
  - Small regular car
  - Medium regular car
  - Medium luxury car
- Each category has three rate tiers, with per-day cost decreasing as commitment length increases:
  - **Daily rate** (most expensive per day)
  - **Weekly rate** (cheaper per-day equivalent than daily)
  - **Monthly rate** (cheapest per-day equivalent)

### 7.2 Seasonal / Peak Pricing
- The system must support **time-limited surge/seasonal pricing overlays**, applied during peak holiday seasons.

### 7.3 Competitor Pricing
- Regular (periodic) competitor price monitoring should be supported as an input for the marketing/pricing team; this is informational and does not imply automated competitor-based repricing at launch.

### 7.4 Pricing Simulation
- The system must allow **simulated pricing scenarios on the confirmation page** before a booking is finalized, so the customer/staff can preview the cost impact of different rate tiers (daily/weekly/monthly) before committing.

### 7.5 Out of Scope for Launch
- Marketing does not control real-time dynamic pricing execution beyond the static tiers and seasonal overlays described above.
- Granular pricing by location, hour of day, or booking lead time is not required.

## 8. Cross-Sell / Upsell & Add-Ons
Not required for launch. No add-ons (GPS, child seat, insurance tiers, roadside assistance, premium cleaning, extra driver), upsell ranking, or contextual trigger offers are in scope.

## 9. Loyalty & Retention
Not required for launch. No loyalty program, points system, retroactive crediting, or integration with car sales customer IDs is in scope.

## 10. Content & Brand
Not required for launch. No content versioning, multilingual support (English only), or brand-guideline-driven UI theming requirements beyond standard branding are in scope.

## 11. Data & Analytics
Detailed analytics tooling, real-time vs. batch requirements, guaranteed-delivery events, and cohort/lifecycle analysis are not defined for launch beyond the reporting requirements in Section 13.

## 12. Compliance & Governance

### 12.1 Consent
- Customer consent is required for **valid ID card validation** as part of the rental process.

### 12.2 Other Compliance
- No additional marketing-specific privacy/consent flows (e.g., unsubscribe propagation, geographic advertising compliance) are required at launch.

## 13. Reporting Requirements

The following reports are mandatory, at **daily, weekly, and monthly** cadence where noted:

| Report | Cadence |
|---|---|
| Car Rented Percentage (rented cars ÷ total fleet) | Daily / Weekly / Monthly |
| Income report | Daily / Weekly / Monthly |
| New customers | Monthly |
| Customers with repeated orders | Monthly (or as needed) |

Automated anomaly alerts (e.g., sudden drop in conversions, spike in cancellations) are not required at launch.

## 14. Workflow & Approvals

### 14.1 Promotion/Pricing Approval
- New promotions (when introduced in future) are approved by the **Marketing Department Head**.
- No draft → review → publish workflow states are required at launch.

### 14.2 Audit History
- The system must record **who changed price tiers**, when, and the previous values (audit trail for pricing tier changes).

### 14.3 Roles & Permissions
Marketing roles, from lowest to highest privilege:
1. **Staff**
2. **Supervisor**
3. **Marketing Department Head**
4. **Marketing Director**

**Rental approval limits** are role-based and **configurable**:
- Staff can approve rentals up to a configurable limit **X**.
- Supervisor can approve rentals up to a configurable limit **Y**.
- Marketing Department Head can approve rentals up to a configurable limit **Z**.
- Marketing Director has **unlimited** approval authority.

X, Y, and Z must be configurable values (not hard-coded), and X < Y < Z is the expected ordering, though the system should not assume a fixed relationship — it should simply enforce each role's configured ceiling.

Environment separation (sandbox vs. production for campaign testing) is not required at launch.

## 15. Risks & Concerns
- **Primary risk**: customers who rent or extend a rental but do not pay (payment default / non-payment risk). This is the top concern driving the "pay on time" success definition (Section 3.2) and the approval-limit workflow (Section 14.3).

## 16. Data Retention
- Marketing performance data must be retained for **3 years**.

## 17. Out of Scope / Future Expansion
The following are explicitly **not required** for the initial launch and may be considered in future phases:
- Promotion & campaign management (codes, discounts, A/B testing, ROI attribution, scheduling).
- Upsell/cross-sell add-ons and contextual offer triggers.
- Loyalty and retention programs.
- Multilingual content and content versioning workflows.
- Abandoned booking recovery and on-site personalization.
- Subscription-based rentals, car-sharing, or bundled future services (chauffeur, EV charging).
- Automated anomaly alerting and advanced analytics/cohort tooling.
- Campaign kill-switch and customer feedback loop integration into promotions.

## 18. Open Questions
- None identified beyond the deferred items above; no additional go-live blockers were raised during interview (Q60).
