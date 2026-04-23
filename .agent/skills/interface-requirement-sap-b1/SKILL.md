---
name: interface-requirement-sap-b1
description: Expert in AdaPos+ to SAP Business One (B1) integration using Service Layer or DI API.
---

# SAP Business One (B1) Interface Requirement Prompter

This skill specializes in gathering requirements for **SAP Business One** integration, specifically focused on **Service Layer (OData)** or **DI API**.

## 1. SAP B1 Environment
- **Version:** SAP B1 for HANA (Service Layer available) or SQL Version?
- **Connection:** Service Layer (REST/OData) vs DI Server (SOAP)?
- **Database:** Company DB Name?

## 2. Master Data (SAP B1 -> POS)
### 2.1 Item Master
- **Endpoint:** `GET /Items`
- **Fields:** `ItemCode`, `ItemName`, `BarCode`, `U_CustomField`?
- **Price List:** Which `PriceList` index to use? (e.g., 1=Base, 2=Retail)
- **Warehouse:** Filter by `DefaultWarehouse`?

### 2.2 Business Partners
- **Endpoint:** `GET /BusinessPartners`
- **Type:** `cCustomer` (C) only?

## 3. Sales Processing (POS -> SAP B1)
### 3.1 A/R Invoice vs Order
- **DocType:** `Invoices` (Direct Sale) or `Orders` (Booking)?
- **Mapping:**
  - `DocumentLines`: SKU, Quantity, Price, TaxCode (`VatGroup`).
  - `DocDate`: Transaction Date.
  - `CardCode`: Customer Code (or 'C99999' for General Cash).

### 3.2 Payments
- **Endpoint:** `IncomingPayments`? or inside Invoice?
- **Mapping:** Cash Account vs Credit Card GL Account.

## 4. Technical Integration
- **Session:** Login via `/Login` -> Get `B1SESSION` cookie?
- **Batch:** Use `$batch` for bulk insert?
- **Error Handling:** Parsing B1 Error Message (`error.message.value`).

## 5. Sequence & Data Flow Patterns
- **Reference Pattern:** `Service Layer` (Sync REST/OData).
- **Diagram:** Link to standard B1 sequence diagram (`templates/seq-sap-b1.puml`).

## 6. Output Generation
Generate a technical prompt for the developer:

```markdown
# SAP B1 Integration Spec: [Project Name]

## Connectivity
- **Service Layer URL:** https://[Server]:50000/b1s/v1/
- **Company DB:** [DB Name]
- **Auth:** Login Route (Session Cookie)

## Interface Catalog

### IF-01: Item Master Sync
- **Endpoint:** `GET /Items?$select=ItemCode,ItemName,ForeignName,BarCode,PriceList`
- **Filter:** `Valid eq 'tYES'`
- **Mapping:**
  - `ForeignName` -> POS Description

### IF-02: Post Invoice
- **Endpoint:** `POST /Invoices`
- **Payload:**
  - `CardCode`: "C0001"
  - `DocDate`: "2024-01-01"
  - `DocumentLines`: [ { "ItemCode": "A001", "Quantity": 1, "Price": 100 } ]
```
