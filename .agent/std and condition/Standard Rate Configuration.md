# Standard Rate Configuration (อัตราค่าจ้างและเงื่อนไขมาตรฐาน)

เอกสารฉบับนี้ใช้กำหนดอัตราค่าจ้าง (Daily Rate) และเงื่อนไขตั้งต้น (Initial Conditions) สำหรับการประเมินงบประมาณโครงการ AdaPos+ Interface

---

## 1. SM Rate (Selling Price - Rev 2)
*ใช้สำหรับ: Project Proposal และการคำนวณราคาขายลูกค้า*

| บทบาท (Role) | อัตราค่าจ้าง (บาท/วัน) |
| :--- | :--- |
| **Project Manager (PM)** | 15,000 |
| **System Analyst (SA)** | 10,000 |
| **Developer (Dev)** | 7,500 |
| **Tester (QA)** | 7,500 |
| **Implementer (Imp)** | 6,500 |

---

## 2. DI Rate (Internal Cost)
*ใช้สำหรับ: การคำนวณต้นทุนภายใน (Internal Tracking) และการประเมินความคุ้มค่าของโครงการ*

| บทบาท (Role) | อัตราค่าจ้าง (บาท/วัน) |
| :--- | :--- |
| **Project Manager (PM)** | 9,000 |
| **System Analyst (SA)** | 6,000 |
| **Developer (Dev)** | 3,500 |
| **Tester (QA)** | 3,000 |
| **Implementer (Imp)** | 3,000 |

---

## 3. Complexity Effort Ratios (ตัวคูณตามความซับซ้อน)
*ค่าเปอร์เซ็นต์ที่ใช้คูณกับ Adjusted Dev Days เพื่อระบุ Effort ของบทบาทต่างๆ*

| Complexity | Req (%) | Tester (%) | SA (%) | UAT (%) | Interface (%) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Low** | 10% | 20% | 10% | 15% | 15% |
| **Medium** | 12% | 25% | 15% | 15% | 15% |
| **High** | 15% | 35% | 25% | 20% | 15% |

---

## 3. Project Management Ratios (ตัวคูณ PM ตามขนาดโครงการ)
*คิดเปอร์เซ็นต์จาก Total Project Duration (Timeline)*

| Project Size | PM Ratio (%) |
| :--- | :---: |
| **Small** | 8% |
| **Medium** | 10% |
| **Large** | 15% |

---

## 4. Developer Level Multipliers (ตัวคูณตามทักษะ)
*ใช้ปรับแก้เนื้องาน (Man-Days) ตามความเชี่ยวชาญ*

| Skill Level | Multiplier (%) |
| :--- | :---: |
| **Junior** | 120% |
| **Standard** | 100% |
| **Senior** | 80% |

---

## 5. Other Initial Conditions (เงื่อนไขอื่นๆ)
1. **Buffer Default**: 15% (เพิ่มเข้าเนื้องานก่อนคำนวณระยะเวลา)
2. **Go-Live Fixed**: 1 วัน (Man-Hour พื้นฐานสำหรับการขึ้นระบบ)
3. **8-Hour Day**: 1 Man-Day = 8 ชั่วโมงทำงาน
4. **Duration Calculation**: จำนวนวัน = (Effort / จำนวนคนในทีม)
5. **Weekend Skip**: ระบบคำนวณจะข้ามวันเสาร์-อาทิตย์โดยอัตโนมัติ

---

## 6. Official Holidays (วันหยุดนักขัตฤกษ์ - พ.ศ. 2569)
*ระบุวันที่ต้องการให้ระบบข้ามในการคำนวณ Total Duration*

| วันที่ (Date) | ชื่อวันหยุด (Holiday Name) |
| :---: | :--- |
| 2026-01-01 | New Year's Day |
| 2026-04-13 | Songkran Festival |
| 2026-04-14 | Songkran Festival |
| 2026-04-15 | Songkran Festival |
| 2026-05-01 | Labor Day |
| 2026-10-13 | Memorial Day (Rama IX) |
| 2026-12-05 | Father's Day |
| 2026-12-31 | New Year's Eve |

---
**ปรับปรุงล่าสุด:** 2026-03-19 (Added Holiday Support)
