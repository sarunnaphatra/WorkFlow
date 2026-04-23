# กฎการตั้งชื่อ Table และ Field ในการออกแบบฐานข้อมูล AdaPos+
> Reference Document สำหรับ AdaPos+ SKILL
> ใช้บังคับกับทุก Table และ Field ในระบบ AdaPos+

---

## 1. ชื่อ Table — รูปแบบ `TXXYName`

```
T   = Table (Fixed, ขึ้นต้นด้วย T เสมอ)
XX  = Module Code (2 ตัวอักษร)
Y   = Data Category (1 ตัวอักษร)
Name = ชื่อตาราง (≤ 12 ตัวอักษร)
```

### 1.1 Module Code (XX)
| Code | ระบบ | ตัวอย่าง |
|------|------|---------|
| `PS` | POS | `TPSTSalHD` |
| `FN` | Finance | `TFNMBank` |
| `TK` | Ticket | `TTKTOrder` |
| `FB` | Food & Beverage | `TFBMMenu` |
| `CN` | Center (ใช้ข้ามระบบ) | `TCNMPdt` |
| `AP` | Accounts Payable | `TAPTOrder` |
| `AR` | Accounts Receivable | `TARTSiHD`, `TARTSiDT` |
| `WH` | Warehouse | `TWHTStock` |
| `MB` | Member | `TMBTPoint` |
| `SC` | SCO | `TSCTTrans` |
| `VD` | Vending | `TVDTSale` |
| `LK` | Locker | `TLKTRent` |
| `DT` | Duty Free | `TDTTSale` |

### 1.2 Data Category (Y)
| Code | ความหมาย | ตัวอย่างข้อมูล |
|------|---------|--------------|
| `M` | Master Data | สินค้า, พนักงาน, ลูกค้า, เครื่อง POS |
| `T` | Transaction | เอกสารขาย, ใบสั่งซื้อ, ใบรับของ |
| `S` | System Config | ตั้งค่าระบบ, Parameter (จัดการโดย Programmer) |

### 1.3 ตัวอย่างชื่อตาราง
```
TCNMPdt    = Center > Master > Product (สินค้า)
TPSTSalHD  = POS > Transaction > Sale Header (หัวใบขาย)
TPSTSalDT  = POS > Transaction > Sale Detail (รายการใบขาย)
TAPTOrder  = AP > Transaction > Order (ใบสั่งซื้อ)
TARTSiHD   = AR > Transaction > Sale Invoice Header
TARTSiDT   = AR > Transaction > Sale Invoice Detail
TFNMBank   = Finance > Master > Bank (ธนาคาร)
TMBMCust   = Member > Master > Customer (ลูกค้าสมาชิก)
TWHTStock  = Warehouse > Transaction > Stock
```

---

## 2. ชื่อ Field — รูปแบบ `FXAbcName`

```
F   = Field (Fixed, ขึ้นต้นด้วย F เสมอ)
X   = Data Type (1 ตัวอักษร)
Abc = Table Abbreviation (3 ตัวอักษร)
Name = ชื่อ Field (≤ 10 ตัวอักษร)
```

### 2.1 Data Type (X)
| Code | ประเภท | SQL Type ที่ใช้ |
|------|--------|---------------|
| `T` | Text / String | `VARCHAR`, `NVARCHAR`, `CHAR` |
| `D` | Date / DateTime | `DATETIME`, `DATE` |
| `N` | Integer (ไม่มีทศนิยม) | `INT`, `BIGINT`, `SMALLINT` |
| `C` | Decimal (มีทศนิยม) | `DECIMAL(15,2)`, `NUMERIC`, `FLOAT` |

### 2.2 Table Abbreviation (Abc)
- ใช้ **3 ตัวอักษร** จาก Name ของตาราง
- ทุก Field ในตาราง **ที่ไม่ใช่ Foreign Key** ต้องใช้ Abc เดียวกัน
- Foreign Key ใช้ Abc ของตาราง**ต้นทาง**

| ตาราง | Name Part | Abc |
|-------|-----------|-----|
| TCNMPdt | Pdt | `Pdt` |
| TPSTSalHD | SalHD → XSH | `Xsh` |
| TPSTSalDT | SalDT → XSD | `Xsd` |
| TFNMBank | Bank | `Bnk` |
| TAPTOrder | Order | `Ord` |
| TMBMCust | Cust | `Cst` |
| TPSTPayHD | PayHD → XPH | `Xph` |

### 2.3 ตัวอย่างชื่อ Field

#### ตาราง `TFNMBank` (Abc = `Bnk`)
```sql
FTBnkCode     VARCHAR(20)       -- รหัสธนาคาร
FTBnkName     NVARCHAR(100)     -- ชื่อธนาคาร
FTBnkShort    NVARCHAR(20)      -- ชื่อย่อ
FCBnkAmt      DECIMAL(15,2)     -- จำนวนเงิน
FDBnkRegis    DATETIME          -- วันที่ลงทะเบียน
FNBnkSeq      INT               -- ลำดับ
```

#### ตาราง `TCNMPdt` (Abc = `Pdt`)
```sql
FTPdtCode            VARCHAR(20)     -- รหัสสินค้า
FTPdtName            NVARCHAR(200)   -- ชื่อสินค้า
FCPdtPrice           DECIMAL(15,2)   -- ราคาขาย
FCPdtCost            DECIMAL(15,2)   -- ราคาทุน
FTPdtStaAllowDiscount CHAR(1)        -- อนุญาตส่วนลด Y/N
FTPdtVatType         CHAR(1)         -- ประเภท VAT (I=รวม, E=แยก, N=ไม่มี)
FNPdtQtyMin          INT             -- จำนวนขั้นต่ำ
FDBnkCreateDate      DATETIME        -- วันที่สร้าง
```

#### ตาราง `TPSTSalHD` (Abc = `Xsh`)
```sql
FTXshDocNo           VARCHAR(20)     -- เลขที่เอกสาร
FDXshDocDate         DATETIME        -- วันที่เอกสาร
FTXshCustCode        VARCHAR(20)     -- FK: รหัสลูกค้า (ใช้ Abc ของ Customer)
FCXshAmt             DECIMAL(15,2)   -- ยอดรวม
FCXshVatable         DECIMAL(15,2)   -- ยอด Vatable
FCXshVat             DECIMAL(15,2)   -- VAT
FCXshDiscount        DECIMAL(15,2)   -- ส่วนลด
FCXshNetAmt          DECIMAL(15,2)   -- ยอดสุทธิ
FTXshStatus          VARCHAR(10)     -- สถานะ (OPEN/POSTED/CANCEL)
FCXshAmtOriginal     DECIMAL(15,2)   -- ยอดก่อน Prorate
FCXshVatOriginal     DECIMAL(15,2)   -- VAT ก่อน Prorate
```

#### ตาราง `TPSTSalDT` (Abc = `Xsd`)
```sql
FTXsdDocNo           VARCHAR(20)     -- FK: เลขที่เอกสาร
FNXsdSeq             INT             -- ลำดับรายการ
FTXsdPdtCode         VARCHAR(20)     -- FK: รหัสสินค้า
FCXsdQty             DECIMAL(15,4)   -- จำนวน
FCXsdPrice           DECIMAL(15,2)   -- ราคา
FCXsdAmt             DECIMAL(15,2)   -- ยอดรายการ
FCXsdVatable         DECIMAL(15,2)   -- Vatable รายการ
FCXsdVat             DECIMAL(15,2)   -- VAT รายการ
FCXsdCost            DECIMAL(15,4)   -- ต้นทุน
FTXsdPromoType       VARCHAR(20)     -- ประเภท Prorate (DEPOSIT/DISCOUNT)
```

---

## 3. กฎเพิ่มเติม

### 3.1 Foreign Key
- ใช้ Abc ของ**ตาราง Parent** ที่อ้างถึง
- ตัวอย่าง: Field อ้างถึง TCNMPdt → ใช้ `FTPdtCode`

### 3.2 ชื่อ Field ทั่วไปที่นิยมใช้
| ชื่อ | ความหมาย |
|------|---------|
| `Code` | รหัส Primary Key |
| `Name` | ชื่อ |
| `Amt` | จำนวนเงิน |
| `Qty` | จำนวนสินค้า |
| `Price` | ราคา |
| `Cost` | ต้นทุน |
| `Vat` | ภาษี VAT |
| `Vatable` | ฐาน VAT |
| `Status` | สถานะ |
| `DocNo` | เลขที่เอกสาร |
| `DocDate` | วันที่เอกสาร |
| `Seq` | ลำดับ |
| `Remark` | หมายเหตุ |
| `CreateDate` | วันที่สร้าง |
| `CreateBy` | ผู้สร้าง |
| `UpdateDate` | วันที่แก้ไข |
| `UpdateBy` | ผู้แก้ไข |

### 3.3 Status Values มาตรฐาน
```
DRAFT    = ร่าง (ยังไม่บันทึก)
OPEN     = เปิด (บันทึกแล้ว รอดำเนินการ)
POSTED   = ผ่านบัญชี/ยืนยันแล้ว
CLOSED   = ปิดแล้ว (ครบถ้วน)
CANCEL   = ยกเลิก
PARTIAL  = บางส่วน
```

---

## 4. Quick Checklist ก่อนตั้งชื่อ

```
Table:
☐ ขึ้นต้นด้วย T
☐ XX = Module Code 2 ตัวอักษร
☐ Y = M/T/S
☐ Name ≤ 12 ตัวอักษร
☐ ไม่มีช่องว่าง ไม่มีอักขระพิเศษ

Field:
☐ ขึ้นต้นด้วย F
☐ X = T/D/N/C ตามประเภทข้อมูล
☐ Abc = 3 ตัวอักษรของตาราง (ยกเว้น FK ใช้ Abc ของ Parent)
☐ Name ≤ 10 ตัวอักษร
☐ PascalCase เสมอ
```
