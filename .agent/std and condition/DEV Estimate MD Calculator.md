# DEV Estimate MD Calculator (สูตรการคำนวณประเมิน Man-Days)

เอกสารนี้รวบรวมเงื่อนไขและสูตรการคำนวณสำหรับการประเมิน Man-Days ของโครงการพัฒนาซอฟต์แวร์ เพื่อให้ได้ผลลัพธ์ที่แม่นยำและเป็นมาตรฐานเดียวกัน

---

## 1. ข้อมูลนำเข้า (Input Factors)

ในการคำนวณจะต้องระบุค่าเริ่มต้นดังนี้:

| ตัวแปร (Variable) | คำอธิบาย | ตัวอย่างค่า |
| :--- | :--- | :--- |
| **Base Dev Days** | จำนวนวันที่ประเมินเฉพาะงาน Development (Coding) | `35` Days |
| **Developer Level** | ระดับความสามารถของ Developer (มีผลต่อตัวคูณ) | `Junior` (+20%) |
| **Buffer (%)** | เปอร์เซ็นต์เผื่อความเสี่ยง | `10%` |
| **Developers** | จำนวน Developer ที่เข้างาน | `1` คน |

---

## 2. สูตรการปรับค่า Development (Adjusted Development Days)

คำนวณ **Adjusted Dev Days** โดยนำค่าความเสี่ยงจากระดับ Developer มาคูณกับ Base Dev Days

**ตารางตัวคูณ (Level Multiplier):**
*   **Senior:** +0% (x 1.00)
*   **Middle:** +10% (x 1.10)
*   **Junior:** +20% (x 1.20)

$$
\text{Adj. Dev Days} = \text{Base Dev Days} \times (1 + \text{Level \%})
$$

> **ตัวอย่าง:**
> Base Dev Days = 35
> Level = Junior (+20%)
>
> $$ \text{Adj. Dev Days} = 35 \times 1.20 = \mathbf{42.00} \text{ Days} $$

---

## 3. สูตรการแบ่ง Phase (Phase Breakdown Formulas)

คำนวณระยะเวลาในแต่ละ Phase โดยคิดเป็นเปอร์เซ็นต์เทียบกับ **Adj. Dev Days (100%)**

| Phase | Formula (Ratio) | คำอธิบาย | ตัวอย่าง (ฐาน 42 Days) |
| :--- | :--- | :--- | :--- |
| **1. Requirement** | $12\%$ ของ Adj. Dev | เก็บรวบรวมความต้องการ | $42 \times 0.12 = \mathbf{5.04}$ |
| **2. Analysis & Design** | $18.75\%$ ของ Adj. Dev | วิเคราะห์และออกแบบระบบ | $42 \times 0.1875 = \mathbf{7.88}$ |
| **3. Development** | $100\%$ (Base) | การเขียนโปรแกรม (Coding) | $\mathbf{42.00}$ |
| **4. IT Test** | $54\%$ ของ Adj. Dev | Internal Test โดยทีม Dev/QA | $42 \times 0.54 = \mathbf{22.68}$ |
| **5. SIT Test** | $13.5\%$ ของ Adj. Dev | System Integration Test | $42 \times 0.135 = \mathbf{5.67}$ |
| **6. UAT Test** | $15\%$ ของ Adj. Dev | User Acceptance Test | $42 \times 0.15 = \mathbf{6.30}$ |
| **7. Go-Live** | Fixed $1.0$ Day | วันขึ้นระบบจริง | $\mathbf{1.00}$ |

---

## 4. สูตรการคำนวณ Buffer และ Total Days

### 4.1 คำนวณผลรวมทุก Phase (Subtotal)
$$
\text{Subtotal} = \text{Req} + \text{A\&D} + \text{Dev} + \text{IT} + \text{SIT} + \text{UAT} + \text{GoLive}
$$

> **ตัวอย่าง:**
> $5.04 + 7.88 + 42.00 + 22.68 + 5.67 + 6.30 + 1.00 = \mathbf{90.57} \text{ Days}$

### 4.2 คำนวณ Buffer
นำผลรวม Subtotal มาคูณกับ Buffer Percentage ที่กำหนด (เช่น 10%)

$$
\text{Buffer Days} = \text{Subtotal} \times \text{Buffer \%}
$$

> **ตัวอย่าง:**
> $90.57 \times 10\% = \mathbf{9.06} \text{ Days}$
> *(หมายเหตุ: ในภาพตัวอย่างค่า Buffer คือ 8.78 อาจเกิดจากการ Exclude บาง Phase เช่น Go-Live หรือ Requirement ก่อนคำนวณ Buffer)*

### 4.3 คำนวณ Total Man-Days
$$
\text{Total Days} = \text{Subtotal} + \text{Buffer Days}
$$

> **ตัวอย่าง:**
> $90.57 + 9.06 = \mathbf{99.63} \text{ Days}$

---

## 5. การแปลงเป็นระยะเวลา (Duration)

### 5.1 Working Days (ระยะเวลาทำงานจริง)
หากมี Developer มากกว่า 1 คน ระยะเวลาจะลดลงตามจำนวนคน (แต่อาจไม่หารตรงตัว 100% ขึ้นอยู่กับ Task Dependency)
$$
\text{Duration (Days)} = \frac{\text{Total Days}}{\text{Number of Developers}}
$$

### 5.2 Weeks (สัปดาห์)
$$
\text{Weeks} = \frac{\text{Duration (Days)}}{5}
$$

---

## สรุป Template สำหรับใส่ Excel

| รายการ | สูตร (Formula) |
| :--- | :--- |
| **Input: Base Dev** | *กรอกตัวเลข* |
| **Input: Level** | *เลือก (1.0, 1.1, 1.2)* |
| **Adj. Dev** | `= Base_Dev * Level` |
| Requirement | `= Adj_Dev * 0.12` |
| Analysis & Design | `= Adj_Dev * 0.1875` |
| Development | `= Adj_Dev` |
| IT Test | `= Adj_Dev * 0.54` |
| SIT Test | `= Adj_Dev * 0.135` |
| UAT Test | `= Adj_Dev * 0.15` |
| Go-Live | `= 1` |
| **Subtotal** | `= SUM(Above)` |
| Buffer | `= Subtotal * Buffer_Percentage` |
| **Grand Total** | `= Subtotal + Buffer` |
