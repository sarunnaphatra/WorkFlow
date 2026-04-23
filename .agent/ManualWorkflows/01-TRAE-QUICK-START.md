---
description: คู่มือเริ่มต้นใช้งาน Trae แบบด่วน เข้าใจง่าย ใช้งานได้ทันที
---

# 🚀 Trae Quick Start Guide

คู่มือสรุปการใช้งาน Trae IDE เพื่อให้คุณเริ่มต้นทำงานกับ AI Assistant ได้อย่างมีประสิทธิภาพและรวดเร็ว

## 💡 แนวคิดหลัก (Core Concepts)
- **AI-Native IDE**: Trae ไม่ใช่แค่ Editor แต่เป็นสภาพแวดล้อมที่ AI เข้าใจ Context ของทั้งโปรเจกต์
- **Context is King**: ยิ่ง AI เห็นไฟล์ที่เกี่ยวข้องมากเท่าไหร่ คำตอบจะยิ่งแม่นยำขึ้น
- **Collaborative Coding**: ทำงานร่วมกับ AI เหมือนเป็น Partner (Pair Programming)

## 🛠️ โหมดการใช้งานหลัก
1. **Chat Mode (Ctrl + L)**: ใช้ถามคำถามทั่วไป, ขอคำอธิบายโค้ด หรือวางแผนงาน
2. **Inline Edit (Ctrl + I)**: ใช้แก้ไขโค้ดในบรรทัดที่เลือกโดยตรง
3. **Builder Mode / Agent Mode**: สั่งให้ AI จัดการงานที่ซับซ้อนหลายขั้นตอน (Multi-step tasks)

## 🏃 ขั้นตอนการใช้งานให้ไว (Fast Workflow)
1. **Index Project**: เมื่อเปิดโปรเจกต์ใหม่ ให้รอ Trae ทำ Indexing จนเสร็จเพื่อให้ AI รู้จักไฟล์ทั้งหมด
2. **เลือก Context**: 
   - กด `#` ใน Chat เพื่อระบุไฟล์ที่ต้องการให้ AI ดู
   - ลากโค้ดที่ต้องการถามแล้วกด `Ctrl + L`
3. **สั่งงานด้วยภาษาธรรมชาติ**: 
   - "ช่วยเพิ่มปุ่ม Login ที่หน้า Home โดยใช้ Tailwind CSS"
   - "Refactor ฟังก์ชันนี้ให้รองรับ Async/Await"
4. **ตรวจสอบและยอมรับ (Review & Accept)**: 
   - ดูโค้ดที่ AI เสนอ (Diff View)
   - กด `Accept` เพื่อบันทึก หรือ `Reject` เพื่อยกเลิก

## ⚠️ ข้อควรระวัง
- **Verify Always**: ตรวจสอบโค้ดที่ AI เขียนเสมอ โดยเฉพาะ Logic ที่ซับซ้อน
- **Terminal Access**: Trae สามารถรันคำสั่งใน Terminal ได้ (เช่น npm install) ควรดูคำสั่งก่อนกดรัน
- **Keep it Simple**: สั่งงานทีละส่วน (Atomic Tasks) จะได้ผลลัพธ์ที่ดีกว่าสั่งงานใหญ่ทีเดียว
