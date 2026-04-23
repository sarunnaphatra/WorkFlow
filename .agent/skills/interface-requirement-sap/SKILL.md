---
name: interface-requirement-sap
description: Generates a comprehensive prompt for implementing AdaPos+ Interface projects specifically for SAP (Business One / HANA / ECC / S4).
---

# SAP Interface Requirement Prompter

This skill specializes in gathering requirements for SAP integration projects. It focuses on standard SAP integration patterns (IDoc, BAPI, OData, PO/PI, CPI).

## 1. SAP Landscape Identification
- **SAP Version:** SAP B1 / ECC 6.0 / S/4HANA / SAP CAR?
- **Middleware:** SAP PO/PI, SAP CPI, or Direct Connection?
- **Project Name:** (e.g., Project Baimiang, Fit Auto)

## 2. Master Data (Inbound: SAP -> POS)
### 2.1 Product & Price (Material Master)
- **Method:** IDoc (MATMAS/COND_A), Proxy, or OData?
- **Pricing Condition:** Which Condition Types (PR00, Z...)?
- **Tax:** Is Tax included in the Condition Record or separate?
- **Barcode:** IDoc (MEAN) or separate?

### 2.2 Customer & Vendor
- **Customer:** DEBMAS or Business Partner (BP)?
- **Vendor:** CREMAS?
- **UDF/Custom Fields:** Any Z-fields required?

## 3. Inventory (Bi-Directional)
- **Stock Update:** BAPI_GOODSMVT or IDoc (WMMBXY)?
- **Movement Types:** 101/102 (GR), 301/303 (Transfer), 701/702 (Count)?
- **Real-time Check:** BAPI call for stock availability?

## 4. Sales Processing (Outbound: POS -> SAP)
### 4.1 Sales Data (POSLog / IDoc)
- **Format:** IDoc (WPUBON - Receipt, WPUUMS - Aggregated, WPUTAB)?
- **Structure:** Per Receipt or Aggregated per day?
- **Tax Code Mapping:** POS Tax Code -> SAP Tax Code (MWSKZ).
- **Payment Method Mapping:** POS Pay Code -> SAP GL Account or Payment Type.

### 4.2 Financials
- **Petty Cash:** Cash Journal (FBCJ)?
- **Shift Validations:** Z-Report reconciliation?

## 5. Technical Integration
- **Connection:** FTP/SFTP (File adapter) or REST/SOAP (HTTP adapter)?
- **Authentication:** Basic Auth, Certificate, or OAuth?
- **Frequency:** Batch (End of Day) or Near Real-time (Trickle feed)?

## 6. Sequence & Data Flow Patterns
- **Reference Pattern:** `IDoc` (Async/Event) vs `BAPI` (Sync) vs `OData`.
- **Diagram:** Link to standard SAP sequence diagram (`templates/seq-sap-idoc.puml`).

## 7. Output Generation
Generate a technical prompt for the developer:

```markdown
# SAP Integration Spec: [Project Name]

## Integration Architecture
- **SAP System:** [Version]
- **Middleware:** [Middleware]
- **Protocol:** [Protocol]

## Interface Catalog

### IF-01: Master Data (Product/Price)
- **Source:** SAP
- **Target:** POS
- **Object:** [MATMAS/COND_A]
- **Mapping Rules:**
    - Material Group -> [POS Category]
    - EAN11 -> [Barcode]
    - KBETR -> [Price]

### IF-02: Sales Transation
- **Source:** POS
- **Target:** SAP
- **Object:** [WPUBON/WPUUMS]
- **Mapping Rules:**
    - Mapping Table: Payment Type -> GL Account
    - Mapping Table: Tax Code -> SAP Tax Code

## Error Handling
- [Retry Mechanism]
- [Alerting]
```
