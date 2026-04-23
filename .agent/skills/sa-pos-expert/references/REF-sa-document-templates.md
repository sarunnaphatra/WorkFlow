# REF: SA Document Templates สำหรับ AdaPos+

## 1. BRD FULL TEMPLATE

```markdown
# BRD: [ชื่อ Feature/Module]
**Document ID:** BRD-[MODULE]-[NNN]
**Version:** 1.0
**Date:** [วันที่]
**Author:** [ชื่อ SA]
**Status:** Draft / Review / Approved

---

## 1. Executive Summary
[สรุปความต้องการทางธุรกิจใน 3-5 บรรทัด]

## 2. Business Objectives
- OBJ-01: [เป้าหมาย 1]
- OBJ-02: [เป้าหมาย 2]

## 3. Scope
### In Scope
- [สิ่งที่อยู่ในขอบเขต]

### Out of Scope
- [สิ่งที่ไม่รวม]

## 4. Stakeholders
| Role | Name | Responsibility |
|------|------|----------------|
| Business Owner | | อนุมัติ Requirement |
| SA | | วิเคราะห์/ออกแบบ |
| Developer | | พัฒนา |
| QA | | ทดสอบ |

## 5. Business Process
### As-Is (กระบวนการปัจจุบัน)
[อธิบาย/แสดง Flow ปัจจุบัน]

### To-Be (กระบวนการที่ต้องการ)
[อธิบาย/แสดง Flow ใหม่]

### Gap Analysis
| As-Is | To-Be | Gap |
|-------|-------|-----|

## 6. Business Rules
| BR-ID | Description | Source |
|-------|-------------|--------|
| BR-001 | [กฎ] | [ที่มา] |

## 7. Constraints & Assumptions
### Constraints
- [ข้อจำกัด]

### Assumptions
- [สมมติฐาน]

## 8. Success Criteria
- KPI-01: [วัดผลอย่างไร]

## 9. Dependencies
- [สิ่งที่ขึ้นต่อกัน]

## 10. Risks
| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
```

---

## 2. SRS FULL TEMPLATE

```markdown
# SRS: [ชื่อ Module/Feature]
**Document ID:** SRS-[MODULE]-[NNN]
**Version:** 1.0
**Date:** [วันที่]
**Based on BRD:** BRD-[MODULE]-[NNN]

---

## 1. Introduction
### 1.1 Purpose
[วัตถุประสงค์ของเอกสาร]

### 1.2 Scope
[ขอบเขตของ Software ที่พัฒนา]

### 1.3 Definitions
| Term | Definition |
|------|-----------|
| ABB | ใบกำกับภาษีอย่างย่อ |
| PAS | Purchase After Sale |
| EJ | Electronic Journal |

## 2. System Overview
### 2.1 Context Diagram
[แสดง Mermaid C4 Context หรือ Flowchart]

### 2.2 System Interfaces
[ระบบภายนอกที่เชื่อมต่อ]

## 3. Functional Requirements
### FR-[MODULE]-001: [ชื่อ Requirement]
**Priority:** Must Have / Should Have / Nice to Have
**Description:** [รายละเอียด]
**Input:** [ข้อมูลนำเข้า]
**Processing:** [การประมวลผล]
**Output:** [ผลลัพธ์]
**Business Rules:** [อ้างอิง BR-ID]

## 4. Non-Functional Requirements
### 4.1 Performance
- Response Time: < 2 วินาที สำหรับ Transaction ปกติ
- Concurrent Users: รองรับ X เครื่องพร้อมกัน

### 4.2 Security
- Authentication: Windows Auth / JWT Token
- Authorization: Role-based Access Control
- Audit Trail: บันทึกทุก Transaction

### 4.3 Availability
- Uptime: 99.5% (ไม่รวม Maintenance)
- Offline Mode: รองรับการทำงานเมื่อ Network ขาด

## 5. Data Requirements
### 5.1 New Tables
[DB Schema]

### 5.2 Modified Tables
[Field ที่เพิ่ม/แก้ไข]

### 5.3 Data Migration
[ถ้ามี]

## 6. Interface Requirements
### 6.1 UI Requirements
[หน้าจอที่ต้องพัฒนา]

### 6.2 API Requirements
[Endpoint ที่ต้องพัฒนา]

### 6.3 External Interfaces
[การเชื่อมต่อภายนอก]

## 7. Business Rules Summary
[รวม Business Rules ทั้งหมดจาก Section 3]

## 8. Constraints
[ข้อจำกัดทางเทคนิค]
```

---

## 3. PRD FULL TEMPLATE

```markdown
# PRD: [ชื่อ Feature]
**Document ID:** PRD-[MODULE]-[NNN]
**Version:** 1.0
**Date:** [วันที่]
**Product Owner:** [ชื่อ]

---

## 1. Problem Statement
**Problem:** [ปัญหาที่ต้องแก้]
**Impact:** [ผลกระทบถ้าไม่แก้]
**Opportunity:** [โอกาสถ้าแก้ได้]

## 2. Goals & Success Metrics
| Goal | Metric | Target |
|------|--------|--------|
| [เป้าหมาย] | [วัดจาก] | [ค่าเป้า] |

## 3. User Stories
### US-001: [ชื่อ Story]
**As a** [Actor]
**I want to** [Action]
**So that** [Value]

**Acceptance Criteria:**
- [ ] Given [context], When [action], Then [result]
- [ ] [criteria อื่นๆ]

**Priority:** P1/P2/P3
**Estimate:** [จำนวน days/points]

## 4. Feature Specifications
### Feature: [ชื่อ Feature]
**Description:** [รายละเอียด]
**Wireframe:** [อ้างอิงหรือแสดง ASCII art]
**Edge Cases:** [กรณีพิเศษ]

## 5. Technical Considerations
- [ประเด็นทางเทคนิคที่ต้องพิจารณา]

## 6. Release Criteria
- [ ] [เงื่อนไขที่ต้องผ่านก่อน Release]

## 7. Out of Scope (v1)
- [สิ่งที่จะทำในรุ่นต่อไป]
```

---

## 4. USE CASE TEMPLATE

```markdown
## Use Case: [ชื่อ Use Case]
**UC-ID:** UC-[MODULE]-[NNN]
**Version:** 1.0
**Date:** [วันที่]

### Basic Information
| Item | Value |
|------|-------|
| Use Case Name | [ชื่อ] |
| Primary Actor | [Actor หลัก] |
| Secondary Actor(s) | [Actor รอง] |
| Trigger | [สิ่งที่ทำให้เริ่ม Use Case] |
| Goal | [เป้าหมายของ Use Case] |

### Preconditions
1. [เงื่อนไขที่ต้องเป็นจริงก่อน]

### Main Success Flow
| Step | Actor | Action | System Response |
|------|-------|--------|----------------|
| 1 | Cashier | เลือกสินค้า | แสดงราคาและรายการ |
| 2 | System | คำนวณยอด | อัปเดต Cart |
| 3 | Cashier | กดชำระ | แสดงหน้าชำระเงิน |
| ... | | | |

### Alternative Flows
#### AF-01: [กรณีเลือก]
- ที่ Step X: ถ้า [เงื่อนไข] ให้ทำ [action] แล้วกลับ Step Y

### Exception Flows
#### EF-01: [กรณีผิดพลาด]
- ที่ Step X: ถ้า [error] ให้แสดง [message] และ [action]

### Postconditions
- **Success:** [สถานะหลังสำเร็จ]
- **Failure:** [สถานะหลังล้มเหลว]

### Business Rules
- BR-001: [กฎที่เกี่ยวข้อง]

### Special Requirements
- [ข้อกำหนดพิเศษ เช่น Performance, Security]

### Related Use Cases
- Extends: [UC-ID อื่นๆ]
- Includes: [UC-ID อื่นๆ]
```

---

## 5. API SPECIFICATION TEMPLATE

```markdown
## API: [ชื่อ Endpoint]
**Method:** POST / GET / PUT / DELETE
**URL:** `/api/v1/{module}/{resource}`
**Auth:** Bearer Token / API Key
**Version:** v1

### Request
**Headers:**
```
Authorization: Bearer {token}
Content-Type: application/json
```

**Body:**
```json
{
  "field1": "string",
  "field2": 0
}
```

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|

### Response (Success 200)
```json
{
  "success": true,
  "data": { },
  "message": "สำเร็จ"
}
```

### Response (Error)
| Code | Error Code | Description |
|------|-----------|-------------|
| 400 | INVALID_INPUT | ข้อมูลไม่ถูกต้อง |
| 401 | UNAUTHORIZED | ไม่มีสิทธิ์ |
| 404 | NOT_FOUND | ไม่พบข้อมูล |
| 500 | SERVER_ERROR | ข้อผิดพลาดระบบ |

### Business Logic
[อธิบาย Logic สำคัญ]

### Related Tables
- Read: [ตารางที่อ่าน]
- Write: [ตารางที่เขียน]
```
