---
name: interface-requirement-sap-hana
description: Expert in AdaPos+ to SAP S/4HANA (or SAP ECC) integration using IDoc/RFC/Proxy patterns.
---

# SAP HANA (Enterprise) Interface Requirement Prompter

This skill specializes in gathering requirements for **SAP S/4HANA** or **SAP ECC** integration projects involving standard Retail IDocs (`WPUBON`, `WBBDLD`, `WPUTAB`).

## 1. SAP Landscape Identification
- **SAP Version:** S/4HANA On-Premise / S/4HANA Cloud / ECC 6.0?
- **Module:** SAP IS-Retail (Industry Solution)?
- **Middleware:** SAP PO/PI, CPI, or Mulesoft?

## 2. Inbound Data (SAP -> POS)
### 2.1 Master Data (Assortment List)
- **Object:** Assortment List (`WBBDLD`) vs Material (`MATMAS`)?
- **Trigger:** Change Pointer (Delta) + Full Load (Weekly)?
- **Mapping:**
  - `EAN11` -> Barcode
  - `MATKL` -> Product Group
  - `KBETR` -> Price (Condition Type `PR00` or `VKP0`?)

### 2.2 Z-Table / Custom RFC
- **Promotion:** `WAK1` (Promotion IDoc) or Custom Z-Table?
- **Customer:** `DEBMAS` or Business Partner (`BUPA`)?

## 3. Outbound Data (POS -> SAP)
### 3.1 POSLog / Sales XML
- **Standard:** `WPUBON` (Receipt-level) or `WPUUMS` (Aggregated)?
- **Tax:** Send Tax Code (`MWSKZ`) or Tax Amount?
- **Payment:** Mapping POS Tender -> SAP Tender Type (`E1WPB06-ZART`).
- **Store ID:** Mapping `BranchCode` -> `Plant` (`WERKS`).

### 3.2 Inventory Movements
- **Goods Receipt:** `WMMBXY` (Movement 101)?
- **Stock Count:** `WVINVE` (Physical Inventory)?

## 4. Technical Integration
- **Protocol:** SOAP / REST / SFTP (File Adapter)?
- **Security:** VPN / Basic Auth / Client Cert?
- **Batching:** 1 File per Store per Day? Or Real-time trickle?

## 5. Sequence & Data Flow Patterns
- **Reference Pattern:** `IDoc` (Async) via PO/PI.
- **Diagram:** Link to standard HANA IDoc sequence diagram (`templates/seq-sap-hana.puml`).

## 6. Output Generation
Generate a technical prompt for the developer:

```markdown
# SAP HANA Integration Spec: [Project Name]

## Architecture
- **SAP:** S/4HANA (Retail)
- **Middleware:** SAP PI/PO
- **Format:** IDoc XML (SAP Standard)

## Interface Catalog

### IF-01: Inbound Article (WBBDLD)
- **Source:** SAP (HANA)
- **Target:** POS DB (Product Master)
- **Mapping:**
  - `IDOC/E1WBB01/MATNR` -> SKU
  - `IDOC/E1WBB01/E1WBB03/EAN11` -> Barcode
  - `IDOC/E1WBB01/E1WBB07/KBETR` -> Selling Price

### IF-02: Outbound Sales (WPUBON)
- **Source:** POS
- **Target:** SAP (Billing)
- **Structure:** Per Receipt (Bonus Buy supported)
- **Validation:** Ensure Plant (Store ID) exists.
```
