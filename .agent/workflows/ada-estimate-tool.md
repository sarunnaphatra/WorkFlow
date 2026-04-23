---
description: Interactive AdaPos+ estimation tool with sequential input validation and calculation.
---

# /ada-estimate-tool - AdaPos+ Estimation Workflow

This workflow provides a step-by-step interactive process to calculate project estimates for AdaPos+ Interface projects based on official duration-based formulas.

## Guardrails
- **Mandatory Confirmation**: You MUST wait for user confirmation after each input parameter before proceeding to the next.
- **Source of Truth**: Refer to `@/.agent/std and condition/Standard Rate Configuration.md` for ALL rates, multipliers, and holidays.
- **Accuracy**: Final total must match the exact sum of all calculated items.
- **File Output**: Always save the final breakdown in a file ending in `_rev.md`.

## Steps

### 1. Load Initial Conditions (Automatic)
Before starting, read **Standard Rate Configuration.md** to load:
- [x] **SM Rates** (7,500 - 15,000)
- [x] **Complexity Effort Ratios** (Req%, Tester%, etc.)
- [x] **PM Ratios** (Small, Medium, Large)
- [x] **Dev Multipliers** (Junior, Senior)
- [x] **Official Holiday List**

### 2. Interactive Parameters
Ask the following questions one by one and wait for confirmation:
1.  **Project Name**: (e.g., "NetSuite Interface").
2.  **Base Development Days**: 
    - Enter number or provide **Markdown File**.
3.  **Team Composition**: (Critical for SIT/UAT planning)
    - `devCount`: Number of Developers | `testerCount`: Number of Testers.
4.  **Factors & Complexity**:
    - **Dev Level**: `Junior, Standard, Senior`.
    - **Complexity**: `Low, Medium, High`.
    - **hasInterface**: Does it interface with other systems? (Yes/No).
    - **bufferPercent**: Buffer percentage (Default: 15%).
5.  **Project Scheduling**:
    - **Project Size**: `Small, Medium, Large`.
    - **Project Start Date**: `YYYY-MM-DD`.

### 3. Calculation Methodology (Math Traceability)
Execute code using `Standard Rate Configuration.md` values:
1.  **Efforts**:
    - `testerEffort = dev_days * Tester%`
    - `saEffort = (dev_days + testerEffort) * SA%`
    - `interfaceEffort = (dev_days + testerEffort + saEffort) * Interface% (if hasInterface)`
2.  **Buffer & Multipliers**: 
    - `baseEffort = Req + Dev + Tester + SA + Interface + UAT + 1 day (Go-Live)`
    - `bufferEffort = baseEffort * bufferPercent`
3.  **Holidays & Timeline**:
    - ข้ามวันหยุดนักขัตฤกษ์ (ตามที่ระบุใน Config) และวันหยุดเสาร์-อาทิตย์ เพื่อคำนวณ **Launch Date**.
4.  **SM & DI Pricing**:
    - **SM Cost**: ใช้ `SM Rate` ประเมินราคาขาย.
    - **DI Cost**: (Optional) ใช้ `DI Rate` ประเมินต้นทุนภายใน.

### 4. Review & Generation
- **Display Table**: Show all inputs and calculated efforts.
- **Confirmation**: "ยืนยันผลการประเมินนี้หรือไม่?"
- **Output**: Save to `{Project_Name}_rev.md` with explicit math audit trail.

## Reference Documents
- `Standard Rate Configuration.md`: Central config for rates/multipliers.
- `adapos` (Skill): POS & Interface expertise.
