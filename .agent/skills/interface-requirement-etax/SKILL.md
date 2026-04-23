---
name: interface-requirement-etax
description: Expert in Thai e-Tax Invoice & e-Receipt requirements (RD Standard, PDF/A-3, XML).
---

# e-Tax Invoice Requirement Prompter

This skill specializes in gathering requirements for **Thai e-Tax Invoice & e-Receipt** projects, ensuring compliance with Revenue Department (RD) standards.

## 1. Service Provider Strategy
- **Provider:** Connect directly to RD (rare) or via Service Provider (e.g., INET, Digio, Brainergy)?
- **API Spec:** Provider-specific API or Standard RD XML upload?
- **Certificate:** Who holds the CA Token (USB vs HSM)?

## 2. Functional Requirements
### 2.1 E-Tax Generation
- **Trigger:** Immediate at POS or End-of-Day Batch?
- **Validation:** Check 13-digit Tax ID (Head Office vs Branch)?
- **Format:**
  - **PDF/A-3:** For sending to customer (Email/Print).
  - **XML (etax-invoice model):** For signing and submitting to RD.

### 2.2 Customer Delivery
- **Channel:** Email (SMTP) / SMS / Print Short URL?
- **Consent:** PDPA Consent required before sending Email?

### 2.3 Cancellation (Credit Note/Debit Note)
- **Reason Code:** RD Standard Reason Codes (e.g., 'Wrong Price', 'Return Goods').
- **Reference:** Must reference Original Invoice UUID?

## 3. Data Structure (RD XML)
- **Seller:** Name, Address, Tax ID, Branch Code (00000).
- **Buyer:** Name, Address, Tax ID/ID Card, Branch/HQ.
- **Line Items:** Product Name, Qty, Unit, Price, VAT Amount, Exempt Amount.
- **Totals:** Total Vatable, Total Exempt, Total VAT, Grand Total.

## 4. Technical Integration
- **Signing:** Server-side signing (HSM) or Client-side (hard)?
- **Storage:** Archive PDF/XML for 5 years (Regulatory Requirement)?

## 5. Sequence & Data Flow Patterns
- **Reference Pattern:** `Async Upload` or `Realtime Sign`.
- **Diagram:** Link to standard e-Tax sequence diagram (`templates/seq-etax.puml`).

## 6. Output Generation
Generate a technical prompt for the developer:

```markdown
# e-Tax Integration Spec: [Project Name]

## Service Provider
- **Vendor:** [Name]
- **API Type:** REST API
- **Certificate:** Hosted on Cloud HSM

## Process Flow

### IF-01: Issue Tax Invoice
- **Input:** Transaction Data (POS)
- **Process:**
  1. Validate Tax ID format.
  2. Send JSON to Service Provider.
  3. Provider generates XML & Signs.
  4. Provider returns Signed PDF URL & XML.
- **Output:** Store PDF URL in POS Database.

### IF-02: Send Email
- **Trigger:** Upon success of IF-01.
- **Subject:** "ใบกำกับภาษีอิเล็กทรอนิกส์ (e-Tax Invoice)"
- **Attachment:** Signed PDF.
```
