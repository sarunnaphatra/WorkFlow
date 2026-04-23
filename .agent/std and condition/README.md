# ✨ AdaPos+ Standard Conditions & Rates
> ศูนย์รวมมาตรฐานการคำนวณและเงื่อนไขตั้งต้นสำหรับโครงการประเมิน Man-Day (AdaPos+ Standard SDLC)

โฟลเดอร์นี้รวบรวมเอกสารการตั้งค่า (Configuration) และเงื่อนไขมาตรฐานที่จะถูกนำไปใช้ใน Workflow การประเมินผลทั้งหมด เพื่อให้มั่นใจว่าทีมงานทุกคนใช้ **"อัตราเดียวกัน มาตรฐานเดียวกัน"**

---

## 📂 รายชื่อไฟล์ที่สำคัญ
### 1. [Standard Rate Configuration.md](file:///c:/example/IDE/10.Project/2026/01.AdaPos+%20Interface%20STD/API%20WebApp/.agent/std%20and%20condition/Standard%20Rate%20Configuration.md)
*แหล่งอ้างอิงหลัก (Source of Truth) ของตัวเลขทั้งหมด*
- **SM Rate**: อัตราราคาขาย (7.5k - 15k) ตามตำแหน่ง
- **DI Rate**: อัตราต้นทุนภายใน (Internal Tracking)
- **Multipliers**: ตัวคูณความซับซ้อน (Req, Tester, SA, UAT) 10-35%
- **Dev Level**: Junior/Senior Adjustment (80-120%)
- **Holidays**: รายชื่อวันหยุดนักขัตฤกษ์สำหรับคำนวณ Timeline

---

## 🚀 วิธีการใช้งาน (สำหรับทีมงาน)
เมื่อมีการเริ่มการประเมินโครงการใหม่ผ่าน Workflow เหล่านี้ ระบบจะดึงค่าจากโฟลเดอร์นี้ไปใช้โดยอัตโนมัติ:

1.  **/ada-estimate-tool**: ใช้ประเมิน Man-Day แบบรวดเร็ว (Interactive) ผ่านแชท
2.  **/ada-estimate-md**: ใช้สร้างชุดเอกสาร SDLC ทั้ง 8 ฉบับ (รวม Man-Day Estimation เอกสารที่ 8)

---

## 🛠️ การแก้ไขมาตรฐาน (ADMIN Only)
หากมีการอัปเดตอัตราค่าจ้างหรือวันหยุดประจำปี:
- แก้ไขที่ไฟล์ `Standard Rate Configuration.md` เท่านั้น
- **ห้าม** แก้ไขสูตรคำนวณใน Workflow โดยตรง เพื่อป้องกันความสับสน

---
**Last Updated:** 2026-03-19
**Owner:** AdaPos+ Project Management Team
