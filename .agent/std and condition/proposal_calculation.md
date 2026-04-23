# สูตรการคำนวณ Proposal (ให้ได้ยอดตรงกับแอป)

เอกสารนี้สรุป “พารามิเตอร์ที่ต้องใช้” และ “สูตรการคำนวณ” เพื่อสร้างฟังก์ชันคำนวณยอด Proposal ให้ได้ผลลัพธ์ตรงกับแอป DEV Estimate MD Calculator

อ้างอิงโค้ด:
- [calculator.ts](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts)
- [ProposalTab.tsx](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/components/ProposalTab.tsx)
- [settings.ts](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/settings.ts)
- [calculator.ts (types)](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/calculator.ts)

---

## 1) อินพุตที่ต้องใช้ (Parameters)

### 1.1 InputParameters (ข้อมูลจากหน้ากรอกข้อมูล)
อ้างอิง [InputParameters](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/calculator.ts#L6-L18)

| ชื่อ | ชนิด | ความหมาย |
|---|---|---|
| devDays | number | จำนวนวัน Developer (ฐาน) |
| devCount | number | จำนวน Developer |
| saCount | number | จำนวน SA |
| testerCount | number | จำนวน Tester |
| devLevel | 'Junior' \| 'Standard' \| 'Senior' | ระดับ Developer (มีผลต่อการปรับ devDays) |
| complexity | 'Low' \| 'Medium' \| 'High' | ความซับซ้อน |
| projectSize | 'Small' \| 'Medium' \| 'Large' | ขนาดโครงการ (มีผลต่อ PM%) |
| hasInterface | boolean | มี Interface กับระบบอื่นหรือไม่ (มีผลต่อ SIT/IT Test) |
| bufferPercent | number | Buffer Percentage (%) |
| projectDuration | number | ระยะเวลาโครงการ (สัปดาห์) ถ้า 0 = Auto |
| projectStartDate | string (YYYY-MM-DD) | วันเริ่มโครงการ |

### 1.2 วันหยุด (Holidays)
อ้างอิง [Holiday](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/calculator.ts#L53-L59) และ [isHoliday](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L180-L191)

ใช้สำหรับ “การคำนวณไทม์ไลน์/วันทำงาน” ไม่ได้เปลี่ยนจำนวนวันใน Proposal โดยตรง แต่ส่งผลต่อช่วงวันที่และโหมด Manual

โครงสร้าง:
- date: YYYY-MM-DD
- countAsWorking: ถ้า true จะไม่นับเป็นวันหยุด

หมายเหตุ: เสาร์-อาทิตย์ถูกนับเป็นวันหยุดเสมอ

### 1.3 ค่าตั้งต้น/ปรับแต่ง (CustomPercentages / Rates)
อ้างอิง [CustomPercentages](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/settings.ts#L22-L60) และ [DEFAULT_PERCENTAGES](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/types/settings.ts#L88-L108)

ต้องมีอย่างน้อย:

1) devLevel factor (%)
- Junior / Standard / Senior (เช่น 120/100/80)

2) complexity percentages (%)
- requirement, tester, sa, uat, interface

3) projectSize percentages (%)
- Small / Medium / Large (ใช้เป็น PM%)

4) goLiveDays (จำนวนวัน Go-Live แบบคงที่)

5) Rates (อัตราค่าจ้างต่อวัน)
- DI Rate: diRates.{developer,tester,systemAnalyst,projectManager,implementer}
- SM Rate: smRates.{developer,tester,systemAnalyst,projectManager,implementer}

---

## 2) ขั้นตอนคำนวณ Man-Days / Timeline (เพื่อให้ได้ CalculationResult)

ฟังก์ชันหลัก: [calculateManDays](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L4-L94)

### 2.1 ปรับ devDays ตามระดับ Developer

```
adjustedDevDays = devDays * (devLevelFactor(devLevel) / 100)
```

### 2.2 ดึงเปอร์เซ็นต์ตาม Complexity และ Project Size

```
complexityPerc = complexity[complexity]
pmPercent = projectSize[projectSize] / 100
```

### 2.3 คำนวณ Effort ของแต่ละส่วน (หน่วยเป็น “Man-Days (Effort)”)

```
requirementEffort = adjustedDevDays * (complexityPerc.requirement / 100)
testerEffort      = adjustedDevDays * (complexityPerc.tester / 100)
saEffort          = (adjustedDevDays + testerEffort) * (complexityPerc.sa / 100)
pmEffort          = (adjustedDevDays + testerEffort + saEffort) * pmPercent
uatEffort         = adjustedDevDays * (complexityPerc.uat / 100)
interfaceEffort   = hasInterface
                   ? (adjustedDevDays + testerEffort + saEffort) * (complexityPerc.interface / 100)
                   : 0
goLiveDays        = goLiveDays (ค่าคงที่)
```

### 2.4 คำนวณ Buffer (Effort)

```
baseTotalEffort = requirementEffort
               + adjustedDevDays
               + testerEffort
               + saEffort
               + pmEffort
               + uatEffort
               + interfaceEffort
               + goLiveDays

bufferEffort = baseTotalEffort * (bufferPercent / 100)
```

### 2.5 แปลงเป็น Duration (หน่วยเป็น “วันตาม Timeline”)

จำนวนคนมีผลกับ Requirement/Analysis/Development/Testing ตามโค้ด:

```
requirementDuration  = requirementEffort / saCount
analysisDuration     = saEffort / saCount
developmentDuration  = adjustedDevDays / devCount
```

ส่วน Testing แบ่งตามกรณีมี Interface:

อ้างอิง:
- [calculateITTestDays](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L96-L99)
- [calculateSITTestDays](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L101-L104)

```
testBase = testerEffort + interfaceEffort + bufferEffort

ITTestEffort = hasInterface ? testBase * 0.8 : testBase
SITTestEffort = hasInterface ? testBase * 0.2 : 0

itTestDuration = ITTestEffort / testerCount
sitTestDuration = hasInterface ? (SITTestEffort / testerCount) : 0
```

ส่วน UAT และ Buffer ในโค้ดใช้เป็นจำนวนวันตรง ๆ (ไม่ได้หารด้วยจำนวนคน):

```
uatDuration = uatEffort
bufferDuration = bufferEffort
```

### 2.6 รวม Total Days (ที่นำไปโชว์และใช้หาร Cost per Day)

```
baseTotalDuration = requirementDuration
                 + analysisDuration
                 + developmentDuration
                 + itTestDuration
                 + sitTestDuration
                 + uatDuration
                 + goLiveDays

totalDays = baseTotalDuration + bufferDuration
```

### 2.7 สร้าง Timeline ด้วยวันทำงาน (ตัดวันหยุด/เสาร์-อาทิตย์)

อ้างอิง [calculateTimeline](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L106-L164)

การเดิน Phase:
- order: requirement → analysis → development → itTest → sitTest (ถ้ามี) → uat → goLive
- endDate ของแต่ละ phase = addWorkingDays(currentDate, ceil(phaseDays), holidays)
- currentDate ของ phase ถัดไป = endDate + 1 วันปฏิทิน

หมายเหตุ: buffer ถูกเก็บเป็น phase แยก แต่ไม่ได้ถูกวางลงใน timeline ช่วงวันที่จริงในฟังก์ชันนี้

### 2.8 Manual Mode (กำหนดระยะเวลาโครงการเป็นสัปดาห์)

เงื่อนไข: projectDuration > 0 (weeks)

แนวคิด: ปรับสัดส่วน phase ที่ปรับได้ให้พอดีกับจำนวน “วันทำงานที่มีอยู่” ภายในช่วงเวลาที่ผู้ใช้กำหนด โดยยกเว้น phase ที่ถือว่า fixed:
- fixed: development, goLive
- adjustable: requirement, analysis, itTest, sitTest, uat, buffer

สูตร:

```
manualEndDate = startDate + (projectDuration * 7 วันปฏิทิน)
availableWorkingDays = workingDays(startDate..manualEndDate)

fixedDays = development + goLive
adjustableTotal = sum(allPhaseDays) - fixedDays
availableForAdjustable = availableWorkingDays - fixedDays
adjustmentFactor = availableForAdjustable / adjustableTotal

สำหรับทุก phase ที่ไม่ใช่ development และ goLive:
  phaseDays[phase] = phaseDays[phase] * adjustmentFactor
```

---

## 3) สูตรคำนวณ Proposal (ยอดขาย/ต้นทุน)

แอปใช้งานจริงผ่านฟังก์ชัน:
- [calculateClientPrice](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L282-L327) (SM Rate)
- [calculateInternalCost](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L234-L279) (DI Rate)

> หมายเหตุ: ใน [ProposalTab.tsx](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/components/ProposalTab.tsx#L108-L156) มี calculateProposal() อีกชุด แต่ไม่ได้ถูกใช้เป็นผลลัพธ์หลักในการแสดงผล (ผลลัพธ์หลักคือ smProposal/diProposal จาก utils)

### 3.1 อินพุตที่ต้องใช้สำหรับ Proposal

1) CalculationResult (จาก calculateManDays)
- result.phases.*.days
- result.totals.totalDays
- result.counts.{developer,sa,tester}

2) projectSize และ projectSizePercentages
- projectSizePercentages[projectSize] เป็น % ของ PM (เช่น 8/10/15)

3) rates
- DI Rate หรือ SM Rate (บาท/วัน) แยกตาม role

### 3.2 คำนวณจำนวนวันต่อ Role (roles.* เป็น “Man-Days ของ role”)

อ้างอิง [calculateClientPrice](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/utils/calculator.ts#L299-L305)

มีการคูณจำนวนคนสำหรับ Developer/Tester/SA แต่ Implementer ไม่คูณจำนวนคน (ตามโค้ด):

```
developerDays = round2(result.phases.development.days * result.counts.developer)
testerDays = round2((result.phases.itTest.days + (result.phases.sitTest?.days ?? 0)) * result.counts.tester)
systemAnalystDays = round2(result.phases.analysis.days * result.counts.sa)
projectManagerDays = round2(result.totals.totalDays * (projectSizePercentages[projectSize] / 100))
implementerDays = round2(result.phases.uat.days + result.phases.goLive.days)
```

โดย round2(x) = Math.round(x * 100) / 100

### 3.3 คำนวณค่าใช้จ่ายต่อ Role และ Total

```
developerCost = developerDays * rate.developer
testerCost = testerDays * rate.tester
systemAnalystCost = systemAnalystDays * rate.systemAnalyst
projectManagerCost = projectManagerDays * rate.projectManager
implementerCost = implementerDays * rate.implementer

totalCost = developerCost + testerCost + systemAnalystCost + projectManagerCost + implementerCost
```

### 3.4 คำนวณสัดส่วน (%) ของแต่ละ Role

```
developerPct = (developerCost / totalCost) * 100
testerPct = (testerCost / totalCost) * 100
systemAnalystPct = (systemAnalystCost / totalCost) * 100
projectManagerPct = (projectManagerCost / totalCost) * 100
implementerPct = (implementerCost / totalCost) * 100
```

---

## 4) ค่าที่แสดงบนหน้าจอ Proposal Summary

อ้างอิง [ProposalTab.tsx](file:///c:/example/IDE/08QoDer/DEV-Estimate-MD-Calculator-main/src/components/ProposalTab.tsx#L610-L645)

### 4.1 Total Cost

```
TotalCost = selectedProposal.costs.total
```

### 4.2 Total Days

```
TotalDays = result.totals.totalDays
```

### 4.3 Cost per Day

```
CostPerDay = round0(TotalCost / TotalDays)
```

โดย round0(x) = Math.round(x)

---

## 5) ชุดฟังก์ชันขั้นต่ำที่ต้องมีเพื่อให้คำนวณตรงกับแอป

1) calculateManDays(params, holidays, customPercentages) → CalculationResult
- ควรทำให้ได้ค่า result.phases.*.days, result.totals.totalDays, result.counts.*

2) calculateClientPrice(result, smRates, projectSize, projectSizePercentages) → ProposalSummary
- ใช้สำหรับ “ยอดขาย (SM Rate)”

3) calculateInternalCost(result, diRates, projectSize, projectSizePercentages) → ProposalSummary
- ใช้สำหรับ “ต้นทุน (DI Rate)”

