# Test Scenario - Promotion Management

## Document Information
- Author: @copilot
- Date:
- Version:

## Table of Contents
- [Test Scenario](#test-scenario)
- [Test Case 1](#test-case-1)
- [Test Case 2](#test-case-2)
- [Test Case 3](#test-case-3)
- [Test Case 4](#test-case-4)
- [Test Case 5](#test-case-5)

## Test Scenario
- Feature to test: The promotion approval workflow requiring marketing department head sign-off before a promotion goes live, and multi-channel (email, SMS, WhatsApp) delivery of approved promotions with per-channel, per-recipient delivery status (see [PRD - Marketing](../../prd/prd-marketing.md), Functional Requirements 11 and 12).
- Preconditions:
  - Marketing staff is logged in with permission to create promotions.
  - A marketing department head account is available to approve promotions.
  - At least one recipient customer exists with contact information for one or more channels (email, SMS, WhatsApp).

## Test Case 1
- Description: A new promotion cannot go live without marketing department head approval.
- Steps:
  1. As marketing staff, create a new promotion.
  2. Submit the promotion.
  3. Attempt to send the promotion before it is approved.
- Expected result: The promotion cannot go live or be sent until it is approved by the marketing department head.

## Test Case 2
- Description: An approved promotion becomes active.
- Steps:
  1. As marketing staff, create and submit a new promotion.
  2. As the marketing department head, approve the promotion.
- Expected result: The promotion becomes active once the approval is recorded.

## Test Case 3
- Description: Sending an approved promotion through multiple selected channels.
- Steps:
  1. As marketing staff, select an approved promotion.
  2. Select the email and SMS channels for delivery.
  3. Send the promotion to a recipient who has both an email address and a phone number on file.
- Expected result: The promotion is sent to the recipient via both the email and SMS channels.

## Test Case 4
- Description: Recording independent delivery status per channel and recipient.
- Steps:
  1. As marketing staff, send an approved promotion via email and SMS to a recipient.
  2. Simulate a delivery failure on the SMS channel while email delivery succeeds.
  3. View the delivery status for that recipient.
- Expected result: The system records the email delivery as successful and the SMS delivery as failed, independently, without affecting the recorded status of the other channel.

## Test Case 5
- Description: Delivery is only attempted on channels for which the customer has valid contact information.
- Steps:
  1. As marketing staff, select an approved promotion.
  2. Select the email, SMS, and WhatsApp channels for delivery.
  3. Send the promotion to a recipient who has an email address and phone number but no WhatsApp number on file.
- Expected result: Delivery is attempted only via email and SMS; no delivery attempt is recorded for WhatsApp for that recipient.

---
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
