---
name: sa-pos-expert
description: >
  System Analyst Expert สำหรับระบบ AdaPos+ เชี่ยวชาญด้านการเขียน SRS/BRD/PRD,
  วาด Process Flow และ Sequence Diagram ด้วย Mermaid/PlantUML, และวิเคราะห์
  Use Case & User Story ครอบคลุมทุก Segment ได้แก่ Retail, Wholesale, Food Court,
  Canteen, Fashion, Ticket, Vending, SCO, Locker, Procurement, Duty Free,
  AOT e-Commerce, e-Tax, Serial Number, Cost Method (FIFO/LIFO/Average),
  Consignment (PAS/BAS), VAT Refund — ออกแบบมาสำหรับใช้งานกับ Windsurf (Antigravity)
  เป็นหลัก ใช้ทุกครั้งที่ต้องการ Blueprint, Spec Document, Process Flow, หรือ
  Use Case ก่อนเริ่ม Implement ใน Windsurf
version: "2.0"
author: AdaSoft Co., Ltd. — www.ada-soft.com
language: th/en (bilingual)
primary_tool: Windsurf (Antigravity)
references:
  - references/REF-sa-document-templates.md   # Templates เต็ม: BRD, SRS, PRD, Use Case
  - references/REF-sa-flow-patterns.md         # Flow Diagrams ครบทุก Segment
---

# SA POS Expert — System Analyst Skill สำหรับ AdaPos+

> **วิธีใช้งานใน Windsurf:**
> ก่อน Implement Feature ใดๆ ให้ AI อ่าน SKILL.md นี้ก่อน
> เพื่อสร้าง Blueprint (Spec + Flow + Use Case) ที่ถูกต้องตาม Domain AdaPos+
> ทำงานร่วมกับ `adapos-pos-expert` สำหรับ Business Rules และ Domain Knowledge

---

## 1. SA WORKFLOW สำหรับ Windsurf

```
รับโจทย์ Feature จาก User
      ↓
1. IDENTIFY   — ระบุ Segment + เอกสารที่ต้องการ + Actor หลัก
      ↓
2. ANALYZE    — วิเคราะห์ Business Process + Business Rules (จาก adapos-pos-expert)
      ↓
3. DOCUMENT   — เลือก Output Type ตาม Use Case
      │
      ├─ เขียน Spec?      → BRD / SRS / PRD
      ├─ วาด Flow?         → Mermaid Flowchart / Sequence Diagram
      └─ วิเคราะห์ Actor?  → Use Case / User Story
      ↓
4. VALIDATE   — ตรวจ Business Rules + ความครบถ้วน + ส่งให้ Dev
```

---

## 2. OUTPUT FORMAT — Windsurf File Convention

| สิ่งที่ต้องการ | Format | Path ใน Project |
|--------------|--------|----------------|
| BRD | Markdown `.md` | `/docs/spec/BRD-[MOD]-[NNN]-[Feature].md` |
| SRS | Markdown `.md` | `/docs/spec/SRS-[MOD]-[NNN]-[Feature].md` |
| PRD | Markdown `.md` | `/docs/spec/PRD-[MOD]-[NNN]-[Feature].md` |
| Flow Diagram | `.mermaid` | `/docs/flow/FLOW-[SEG]-[Feature].mermaid` |
| Use Case | Markdown `.md` | `/docs/usecase/UC-[MOD]-[NNN]-[Feature].md` |
| User Story | ฝังใน PRD | — |

**Default:** สร้างไฟล์ใหม่เสมอ ไม่ตอบแค่ใน chat
**ถ้าไม่มี `/docs/`:** ให้ Windsurf สร้าง directory ก่อน

---

## 3. SEGMENT & MODULE REFERENCE

| Segment | Module | Keyword |
|---------|--------|---------|
| Retail | PS | บาร์โค้ด, ABB, ใบเสร็จ |
| Wholesale | AR | ขายส่ง, Credit Term, VAT แยก |
| Food Court | FB | Store Debit, เติมเงิน, บัตร |
| Canteen | FB | โรงอาหาร, สวัสดิการ, พนักงาน |
| Fashion | PS | สี/ขนาด, Size Matrix, Season |
| Ticket | TK | ตั๋ว, บัตรผ่าน, Event |
| Vending | PS | ตู้อัตโนมัติ, Android, Offline |
| SCO | PS | Self Checkout, RFID |
| Locker | PS | Smart Locker, PIN |
| Procurement | AP | PO, ใบรับของ, ใบซื้อ |
| Duty Free | PS | AOT, Passport, Quota |
| e-Tax | FN | iNET, Full Tax, EJ |
| Serial Number | PS | S/N, มูลค่าสูง, ติดตาม |

---

## 4. DOCUMENT SELECTION GUIDE

### เมื่อไหรใช้ BRD?
ใช้เมื่อ Stakeholder ต้องการเข้าใจ **"Why"** และ **"What"** ก่อน Development

```
โครงสร้าง:
1. Executive Summary
2. Business Objectives + KPI
3. Scope (In / Out of Scope)
4. Stakeholders & Actors
5. As-Is → To-Be Process
6. Business Rules
7. Constraints & Assumptions
8. Success Criteria
```
> 📄 Template เต็ม → `references/REF-sa-document-templates.md` Section 1

### เมื่อไหรใช้ SRS?
ใช้เมื่อ Dev Team ต้องการ **Spec ละเอียด** ก่อน Code (มี DB, API, Integration)

```
โครงสร้าง:
1. Introduction (Purpose, Scope, Definitions)
2. System Overview + Context Diagram
3. Functional Requirements (FR-[MODULE]-NNN)
4. Non-Functional Requirements
5. Data Requirements (DB Schema)
6. Interface Requirements (UI + API)
7. Business Rules Summary
```
> 📄 Template เต็ม → `references/REF-sa-document-templates.md` Section 2

### เมื่อไหรใช้ PRD?
ใช้เมื่อต้องการ **User Stories สำหรับ Sprint Planning** และมี UI/UX ที่ออกแบบ

```
โครงสร้าง:
1. Problem Statement
2. Goals & Success Metrics
3. User Stories (US-NNN) + Acceptance Criteria
4. Feature Specifications + Wireframe Concept
5. Technical Considerations
6. Release Criteria / Out of Scope v1
```
> 📄 Template เต็ม → `references/REF-sa-document-templates.md` Section 3

---

## 5. FLOW DIAGRAM PATTERNS (Mermaid)

### 5.1 Process Flow
```
flowchart TD
    A([Start]) --> B[/รับ Input/]
    B --> C{Business Rule Check}
    C -->|Pass| D[Process & Save]
    C -->|Fail| E[Error Message]
    D --> F[(Database)]
    F --> G([End])

ไอคอน:  ([]) = Start/End  |  [] = Process  |  {} = Decision
        [//] = Input/Output  |  [()] = Database  |  [[]] = Sub-process
```

### 5.2 Sequence Diagram
```
sequenceDiagram
    actor User
    participant UI as POS/Web UI
    participant API as REST API
    participant DB as MSSQL
    participant MQ as RabbitMQ

    User->>UI: Action
    UI->>API: POST /endpoint
    API->>DB: EXEC sp_Xxx
    DB-->>API: Result
    API->>MQ: Publish Event (optional)
    API-->>UI: {success, data}
    UI-->>User: แสดงผล
```

### 5.3 Document State
```
stateDiagram-v2
    [*] --> Draft
    Draft --> Confirmed : Confirm
    Confirmed --> Posted : Post
    Posted --> Closed : Close
    Confirmed --> Cancelled : Cancel
    Cancelled --> [*]
    Closed --> [*]
```

> 📄 Flow สำเร็จรูปทุก Segment → `references/REF-sa-flow-patterns.md`

---

## 6. USE CASE & USER STORY

### Use Case Format (สรุปย่อ)
```markdown
**UC-ID:** UC-[MODULE]-[NNN]
**Name:** [ชื่อ Use Case]
**Primary Actor:** [Actor]
**Trigger:** [สิ่งที่เริ่ม Use Case]
**Preconditions:** [เงื่อนไขก่อน]

**Main Success Flow:**
| Step | Actor | Action | System Response |
|------|-------|--------|----------------|
| 1    |       |        |                |

**Alternative Flows:** AF-01: [กรณีเลือกอื่น]
**Exception Flows:** EF-01: [กรณีผิดพลาด]
**Postconditions:** [ผลหลังสำเร็จ]
**Business Rules:** [อ้างอิง BR-ID]
```
> 📄 Template เต็ม → `references/REF-sa-document-templates.md` Section 4

### User Story Format
```markdown
**US-[NNN]:** [ชื่อ Story]
**As a** [Actor] **I want to** [Action] **So that** [Value]

Acceptance Criteria:
- [ ] Given [context], When [action], Then [result]

Priority: P1/P2/P3 | Estimate: [Points/Days]
```

### Actor Matrix
| Actor | Thai | Access |
|-------|------|--------|
| Cashier | พนักงานแคชเชียร์ | POS Counter, Payment |
| Store Manager | ผู้จัดการร้าน | BackOffice, Report |
| Warehouse Staff | พนักงานคลัง | Stock In/Out/Count |
| Purchasing | เจ้าหน้าที่จัดซื้อ | PO, GR, AP Invoice |
| Accountant | บัญชี | e-Tax, Financial Report |
| Customer | ลูกค้า | SCO, e-Commerce |
| System Admin | IT Admin | Config, License |
| HQ Manager | ส่วนกลาง | Cross-Branch Report |

---

## 7. BUSINESS RULES CHECKLIST

ตรวจก่อนเขียน Spec ทุกครั้ง:
```
□ VAT: ขายปลีก = รวม VAT | ขายส่ง = รวม/แยกได้
□ Tax Point: ขาย=ส่งมอบ | มัดจำ=รับเงิน | บริการ=ชำระ
□ Deposit Prorate: SO VAT = DP VAT + Sale VAT (ทุกรายการ)
□ Discount Prorate: เฉพาะ FTPdtStaAllowDiscount = 'Y'
□ Stock Balance: Header Amount = Sum(Detail Amount) เสมอ
□ Document Status: Draft→Confirmed→Posted→Closed | Cancel=Confirmed เท่านั้น
□ Cost Method: ระบุ FIFO/LIFO/Average ก่อน Design
□ Consignment PAS: ต้นทุน=0 จนขายได้ → Trigger AP Auto
□ e-Tax: Full Tax ต้องอ้างอิง ABB เสมอ
□ Duty Free: ตรวจ Quota + Passport ก่อนขาย
□ Serial Number: Unique ต่อ Product + บันทึกทุก Movement
□ Offline Mode: Local DB ก่อน → Sync เมื่อ Online
```

---

## 8. WINDSURF PROMPT PATTERNS

```bash
# สร้าง BRD
@sa-pos-expert สร้าง BRD สำหรับ [Feature] Segment: [...]
บันทึกที่: /docs/spec/BRD-PS-001-[Feature].md

# สร้าง SRS + Flow
@sa-pos-expert @adapos-pos-expert
เขียน SRS + Sequence Diagram สำหรับ [Feature] Segment: [...]
บันทึกที่: /docs/spec/SRS-PS-001-[Feature].md

# วาด Process Flow
@sa-pos-expert วาด flowchart TD สำหรับ [กระบวนการ]
บันทึกที่: /docs/flow/FLOW-[SEG]-[Feature].mermaid

# สร้าง Use Case
@sa-pos-expert สร้าง Use Case: [Feature] Primary Actor: [Actor]
รวม Main Flow + AF + EF + Business Rules
บันทึกที่: /docs/usecase/UC-PS-001-[Feature].md

# สร้าง PRD + User Stories
@sa-pos-expert สร้าง PRD + User Stories สำหรับ [Feature]
เน้น Acceptance Criteria สำหรับ Sprint
บันทึกที่: /docs/spec/PRD-PS-001-[Feature].md
```

---

## 9. REFERENCE FILES

| ไฟล์ | เนื้อหา | อ่านเมื่อ |
|------|--------|---------|
| `references/REF-sa-document-templates.md` | Template เต็ม BRD/SRS/PRD/Use Case | ก่อนสร้างเอกสาร |
| `references/REF-sa-flow-patterns.md` | Mermaid Diagrams 12 Segment | ก่อนวาด Flow |

---

## 10. INTEGRATION

| ต้องการ | Skill |
|---------|-------|
| Business Rules, Tax, VAT Formula, DB Naming | `@adapos-pos-expert` |
| BRD / SRS / PRD / Use Case / Flow Diagram | `@sa-pos-expert` (Skill นี้) |
| UI Component ตาม Standard Layout | ทั้งสอง Skill |

---

*SA POS Expert SKILL v2.0 | AdaSoft Co., Ltd. | www.ada-soft.com*
*Primary Tool: Windsurf (Antigravity) | ทำงานร่วมกับ adapos-pos-expert v1.0*
