---
name: adapos-pos-expert
references:
  - references/REF-naming-convention.md   # กฎการตั้งชื่อ Table/Field (ฉบับเต็ม)
  - references/REF-tax-law-pos.md         # กฎหมายภาษีเอกสาร POS (ฉบับเต็ม)
description: >
  ผู้เชี่ยวชาญระบบ AdaPos+ ครอบคลุม POS, Retail, Wholesale, Food Court,
  Canteen, Fashion, Ticket, Vending, SCO, Locker, Procurement, Duty Free,
  AOT e-Commerce, e-Tax, Serial Number, Cost Method (FIFO/LIFO/Average),
  Consignment, VAT Refund, Database Design และ UI/UX Pattern
  ใช้งานได้กับ Claude, Cursor และ Windsurf (Antigravity)
version: "1.0"
author: AdaSoft Co., Ltd. — www.ada-soft.com
language: th/en (bilingual)
---

# AdaPos+ POS Expert SKILL

> **วิธีใช้งาน Skill นี้:**
> ก่อนตอบหรือสร้างโค้ด/เอกสารใดๆ ที่เกี่ยวกับระบบ AdaPos+ ให้ AI อ่าน SKILL.md นี้ก่อนทุกครั้ง
> เพื่อให้เข้าใจ Domain, Business Rules, Naming Convention และ Architecture อย่างถูกต้อง

---

## 1. PRODUCT OVERVIEW

**AdaPos+** คือ Enterprise-Grade Point of Sale System พัฒนาโดย AdaSoft Co., Ltd.
รองรับธุรกิจหลากหลายประเภทในระบบเดียว:

| Segment | คำอธิบาย |
|---------|-----------|
| Retail | ร้านค้าปลีก ทั่วไป |
| Wholesale | ขายส่ง ออกใบกำกับภาษีแยก/รวมได้ |
| Food Court | บัตร Store Debit, จุดแคชเชียร์เติมเงิน |
| Canteen | ระบบโรงอาหารบริษัท |
| Fashion | สต๊อกตามสี/ขนาด, Department/Class/SubClass/Season |
| Ticket | ระบบตั๋ว/บัตรผ่าน |
| Vending | ตู้จำหน่ายสินค้าอัตโนมัติ (Android + SQLite) |
| SCO | Self Checkout, RFID, ชำระ Online |
| Locker | Smart Locker (Windows + MSSQL Express) |
| Procurement | จัดซื้อ, ใบสั่งซื้อ, รับของ |
| Duty Free | สนามบิน AOT, บัตรประชาชน/Passport, VAT Refund |
| e-Commerce | เชื่อมต่อ AOT e-Commerce |
| e-Tax | ส่ง iNET, EJ (ABB), Full Tax, กรมสรรพากร |

---

## 2. SYSTEM ARCHITECTURE

### 2.1 Deployment Model
```
Cloud IaaS / On-Premise
├── VM/Server API1
│   ├── BackOffice Web App (PHP Fullstack)
│   ├── RESTful Microservices
│   │   ├── API2PSSale     — ส่งข้อมูลการขายขึ้น Server
│   │   ├── API2PSMaster   — Download Master สำหรับ Offline
│   │   ├── API2ARDoc      — ค้นหาเอกสารข้ามเครื่อง/ข้ามสาขา
│   │   └── API2FNWallet   — Store Debit, Coupon Online
│   ├── Background Process (RabbitMQ Consumer)
│   └── Schedule / Task App
│
├── VM/Server API2
│   ├── BackOffice Web App (PHP Fullstack)
│   ├── RabbitMQ Server (Message Queue)
│   └── RESTful Microservices (เหมือน API1)
│
├── VM/Server DB
│   ├── MSSQL Server
│   │   ├── AdaPosDB      — ฐานข้อมูลหลัก POS
│   │   └── AdaMemberDB   — ฐานข้อมูลสมาชิก
│
├── VM/Server Member (PHP + RESTful API2CNMember)
│
└── VM/Server Interface (ERP Integration)
    ├── Inbound: API2Link → RabbitMQ
    └── Outbound: MQAdaLink Consumer
```

### 2.2 Client / Device Matrix

| Device | OS | Database |
|--------|-----|----------|
| POS PC | Windows | MSSQL Express |
| POS Mobile/Tablet | Android | SQLite |
| Vending Machine | Android | SQLite |
| Smart Locker | Windows | MSSQL Express |
| SCO | Windows | MSSQL Express |
| Cashier PC | Windows | MSSQL Express |

### 2.3 Connectivity Pattern
- ทุก Client เชื่อมต่อผ่าน **Load Balancer** → API1 / API2
- รองรับ **Offline Mode** (ใช้ Local DB แล้ว Sync ภายหลัง)
- การสื่อสารแบบ **RESTful API** + **Message Queue (RabbitMQ)**

---

## 3. COMPANY STRUCTURE TYPES

```
1. บริษัทจำกัด (Single)    — สาขาหลายสาขา, รหัสสินค้าชุดเดียว
2. กลุ่มบริษัท             — บริษัทย่อยแยกรหัสสินค้า, ใช้สมาชิกร่วม
3. ตัวแทนขาย               — ส่วนกลางจัดการ, สินค้าตัวเอง vs ตัวแทน
4. แฟรนไชส์                — สั่งซื้อ/ขายระหว่างสาขา, ใช้สมาชิกร่วม
```

---

## 4. PRODUCT & INVENTORY

### 4.1 Product Configuration
- **1 สินค้า : หลาย Pack Size** (ชิ้น, โหล, ลัง) → ราคาตาม Pack Size
- **1 Pack Size : หลาย Barcode** (โรงงาน + ร้านค้าสร้างเอง)
- **รูปแบบราคา**: ควบคุมราคา | แก้ไขราคา | เครื่องชั่งพิมพ์ Barcode | ชั่งน้ำหนัก

### 4.2 Fashion
- สต๊อกตาม **สี × ขนาด** (Size Matrix)
- หมวดหมู่: Department → Class → Sub Class → Categories → Season

### 4.3 Serial Number Tracking *(TBD)*
- ติดตามสินค้ามูลค่าสูงตาม Serial Number
- ประวัติการเคลื่อนไหวทุก Transaction
- Costing Method: **Specific Identification**

### 4.4 Cost Calculation Methods

| Method | หลักการ | เหมาะกับ |
|--------|---------|---------|
| **FIFO** | ของเข้าก่อน → ออกก่อน | สินค้าทั่วไป |
| **LIFO** | ของเข้าหลัง → ออกก่อน | สินค้าราคาผันผวน |
| **Average (Moving)** | ต้นทุนเฉลี่ยถ่วงน้ำหนัก | ใช้บ่อยสุด |
| **Specific ID** | ระบุ Lot/Serial ชัดเจน | Serial Number / Consignment |

**สูตรต้นทุนเฉลี่ย:**
```
NewAvgCost = (OldQty × OldAvgCost + InQty × InCost) / (OldQty + InQty)
```

### 4.5 Consignment — PAS / BAS

| รูปแบบ | ความหมาย | Tax Point |
|--------|----------|-----------|
| **PAS** (Purchase After Sale) | บันทึกซื้อหลังจากขายได้ | เมื่อขายได้จริง |
| **BAS** (Bill After Sale) | ออกบิลซื้อหลังขาย | เมื่อสรุปรอบ |

**Business Rules:**
- สต๊อกรับเข้าแบบ Consignment ไม่มีต้นทุนจนกว่าจะขาย
- เมื่อขาย → ระบบ Trigger สร้าง AP Document อัตโนมัติ
- ภาษีซื้อเกิดเมื่อสร้าง PO / Invoice จาก Supplier

---

## 5. DOCUMENT FLOW (ซื้อ-ขาย-คลัง)

### 5.1 เอกสารซื้อ (AP)

```
ใบเสนอราคา → ใบสั่งซื้อ → ใบรับของ (DO) → ใบซื้อ → ชำระ
                                         ↓
                                   [สต๊อกเพิ่ม + คำนวณต้นทุน + ภาษีซื้อ]
```

ผลต่อระบบ:
- ใบรับของ (DO) = สต๊อกเพิ่ม
- ใบซื้อ = ต้นทุน + หนี้ AP + ภาษีซื้อ
- ใบลดหนี้มีสินค้า = สต๊อกลด + ภาษีซื้อลด
- ใบเพิ่มหนี้ = สต๊อกเพิ่ม + ต้นทุน + ภาษีซื้อ

### 5.2 เอกสารขาย (AR)

```
ใบเสนอขาย → ใบสั่งขาย → ใบมัดจำ → ใบกำกับฯ อย่างย่อ (ABB) → Full Tax
                     ↓
              [สต๊อกลด + ภาษีขาย]
```

ผลต่อระบบ:
- ใบขาย ABB = สต๊อกลด + ภาษีขาย
- ใบมัดจำ = หนี้ลด + ภาษีขาย (Tax Point = รับเงิน)
- ใบคืน CN-ABB = สต๊อกเพิ่ม + ภาษีขาย
- Full Tax = ไม่มีผลสต๊อก (แปลงจาก ABB)

### 5.3 เอกสารคลัง

| เอกสาร | สต๊อก | ต้นทุน |
|--------|-------|-------|
| ใบรับเข้า | เพิ่ม | คำนวณ |
| ใบเบิกออก | ลด | — |
| ใบโอนระหว่างคลัง | ต้นทาง↓ ปลายทาง↑ | — |
| ใบโอนระหว่างสาขา | ต้นทาง↓ ปลายทาง↑ | — |
| ใบตรวจนับสต๊อก | +/- | — |
| ใบปรับราคาทุน | — | คำนวณ |

---

## 6. VAT & TAX RULES (กฎหมายภาษีไทย)

> 📄 **ดูฉบับเต็มได้ที่:** `references/REF-tax-law-pos.md`

### 6.1 หลักการสำคัญ
- **ขายปลีก** → ราคา **รวม VAT** เท่านั้น
- **ขายส่ง** → รวม หรือ แยก VAT ได้

**สูตร:**
```
รวมภาษี:  VAT = Amt - (Amt × 100) / (100 + VatRate)
แยกภาษี:  VAT = (Amt × (100 + VatRate) / 100) - Amt
```

### 6.2 Tax Point
| กรณี | Tax Point เกิดเมื่อ |
|------|-------------------|
| ขายสินค้าทั่วไป | ส่งมอบสินค้า (หรือรับเงิน/ออกใบกำกับหากก่อนส่งของ) |
| มัดจำ | รับเงินมัดจำ |
| เช่าซื้อ/ผ่อนชำระ | ถึงกำหนดชำระแต่ละงวด |
| บริการ | รับชำระค่าบริการ |

### 6.3 Deposit (มัดจำ) + VAT Prorate

**หลักการ:** SO VAT = DP VAT + Sale VAT

```
SO0001: ยอด 2,000 บาท, VAT = 130.84
DP0001: ยอด 700 บาท (35%), VAT = 45.79
S0001:  ยอด 1,300 บาท (65%), VAT = 85.05

Proportion Rate = 700 ÷ 2,000 = 35%
Sale VAT = 130.84 × 65% = 85.05 ✅
```

**ความแตกต่าง Prorate:**
- **Discount**: เฉพาะสินค้าที่ `FTPdtStaAllowDiscount = 'Y'`
- **Deposit**: ทุกรายการสินค้า ไม่ต้องเช็ค AllowDiscount

### 6.4 e-Tax Flow

```
POS (ขาย) 
  → [พิมพ์ ABB ทันที]
  → [ลูกค้าขอ Full Tax]
     ├── ผ่าน Cashier: POS เตรียม CSV+PDF → ส่ง Provider (iNET)
     └── ผ่าน Web: QR Code → ลูกค้ากรอกข้อมูล → E-Tax → Provider
  → Provider ลงลายมือชื่อดิจิทัล → กรมสรรพากร
  → ส่ง Email / พิมพ์กระดาษ
```

เอกสาร:
- **EJ** = Electronic Journal (ABB + CN-ABB)
- **e-Tax** = Full Tax + CN-Full Tax

---

## 7. VAT REFUND & DUTY FREE

### 7.1 VAT Refund (ขายปลีกให้นักท่องเที่ยว)
- อ่าน **บัตรประชาชน** หรือ **Passport**
- ตรวจสอบเงื่อนไข: สัญชาติ, มูลค่าขั้นต่ำ, ประเภทสินค้า
- ออก **PP.10** หรือใบแจ้งคืนภาษีให้นักท่องเที่ยว
- ระบบต้องบันทึก Passport No. + สัญชาติ + วันที่ขาย

### 7.2 Duty Free (สนามบิน AOT)
- เชื่อมต่อระบบ AOT (API Integration)
- อ่าน **Passport** สำหรับยืนยันตัวตนผู้โดยสาร
- ตรวจสอบ: Gate, Flight, Quota ตามกฎหมาย
- ไม่คิด VAT สำหรับสินค้า Duty Free
- บันทึก Transaction ส่ง AOT Real-time

---

## 8. PAYMENT METHODS

| ประเภท | หมายเหตุ |
|--------|---------|
| เงินสด | ปัดเศษตามหน่วยท้องถิ่น (0.25/0.5/0.75 บาท) |
| บัตรเครดิต | เชื่อมต่อ EDC > 1 เครื่องต่อจุด |
| PromptPay / QR | Online Payment |
| Alipay / WeChat Pay | Multi-currency |
| คูปองเงินสด (GV) | ไม่กระทบ VAT |
| คูปองส่วนลด | กระทบ VAT ต้องคำนวณใหม่ |
| Store Debit Card | Food Court / Canteen |
| แต้มสมาชิก | แลกส่วนลด / สินค้า |
| มัดจำ | ตัด AR + Prorate VAT |
| เคลียร์หนี้ | ตัด AR หนี้เก่า |

**Multi-currency:** THB, USD, CNY (กำหนดอัตราแลกเปลี่ยน)

---

## 9. MEMBERSHIP & CRM

- รองรับกลุ่มบริษัทใช้บัตรร่วมกัน
- แต่ละบริษัทกำหนดเงื่อนไขสะสม/แลกแต้มต่างกัน
- ระดับสมาชิก (Tier) → สิทธิประโยชน์ต่างกัน
- เชื่อมต่อ **LINE OA** (LIFF) → สมัครสมาชิก, ดูแต้ม, ดาวน์โหลดใบเสร็จ
- Birthday Privilege, Share Promotion

### 9.1 Promotion Engine
- เงื่อนไข: ช่วงเวลา, สาขา, กลุ่มลูกค้า, วันเกิด, กลุ่มราคา
- สิทธิประโยชน์: ส่วนลด, ของแถม, คูปอง Next Bill, แต้มทวีคูณ, คูปองสอยดาว

---

## 10. SPECIAL SYSTEMS

### 10.1 Food Court / Canteen
```
[แคชเชียร์เติมเงิน] → บัตร Store Debit
[จุดขาย] → รับชำระด้วยบัตร, แยกรายได้ตามร้านค้า/พนักงาน
กำหนดโปรโมชั่นการเติมเงินได้
```

### 10.2 Self Checkout (SCO)
- สแกนบาร์โค้ดเอง
- ชำระ Online (บัตรเครดิต, PromptPay)
- ตรวจสอบสินค้าผ่าน RFID

### 10.3 HHT (Hand-Held Terminal) Stock Count
- อุปกรณ์ Android สำหรับตรวจนับสต๊อก
- Sync ข้อมูลผ่าน WiFi → Server
- รองรับ: สแกน Barcode, ระบุจำนวน, เปรียบเทียบ vs ระบบ

### 10.4 Vending Machine
- Android + SQLite
- Offline-first, Sync ยอดขายขึ้น Server
- จัดการสต๊อกภายในตู้

---

## 11. DATABASE NAMING CONVENTION

### 11.1 Table Naming: `TXXYName`

```
T   = Table (Fixed)
XX  = Module Code (2 chars)
     PS = POS
     FN = Finance
     TK = Ticket
     FB = Food & Beverage
     CN = Center (Cross-Module)
     AP = Accounts Payable
     AR = Accounts Receivable
Y   = Data Category (1 char)
     M = Master Data (สินค้า, พนักงาน, ลูกค้า)
     T = Transaction (เอกสาร, ประวัติ)
     S = System Config
Name = ชื่อตาราง ≤ 12 chars
```

**ตัวอย่าง:**
```
TCNMPdt    = Center Master Product
TPSTSalHD  = POS Transaction Sale Header
TARTSiHD   = AR Transaction Sale Header
TAPTOrder  = AP Transaction Order
TFNMBank   = Finance Master Bank
```

### 11.2 Field Naming: `FXAbcName`

```
F    = Field (Fixed)
X    = Data Type
     T = Text / String / Varchar
     D = Date / Datetime
     N = Integer (ไม่มีทศนิยม)
     C = Decimal (มีทศนิยม)
Abc  = Table Abbreviation (3 chars, ตรงกับทุก Field ในตาราง)
Name = ชื่อ Field ≤ 10 chars
```

**ตัวอย่าง:**
```sql
-- ตาราง TFNMBank (Abc = Bnk)
FTBnkCode     VARCHAR(20)      -- รหัสธนาคาร
FTBnkName     NVARCHAR(100)    -- ชื่อธนาคาร
FCBnkAmt      DECIMAL(15,2)    -- จำนวนเงิน
FDBnkRegis    DATETIME         -- วันที่ลงทะเบียน

-- ตาราง TPSTSalHD (Abc = Xsh)
FTXshDocNo    VARCHAR(20)      -- เลขที่เอกสาร
FDXshDocDate  DATETIME         -- วันที่เอกสาร
FCXshAmt      DECIMAL(15,2)    -- ยอดรวม
FCXshVatable  DECIMAL(15,2)    -- ยอด Vatable
FCXshVat      DECIMAL(15,2)    -- ภาษีมูลค่าเพิ่ม
```

### 11.3 Key Stored Procedures
```sql
sp_ProcessDepositProrate   -- Prorate VAT เมื่อตัดมัดจำ
sp_ProcessDiscountProrate  -- Prorate VAT เมื่อใช้คูปองส่วนลด
sp_CalcMovingAvgCost       -- คำนวณต้นทุนเฉลี่ย
sp_StockCount              -- ปรับยอดสต๊อกจากการตรวจนับ
```

---

## 12. UI/UX DESIGN PATTERN (Document Layout Standard)

### 12.1 Standard Document Screen Layout
```
┌─────────────────────────────────────────────────────────────┐
│ HEADER BAR: ชื่อเอกสาร | ปุ่ม บันทึก / พิมพ์ / ชำระ          │
├──────────────┬──────────────────────────────────────────────┤
│              │ TAB BAR: ข้อมูลทั่วไป | ที่อยู่ | เอกสารอ้างอิง│
│              │ TAB CONTENT (10%)                            │
│ LEFT PANEL   ├──────────────────────────────────────────────┤
│ (15%)        │                                              │
│              │         BODY (75%)                           │
│ - รายละเอียด │         ตารางรายการสินค้า                     │
│ - อ้างอิง    │                                              │
│              ├──────────────────────────────────────────────┤
│              │ FOOTER (15%): สรุปยอด | ชำระเงิน              │
└──────────────┴──────────────────────────────────────────────┘
```

### 12.2 Technology Stack (Frontend)
- **React + Tailwind CSS** (Interactive Artifact)
- **Markdown** (Documentation Default)
- **PlantUML / Mermaid** (Architecture Diagram)
- Color Scheme: สีน้ำเงิน `bg-blue-600`, Header `bg-gray-200`

### 12.3 Table Column Standard
```
ลำดับ | รหัสสินค้า | รายการ | จำนวน | หน่วย | ราคา/หน่วย | รวมเงิน
```
- Text Right: ตัวเลข, ราคา
- Text Left: รหัส, ชื่อ
- Hover: `hover:bg-gray-50`

---

## 13. INTEGRATION PATTERNS

### 13.1 AOT (Airports of Thailand)
- Real-time API ส่งข้อมูลการขาย Duty Free
- Passport Verification
- Quota Control ตามกฎหมาย

### 13.2 e-Tax Provider (iNET)
```
POS → CSV + PDF → iNET Provider → Digital Signature → กรมสรรพากร
```
- EJ (Electronic Journal): ABB ทุกใบ
- Full Tax: ตามคำขอลูกค้า

### 13.3 ERP Interface
- **Inbound**: RESTful API2Link → RabbitMQ → Consumer
- **Outbound**: MQAdaLink Consumer → ERP

### 13.4 Payment Gateway
- รองรับ EDC > 1 เครื่อง/จุด
- PromptPay QR, Alipay, WeChat Pay
- LINE Pay (ผ่าน LINE OA)

---

## 14. BUSINESS RULES (Critical)

```
✅ ALWAYS:
- Header Amount = Sum(Detail Amount)
- Header Amount = Sum(Payment Amount)  [Balance Check]
- Header VAT = Sum(Detail VAT)
- SO VAT = DP VAT + Sale VAT           [Deposit Rule]
- ต้นทุน Average = (OldQty×OldCost + InQty×InCost) / (OldQty+InQty)
- ขายปลีก → ราคา Include VAT เสมอ
- ผู้ออกใบกำกับภาษีต้องจด VAT เท่านั้น

⚠️ CRITICAL:
- Consignment PAS: ไม่มีต้นทุนจนกว่าจะขาย
- VAT Refund: ต้องบันทึก Passport No. + สัญชาติ
- Duty Free: ต้องตรวจสอบ Quota ก่อนขาย
- Serial Number: ต้อง Unique ต่อ Product
- Deposit Prorate: คิดจากทุกรายการ (ไม่ใช่แค่ที่ AllowDiscount)
- Discount Prorate: คิดเฉพาะ FTPdtStaAllowDiscount = 'Y'

❌ NEVER:
- อย่า Rollback สต๊อกโดยไม่มี Reference Document
- อย่าปรับราคาทุนแบบ Manual โดยไม่มีใบปรับราคาทุน
- อย่าออก Full Tax โดยไม่มี ABB อ้างอิง
```

---

## 15. DEVELOPMENT GUIDELINES

### 15.1 SQL Server Best Practices
- ใช้ **SERIALIZABLE Isolation Level** สำหรับ Stock Transaction
- Complex CTE → แปลงเป็น **Temp Table** เมื่อข้อมูลมาก
- ทุก Stored Procedure ต้องมี **RETRY Logic** สำหรับ Deadlock
- ใช้ **TRY/CATCH + TRANSACTION** ทุก SP ที่มีการเขียน

### 15.2 API Design
- RESTful Standard (GET/POST/PUT/DELETE)
- Response Format: `{ success: bool, data: {}, message: "" }`
- ใช้ Message Queue สำหรับ Long-Running Process

### 15.3 Offline-First
- Client เก็บ Master Data ไว้ Local
- Transaction บันทึก Local ก่อน → Sync เมื่อ Online
- Conflict Resolution: Server Wins (Last-Write-Wins ด้วย Timestamp)

---

## 16. HOW TO USE THIS SKILL

### สำหรับ Claude / Cursor / Windsurf

เมื่อได้รับโจทย์เกี่ยวกับ AdaPos+ ให้:

1. **ระบุ Segment** ว่าเป็น Retail/Food Court/Fashion/ฯลฯ
2. **ระบุ Document Type** ว่าเป็นซื้อ/ขาย/คลัง/ภาษี
3. **ใช้ Naming Convention** ตาม Section 11 เสมอ
4. **ตรวจสอบ Business Rules** ตาม Section 14 ก่อน Generate Code
5. **Output Format Default**: Markdown
6. **Output Format Interactive**: React + Tailwind CSS

### ตัวอย่าง Prompt Pattern

```
"ออกแบบ SP สำหรับ [Feature] ในระบบ AdaPos+ 
 Segment: [Retail/Fashion/...]
 Document: [ซื้อ/ขาย/คลัง]
 โดยใช้ Naming Convention ตาม SKILL.md"
```

```
"สร้าง UI Component สำหรับ [หน้าจอ] 
 ตาม Document Layout Standard ใน SKILL.md
 ใช้ React + Tailwind CSS"
```

---

## 17. QUICK REFERENCE

### VAT Formula
```
รวมภาษี: VAT = Amt - (Amt*100)/(100+7)
แยกภาษี: VAT = Amt*7/100
Prorate:  VAT_new = VAT_original × (1 - DepositAmt/TotalAmt)
```

### Table Prefix Quick Ref
```
TCNMxxx = Master Data ทั่วไป
TPSTxxx = POS Transaction
TAPTxxx = จัดซื้อ (AP)
TARTxxx = ขาย (AR)
TFNxxx  = การเงิน
TKTxxx  = ตั๋ว
TFBxxx  = Food & Beverage
```

### Document Status Flow
```
Draft → Confirmed → Posted → Closed
                 ↓
              Cancelled (ได้เฉพาะ Confirmed)
```

---

*SKILL Version 1.0 | AdaSoft Co., Ltd. | www.ada-soft.com*
*อัปเดตล่าสุด: กุมภาพันธ์ 2569*
