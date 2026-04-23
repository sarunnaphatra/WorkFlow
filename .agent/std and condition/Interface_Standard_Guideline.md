# Interface Integration Standard Guideline (มาตรฐานงานเชื่อมต่อระบบ)
**สำหรับ:** System Analyst (SA) และ Business Analyst (BA)
**เวอร์ชัน:** 1.0.0
**ปรับปรุงล่าสุด:** 2026-02-01

เอกสารฉบับนี้รวบรวมมาตรฐาน รายการตรวจสอบ (Checklist) และแนวทางการประเมินงาน (Estimation) สำหรับโครงการที่มีการเชื่อมต่อข้อมูล (Interface) ระหว่าง AdaPos และระบบภายนอก (ERP, CRM, ฯลฯ)

---

## 1. บทนำ (Introduction)
การเชื่อมต่อระบบ (Interface) เป็นหัวใจสำคัญของการทำงานร่วมกันระหว่างระบบหน้าร้าน (POS) และระบบหลังบ้าน (ERP/CRM) ความเข้าใจที่ตรงกันระหว่าง SA/BA และลูกค้า จะช่วยลดความผิดพลาดและควบคุมขอบเขตงานได้ชัดเจน

### วัตถุประสงค์
1.  เพื่อให้ SA/BA มีมาตรฐานเดียวกันในการเก็บ Requirement และประเมินงาน
2.  เพื่อลดความเสี่ยงจากการตกหล่นของข้อมูลสำคัญ (Missing Requirements)
3.  เพื่อเป็นคู่มืออ้างอิงในการตรวจสอบความถูกต้องของระบบ (Verification)

---

## 2. ขอบเขตงานมาตรฐาน (Standard Interface Scope)

โครงสร้างการเชื่อมต่อมาตรฐานแบ่งออกเป็น 4 กลุ่มหลัก ตาม Business Flow:

### 2.1 Master Data (ข้อมูลหลัก)
*ทิศทางหลัก:* ERP $\rightarrow$ POS
*   **Product Master:** สินค้า, บาร์โค้ด, หน่วยนับ, หมวดหมู่ (สำคัญ: รองรับ Multi-Pack Size/Multi-Barcode หรือไม่)
*   **Price:** ราคาขาย, ราคาตามสาขา (Price List)
*   **Promotion:** โปรโมชั่นส่วนลด (ถ้ามี)
*   **Customer:** ข้อมูลสมาชิก/ลูกค้า (สำหรับออกใบกำกับภาษี)
*   **Supplier:** ข้อมูลผู้จำหน่าย (สำหรับรับเข้าสินค้า)

### 2.2 Inventory Management (จัดการสต็อก)
*ทิศทาง:* Two-way (ERP $\leftrightarrow$ POS)
*   **Inbound (รับเข้า):** ใบขอโอน (Transfer Request), ใบโอนออก (Transfer Out) จาก ERP
*   **Outbound (ส่งออก):** ใบรับโอน (Transfer In), ใบปรับปรุงสต็อก (Adjust), ใบตรวจนับ (Stock Count)
*   **Inter-Branch:** การโอนระหว่างสาขา (Branch to Branch)

### 2.3 Sales & Finance (การขายและการเงิน)
*ทิศทางหลัก:* POS $\rightarrow$ ERP
*   **Sales Transaction:** บิลขาย (ABB), ใบกำกับภาษี (Full Tax)
*   **Return/CN:** ใบลดหนี้/รับคืน (Credit Note)
*   **Payment:** ข้อมูลการชำระเงิน (Cash, Credit Card, QR)
*   **Reconciliation:** ยอดนำส่งเงิน (Shift Close)

### 2.4 CRM & Loyalty (สมาชิกและแต้ม)
*ทิศทาง:* Two-way (Real-time API)
*   **Member:** สมัครสมาชิก (Register), ตรวจสอบข้อมูล (Inquiry)
*   **Point:** สะสมแต้ม (Earn), แลกแต้ม (Burn), คืนแต้ม (Void)
*   **Coupon:** ตรวจสอบและใช้คูปอง (Validate & Redeem)

---

## 3. รายการตรวจสอบสำคัญ (Critical Checklist)

ก่อนเริ่มงานหรือประเมินราคา ต้องตรวจสอบประเด็นเหล่านี้เสมอ:

### 3.1 Connectivity & Infrastructure
- [ ] **Method:** Web Service (REST/SOAP), FTP (Text/CSV/XML), หรือ Direct DB?
- [ ] **Security:** มีการทำ Authentication (OAuth, Token, API Key) หรือไม่?
- [ ] **Network:** ต้องผ่าน VPN หรือเชื่อมต่อผ่าน Public Internet?

### 3.2 Business Logic Complexity
- [ ] **Prorate & Rounding:** ระบบปลายทางต้องการให้ POS เฉลี่ยส่วนลดลงรายสินค้า (Prorate) หรือไม่? (ความเสี่ยงสูง)
- [ ] **Tax Handling:** ราคาสินค้าเป็น Include หรือ Exclude Tax?
- [ ] **Decimal Precision:** ทศนิยมกี่ตำแหน่ง? (2 หรือ 4)

### 3.3 Data Volume & Constraints
- [ ] **Volume:** ปริมาณข้อมูลต่อวัน (Transactions/Day)
- [ ] **Frequency:** ความถี่ในการส่ง (Real-time หรือ End-Day)

---

## 4. แนวทางการประเมิน Man-Days (Estimation Guideline)

อ้างอิงมาตรฐาน Senior Developer (หากใช้ Junior ให้บวกเพิ่ม Buffer)

| กลุ่มงาน | ความซับซ้อน | ประเมิน (Days) | หมายเหตุ |
| :--- | :--- | :---: | :--- |
| **Master Data** | High | **4 - 5** | สินค้าซับซ้อน (Multi-Unit/Barcode) |
| **Inventory** | Medium | **3.0** | ต่อประเภทเอกสาร (Transfer, Adjust) |
| **Sales (Invoice)** | High | **4 - 5** | รวม Prorate/Tax Logic |
| **Sales (CN/Return)** | Medium | **2 - 3** | อ้างอิงโครงสร้างเดียวกับ Sales |
| **CRM (Earn/Burn)** | High | **3 - 4** | ต่อ Flow (รวมกรณี Void/Retry) |
| **Setup/Config UI** | Fixed | **3 - 4** | หน้าจอตั้งค่า Mapping |
| **Monitor/Log UI** | Fixed | **2 - 3** | หน้าจอตรวจสอบสถานะ |

**System Impact Factors (บวกเพิ่ม):**
*   **Authentication (Token):** +1.0 Day
*   **Middleware Required:** +1.5 - 2.0 Days
*   **Notification (Line/Email):** +2.0 - 3.0 Days

---

## 5. แผนภาพกระบวนการมาตรฐาน (Standard Flow)

(ดูรายละเอียดในไฟล์ `Standard_Interface_Flow.puml`)

---

## 6. เอกสารอ้างอิง (Reference)
*   `docs/05.Diagram/interface plantuml.md` - แผนภาพต้นฉบับ
*   `docs/06.Checklist/Condition Estimate Manday Interface Project.md` - รายละเอียดการประเมิน
*   `docs/06.Checklist/Interface_Integration_Checklist.md` - ตาราง Checklist ละเอียด
