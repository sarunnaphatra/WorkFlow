---
name: interface-requirement-general
description: Generates a comprehensive prompt for implementing AdaPos+ Interface projects (General/Standard) based on the standard checklist. Use this for non-SAP/D365 projects.
---

# General Interface Requirement Prompter

This skill guides you through defining the requirements for an AdaPos+ Interface project, ensuring all critical aspects from the standard checklist are covered.

## 1. Project Identification
First, identify the project you are working on.
- **Project Name:** (e.g., Project Baimiang, Project FIT Auto)
- **Source Documents:** Are there existing requirements documents in `adaproject/`?

## 2. Requirement Gathering
Based on `docs/06.Checklist/Sales_Requirement_Form.md`, we will cover the following sections. If you have a filled document, please provide its content or path. If not, answer the questions below.

### 2.1 ERP - Master : Product - Price
- **Tax:** Include or Exclude?
- **Effective Date:** Required? Rules?

### 2.2 ERP - Master : Price Structure
- **Tax:** Include or Exclude?
- **Structure:** Price Off vs Base Price?
- **Effective/End Date:** Required?

### 2.3 ERP - AP / Procurement : VAT
- **Purchase Tax:** Include or Exclude? Special cases?

### 2.4 ERP - Inventory
- **Vendor Tax Status:** No Tax, OTOP, Exempt?
- **Adjustment Method:** Cycle Count (Sales/Audit)?
- **Start Date/Balance:** Required?

### 2.5 ERP - POS (Export) : Connection
- **Method:** API / Web Service / File?
- **Authentication:** Token? Expiry?
- **Location:** FTP / sFTP / Network Path?
- **File Format:** CSV / TXT / JSON / XML?
- **Database Staging:** Type? Location?

### 2.6 ERP - POS (Export) : Transmission
- **Sales Data:** Split Bill or Lump Sum?
- **Schedule:** Realtime or Batch? Frequency?
- **Day-End:** Before or After Midnight?

### 2.7 ERP - POS (Export) : Documents & Conditions
- **File Structure:** Separate by type or Combined JSON?
- **Item Detail:** SKU-level or Summary?
- **Price Tax:** Include or Exclude?
- **Discounts:** Item, Bill, Promotion? Separation?
- **Customer Data:** Send to ERP? Attached or Separate?
- **Full Tax / CN:** Send to ERP? Attached or Separate?
- **Payment:** Prorate or Bill-level?
- **Currency:** Single/Multi? Rate source?
- **Rounding:** Rules? Level?
- **Decimals:** Precision for Price/Qty/Amount?

### 2.8 CRM : Connection
- **Method:** API / Web Service? Token?
- **Location:** FTP / sFTP / Network Path?
- **File Format:** CSV / TXT / JSON / XML?
- **Database Staging:** Type? Location?

### 2.9 Payment : Connection & Process
- **Method:** API / Web Service? Token/Key? Rotation?
- **Location:** Log/File Path? Security?
- **File Format:** CSV / TXT / JSON / XML?
- **Database Staging:** Type? Location?
- **GetToken Flow:** Trigger?
- **Validate Flow:** What to check?
- **Confirm Flow:** What data to return?

## 3. Output Generation
Once all information is gathered, generate a **Developer Prompt** structured as follows:

```markdown
# Project: [Project Name] - Interface Implementation

## Overview
Implement the interface between AdaPos+ and [External System] based on the following requirements.

## 1. Master Data (ERP -> POS)
- **Product Price:** [Details]
- **Price Structure:** [Details]

## 2. Inventory (ERP <-> POS)
- **Receiving:** [Details]
- **Adjustments:** [Details]

## 3. Sales Data (POS -> ERP)
- **Connection:** [Details]
- **Transmission:** [Details]
- **Data Format:** [Details]
- **Special Conditions:** [Details]

## 4. CRM Integration
- **Connection:** [Details]
- **Flows:** [Details]

## 5. Payment Integration
- **Connection:** [Details]
- **Flows:** [Details]

## Acceptance Criteria
- Verify [Specific scenarios based on requirements].
```
