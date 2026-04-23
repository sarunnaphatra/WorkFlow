---
name: interface-requirement-buzzebees
description: Expert in AdaPos+ to Buzzebees CRM integration for privilege redemption, earning, and campaign management.
---

# Buzzebees Interface Requirement Prompter

This skill specializes in gathering requirements for **Buzzebees** integration projects, focusing on privilege redemption, earning points, and campaign API.

## 1. CRM & Loyalty Landscape
- **Environment:** Sandbox / Production URLs?
- **Authentication:** OAuth2 (Client Credential) / API Key / Agency ID?
- **Terminal ID:** Does POS need to register `TerminalID` or `BranchID`?

## 2. Functional Requirements
### 2.1 Privilege Redemption (Burn)
- **Flow:** Check Privilege -> Select Campaign -> Redeem
- **Input:** Mobile Number / Card Number / QR Code?
- **Validation:** Campaign Quota / User Points / Tier?
- **Output:** Discount Amount / Free Product / Cash Voucher?

### 2.2 Earning Points
- **Trigger:** End of Bill / Real-time?
- **Calculation:** Net Amount vs Gross Amount? Exclude VAT?
- **Payload:** `TransactionID`, `Amount`, `MemberID`?

### 2.3 Void / Cancel
- **Policy:** Allow void redemption within X minutes?
- **Flow:** Auto-void on POS Void Bill?

## 3. Technical Constraints
- **Timeout:** Maximum wait time for redemption API (e.g., 5s)?
- **Offline:** Result if API is down (Allow manual discount or Block)?
- **Receipt Printing:** Requirement to print `Redemption Code` on slip?

## 4. Sequence & Data Flow Patterns
- **Reference Pattern:** `REST API` (Sync) for interactive Redemption.
- **Diagram:** Link to standard Buzzebees sequence diagram (`templates/seq-buzzebees.puml`).

## 5. Output Generation
Generate a technical prompt for the developer:

```markdown
# Buzzebees Integration Spec: [Project Name]

## Connectivity
- **Agency ID:** [Agency ID]
- **API Endpoint:** [URL]
- **Auth:** [Header Type]

## Interface Catalog

### IF-01: Check Campaign
- **Endpoint:** `GET /api/campaign/list`
- **Params:** `MemberID`
- **Display:** Show Title, Detail, Points Required

### IF-02: Redeem
- **Endpoint:** `POST /api/redeem`
- **Params:** `CampaignID`, `MemberID`, `RefCode`
- **Action:** Apply Discount via PRO-001

## Error Handling
- Timeout handling (Retry 1 time)
- Invalid Code message
```
