---
name: interface-requirement-d365
description: Generates a comprehensive prompt for implementing AdaPos+ Interface projects specifically for Microsoft Dynamics 365 (F&O / AX / BC).
---

# D365 Interface Requirement Prompter

This skill specializes in gathering requirements for Microsoft Dynamics 365 integration projects. It focuses on Data Entities, OData, and DMF Patterns.

## 1. D365 Environment Identification
- **Version:** D365 F&O, D365 Business Central (BC), or AX 2012?
- **Project Name:** (e.g., Project Leonian)

## 2. Master Data (Inbound: D365 -> POS)
### 2.1 Products
- **Data Entity:** ReleasedProductsV2, EcoResProduct?
- **Barcodes:** BarcodeEntity?
- **Hierarchy:** RetailProductHierarchy or Standard Category?

### 2.2 Pricing & Discounts
- **Trade Agreements:** OpenSalesPriceJournal?
- **Discounts:** RetailDiscount?

## 3. Inventory
- **Journals:** CountingJournal, MovementJournal?
- **On-Hand:** InventoryOnhandEntity (Real-time OData query)?

## 4. Sales Processing (Outbound: POS -> D365)
### 4.1 Sales Order vs Retail Statement
- **Method:** Create Sales Order (Real-time) or Retail Statement (End of Day)?
- **Statment Posting:** Who triggers posting? (Auto-batch in D365?)

### 4.2 Payments
- **Payment Journal:** CustomerPaymentJournal?
- **Tender Types:** Mapping POS Tender to D365 Method of Payment.

## 5. Technical Integration
- **Pattern:** OData (Real-time, low volume) or Recurring Integrations (DMF - High volume, file-based)?
- **Authentication:** Azure AD (Entra ID) - Client ID / Secret / Tenant ID.
- **Format:** JSON or XML (if file-based)?

## 6. Sequence & Data Flow Patterns
- **Reference Pattern:** `Sync` (OData) vs `Async` (DMF/Batch).
- **Diagram:** Link to standard D365 sequence diagram (e.g., `templates/seq-d365-async.puml`).

## 7. Output Generation
Generate a technical prompt for the developer:

```markdown
# D365 Integration Spec: [Project Name]

## Connectivity
- **Tenant:** [Tenant ID]
- **Resource:** [D365 URL]
- **Pattern:** [DMF/OData]

## Interface Catalog

### IF-01: Product Master
- **Entity:** ReleasedProductsV2
- **Filter:** [DataAreaId = '...']
- **Mapping:**
    - ItemId -> [SKU]
    - SearchName -> [Description]

### IF-02: Sales Integration
- **Entity:** [SalesOrderHeaderV2 / RetailTransaction]
- **Logic:**
    1. Create Header
    2. Add Lines
    3. Post Invoice (optional)

## Validation
- Ensure TLS 1.2+
- Validate Token Expiry handling
```
