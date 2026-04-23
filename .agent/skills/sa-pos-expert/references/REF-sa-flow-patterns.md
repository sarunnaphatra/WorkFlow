# REF: Flow & Diagram Patterns สำหรับแต่ละ Segment — AdaPos+

## 1. RETAIL — Sale Flow

```mermaid
flowchart TD
    A([เริ่มต้น]) --> B[เปิด Session]
    B --> C[/Scan Barcode / เลือกสินค้า/]
    C --> D[เพิ่มลงบิล]
    D --> E{เพิ่มสินค้าต่อ?}
    E -->|Yes| C
    E -->|No| F{ใช้คูปอง/แต้ม?}
    F -->|Yes| G[คำนวณ Prorate Discount]
    F -->|No| H[คำนวณ VAT]
    G --> H
    H --> I[/เลือกวิธีชำระ/]
    I --> J{ชำระสำเร็จ?}
    J -->|No| K[ยกเลิก/ลองใหม่]
    K --> I
    J -->|Yes| L[บันทึก Sale Document]
    L --> M[ลด Stock]
    L --> N[พิมพ์ใบเสร็จ ABB]
    N --> O{ลูกค้าขอ Full Tax?}
    O -->|Yes| P[สร้าง e-Tax Request]
    O -->|No| Q([จบ])
    P --> R[ส่ง iNET Provider]
    R --> Q
```

---

## 2. RETAIL — Return/CN Flow

```mermaid
flowchart TD
    A([เริ่มคืนสินค้า]) --> B[/ค้นหาใบเสร็จเดิม/]
    B --> C{พบเอกสาร?}
    C -->|No| D[แจ้ง Error]
    C -->|Yes| E[แสดงรายการสินค้า]
    E --> F[/ระบุสินค้าและจำนวนที่คืน/]
    F --> G{เกินจำนวนซื้อ?}
    G -->|Yes| H[แจ้ง Error]
    G -->|No| I[คำนวณ VAT คืน]
    I --> J[สร้าง CN-ABB]
    J --> K[เพิ่ม Stock]
    J --> L[คืนเงิน/เพิ่ม Credit]
    L --> M([จบ])
```

---

## 3. PROCUREMENT — Purchase Order Flow

```mermaid
flowchart TD
    A([เริ่ม]) --> B[/สร้าง PO ใบสั่งซื้อ/]
    B --> C[อนุมัติ PO]
    C --> D{อนุมัติ?}
    D -->|Reject| E[แก้ไข PO]
    E --> C
    D -->|Approve| F[ส่ง PO ให้ Supplier]
    F --> G[/รับสินค้า Goods Receipt/]
    G --> H{ตรงกับ PO?}
    H -->|ไม่ตรง| I[บันทึก Discrepancy]
    I --> J[รอจัดการ]
    H -->|ตรง| K[สร้าง DO ใบรับของ]
    K --> L[Stock เพิ่ม]
    K --> M[/รับ AP Invoice จาก Supplier/]
    M --> N[Match DO vs Invoice]
    N --> O[สร้าง AP Purchase]
    O --> P[ภาษีซื้อ + หนี้ AP]
    P --> Q[/ชำระเงิน/]
    Q --> R([จบ])
```

---

## 4. FOOD COURT — Store Debit Flow

```mermaid
sequenceDiagram
    actor Customer
    participant Cashier as Cashier Terminal
    participant API as API Server
    participant DB as Database

    Customer->>Cashier: แสดงบัตร Store Debit
    Cashier->>API: GET /wallet/balance/{cardNo}
    API->>DB: SELECT FNWallet WHERE CardNo=X
    DB-->>API: Balance: 500 บาท
    API-->>Cashier: Balance OK
    Cashier->>Cashier: คำนวณยอดสินค้า
    Cashier->>API: POST /wallet/deduct
    API->>DB: BEGIN TRAN
    DB->>DB: UPDATE Balance -= amount
    DB->>DB: INSERT WalletTransaction
    DB-->>API: COMMIT
    API-->>Cashier: สำเร็จ เหลือ 320 บาท
    Cashier-->>Customer: แสดงใบเสร็จ
```

---

## 5. DUTY FREE — Sale Flow

```mermaid
flowchart TD
    A([เริ่ม]) --> B[/อ่าน Passport/]
    B --> C[API ตรวจสอบกับ AOT]
    C --> D{Passenger Valid?}
    D -->|No| E[ปฏิเสธการขาย]
    D -->|Yes| F[ดึง Quota คงเหลือ]
    F --> G[/Scan สินค้า Duty Free/]
    G --> H{เกิน Quota?}
    H -->|Yes| I[แจ้งเตือน + ลดจำนวน]
    I --> G
    H -->|No| J[คำนวณยอด ไม่มี VAT]
    J --> K[/ชำระเงิน/]
    K --> L[บันทึก DF Sale]
    L --> M[ส่งข้อมูล AOT Real-time]
    M --> N[พิมพ์ใบเสร็จ DF]
    N --> O([จบ])
```

---

## 6. e-TAX — Full Tax Request Flow

```mermaid
sequenceDiagram
    actor Customer
    participant POS
    participant ETaxSrv as e-Tax Server
    participant iNET as iNET Provider
    participant RD as กรมสรรพากร

    Customer->>POS: ขอใบกำกับภาษีเต็มรูปแบบ
    POS->>POS: สร้าง CSV + PDF
    POS->>ETaxSrv: POST /etax/submit (CSV+PDF)
    ETaxSrv->>ETaxSrv: Validate Format
    ETaxSrv->>iNET: Submit Document
    iNET->>iNET: Digital Signature
    iNET->>RD: Submit to Revenue Dept
    RD-->>iNET: Acknowledge
    iNET-->>ETaxSrv: Document ID + Status
    ETaxSrv-->>POS: Success + Ref No.
    POS->>POS: บันทึก Full Tax Reference
    POS-->>Customer: Email / พิมพ์ Full Tax
```

---

## 7. CONSIGNMENT (PAS) — Flow

```mermaid
flowchart TD
    A([รับสินค้า Consignment]) --> B[รับเข้าสต๊อก]
    B --> C[ต้นทุน = 0 รอขาย]
    C --> D{มีการขาย?}
    D -->|No| C
    D -->|Yes| E[ขาย → สต๊อกลด]
    E --> F[Trigger: สร้าง AP PO อัตโนมัติ]
    F --> G[บันทึกต้นทุน]
    G --> H[ภาษีซื้อเกิด ณ วันที่ PO]
    H --> I{ครบรอบ BAS?}
    I -->|No| D
    I -->|Yes| J[สรุป Bill to Supplier]
    J --> K[ชำระเงิน]
    K --> L([จบรอบ])
```

---

## 8. STOCK COUNT (HHT) — Flow

```mermaid
flowchart TD
    A([เริ่มตรวจนับ]) --> B[ผู้จัดการสร้าง Count Sheet]
    B --> C[Lock สต๊อก ณ เวลา X]
    C --> D[/พนักงาน HHT สแกนสินค้า/]
    D --> E[บันทึกจำนวนจริงใน HHT]
    E --> F{นับครบทุก Location?}
    F -->|No| D
    F -->|Yes| G[Sync ข้อมูลขึ้น Server WiFi]
    G --> H[เปรียบเทียบ จำนวนนับ vs ระบบ]
    H --> I{มี Variance?}
    I -->|No| J[Confirm Count ตรงกัน]
    I -->|Yes| K[แสดงรายการ Variance]
    K --> L{อนุมัติปรับ?}
    L -->|No| M[ตรวจนับซ้ำ]
    M --> D
    L -->|Yes| N[สร้างใบปรับสต๊อก]
    N --> O[ปรับ Stock + ต้นทุน]
    J --> P([จบ])
    O --> P
```

---

## 9. FASHION — Size Matrix Flow

```mermaid
flowchart TD
    A([สร้างสินค้า Fashion]) --> B[กำหนด Style Code]
    B --> C[เลือก Color List]
    C --> D[เลือก Size List]
    D --> E[Generate Size Matrix]
    E --> F[กำหนดราคาแต่ละ SKU]
    F --> G[กำหนด Stock ต่อสาขา/คลัง]
    G --> H{รับสินค้าเข้า}
    H --> I[รับตาม Color × Size]
    I --> J[อัปเดต Stock Matrix]
    J --> K{ขาย}
    K --> L[/Scan Barcode ระบุ Color+Size/]
    L --> M[ลด Stock ตาม SKU]
    M --> N{Stock < Reorder?}
    N -->|Yes| O[แจ้งเตือน Replenishment]
    N -->|No| P([จบ])
    O --> P
```

---

## 10. MEMBER LOYALTY — Points Flow

```mermaid
sequenceDiagram
    actor Customer
    participant POS
    participant MemberAPI as Member API
    participant MemberDB as Member DB
    participant LINEOA as LINE OA

    Customer->>POS: แสดงบัตรสมาชิก/LINE
    POS->>MemberAPI: GET /member/{id}
    MemberAPI->>MemberDB: Query member + points
    MemberDB-->>MemberAPI: Member Info + 1,200 pts
    MemberAPI-->>POS: Member data
    POS->>POS: คำนวณแต้มที่ได้รับ
    POS->>POS: ตรวจสอบโปรโมชัน Tier
    POS->>MemberAPI: POST /points/earn
    MemberAPI->>MemberDB: UPDATE Points += earned
    MemberDB-->>MemberAPI: New balance: 1,350 pts
    MemberAPI->>LINEOA: Push Notification
    LINEOA-->>Customer: LINE แจ้ง +150 pts
    MemberAPI-->>POS: Success
```

---

## 11. ERP INTEGRATION — Interface Flow

```mermaid
flowchart TD
    subgraph AdaPos ["AdaPos+ System"]
        A[POS Sale] --> B[RabbitMQ Publisher]
        B --> C[(AdaPosDB)]
    end

    subgraph Interface ["Interface Server"]
        D[MQAdaLink Consumer] --> E[Transform Data]
        E --> F{เชื่อมต่อ ERP?}
        F -->|Online| G[POST to ERP API]
        F -->|Offline| H[Queue in Staging DB]
        H --> I[Retry Job]
        I --> G
    end

    subgraph ERP ["ERP System (SAP/Oracle/...)"]
        G --> J[ERP Inbound Handler]
        J --> K[Validate + Post]
        K --> L[Acknowledge]
    end

    B --> D
    L --> M[Update Status in AdaPos]
```

---

## 12. DEPOSIT (มัดจำ) — Prorate VAT Flow

```mermaid
flowchart TD
    A([สร้าง SO ยอด 2,000 บ. VAT=130.84]) --> B[รับมัดจำ 700 บ. 35%]
    B --> C[สร้าง DP Doc]
    C --> D[DP VAT = 130.84 × 35% = 45.79]
    D --> E{ลูกค้ารับสินค้า?}
    E -->|Yes| F[สร้าง Sale Doc ยอดเต็ม]
    F --> G[ตัดมัดจำ 700 บ.]
    G --> H[Sale VAT = 130.84 × 65% = 85.05]
    H --> I{ตรวจสอบ}
    I --> J{SO VAT = DP VAT + Sale VAT?}
    J -->|130.84 = 45.79 + 85.05 ✅| K[Post Document]
    J -->|ไม่ตรง ❌| L[Error: VAT ไม่สมดุล]
    K --> M([จบ])
```
