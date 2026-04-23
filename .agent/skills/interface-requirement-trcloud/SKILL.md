---
name: interface-requirement-trcloud
description: Generates a comprehensive prompt for implementing AdaPos+ Interface projects specifically for TRCloud (Cloud ERP).
---

# TRCloud Interface Requirement Prompter

This skill specializes in gathering requirements for **TRCloud** integration projects. It focuses on RESTful API, JSON payloads, and Token-based authentication strategies.

## 1. TRCloud Account & API Setup
- **Account Type:** Sandbox / Production?
- **API Version:** v1 / v2?
- **Authentication:** OAuth2 (Client ID/Secret) or API Key (Bearer Token)?
- **Callback URL:** Do we need to register a callback for webhooks (e.g., Order Status updates)?

## 2. Master Data (Inbound: TRCloud -> POS)
### 2.1 Products & Services
- **Endpoint:** `GET /products`
- **Sync Frequency:** Real-time (Webhook) or Scheduled Polling?
- **Filtering:** By Category, Tag, or Updated Date?

### 2.2 Pricing
- **Base Price:** Standard `price` field?
- **Price Tiers:** Does TRCloud handle price tiers (Wholesale/Retail)?

## 3. Inventory (Bi-Directional)
- **Stock Level:** `GET /inventory` (Real-time check before sale?)
- **Stock Movement:** `POST /inventory-adjustments` (For waste/audit)?

## 4. Sales Processing (Outbound: POS -> TRCloud)
### 4.1 Sales Invoices
- **Endpoint:** `POST /invoices` (Tax Invoice) or `POST /receipts` (Cash Receipt)?
- **Format:** JSON structure requirements.
- **Reference:** Linking POS Receipt No. to TRCloud Reference No.

### 4.2 Payments
- **Mapping:** Map POS Payment Method (Cash, Credit, QR) to TRCloud Chart of Accounts (GL Codes).
- **Status:** Unpaid vs Paid (Immediate settlement)?

## 5. Technical Integration
- **Error Handling:** How to handle 429 (Rate Limit) or 5xx errors?
- **Retry Logic:** Exponential backoff strategy?
- **Logging:** Required detail level for API logs?

## 6. Sequence & Data Flow Patterns
- **Reference Pattern:** `REST API` (Sync) vs `Webhook` (Event).
- **Diagram:** Link to standard REST sequence diagram (`templates/seq-trcloud-rest.puml`).

## 7. Output Generation
Generate a technical prompt for the developer:

```markdown
# TRCloud Integration Spec: [Project Name]

## Connectivity
- **Base URL:** [https://api.trcloud.co/...]
- **Auth:** [OAuth2 / API Key]
- **Rate Limit:** [Requests/min]

## Interface Catalog

### IF-01: Product Sync (TRCloud -> POS)
- **Endpoint:** `GET /api/v2/products`
- **Trigger:** Scheduled (Every 1 hr)
- **Mapping:**
    - `id` -> `ProductID`
    - `code` -> `Barcode`
    - `sell_price` -> `Price`

### IF-02: Sales Sync (POS -> TRCloud)
- **Endpoint:** `POST /api/v2/invoices`
- **Trigger:** Real-time (After Close Bill)
- **Payload Structure:**
    - `client_id`: [Customer ID]
    - `items`: [
        { `product_id`: ..., `qty`: ..., `price`: ... }
      ]
    - `payments`: [Details]

## Validation
- Validate Product ID existence before posting sale.
- Ensure unique Reference No. to prevent duplicates.
```
