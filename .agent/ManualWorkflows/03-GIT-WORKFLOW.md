---
description: แนวทางการใช้งาน Git ร่วมกับ Trae เพื่อการจัดการ Source Code ที่ดี
---

# 🌿 Git Workflow with Trae

การจัดการเวอร์ชันของโค้ดเมื่อทำงานร่วมกับ AI เพื่อให้ประวัติการแก้ไขสะอาดและติดตามง่าย

## 🚀 ขั้นตอนการจัดการ Git

### 1. ก่อนเริ่มงาน (Pre-task)
- ตรวจสอบว่าอยู่บน Branch ที่ถูกต้อง
- สั่ง AI: "ตรวจสอบสถานะ Git ปัจจุบันหน่อย"

### 2. การ Commit งาน (Committing)
- **Conventional Commits**: ใช้รูปแบบมาตรฐาน เช่น `feat:`, `fix:`, `docs:`, `chore:`
- **AI-Generated Message**: ให้ Trae ช่วยเขียน Commit Message ให้ได้โดยกดปุ่ม AI ในหน้า Source Control
- **กฎเหล็ก**: 1 Commit ต่อ 1 การเปลี่ยนแปลงที่สำคัญ (อย่ารวมหลายฟีเจอร์ใน 1 Commit)

### 3. การแก้ไขข้อขัดแย้ง (Merge Conflicts)
- เมื่อเกิด Conflict ให้เปิดไฟล์นั้น
- ใช้ Trae ในการวิเคราะห์ว่าควรเลือกโค้ดส่วนไหน (Accept Incoming หรือ Current) หรือให้ AI ช่วยรวมโค้ด (Smart Merge)

### 4. การส่งงาน (Pushing)
- ตรวจสอบความถูกต้องครั้งสุดท้ายก่อน Push
- **Prompt**: "ช่วยสรุปการเปลี่ยนแปลงทั้งหมดที่ฉันทำในวันนี้หน่อย เพื่อนำไปเขียน PR Description"

## 📝 ตัวอย่าง Commit Message ที่ดี
- `feat: add user authentication with JWT`
- `fix: resolve memory leak in data fetching`
- `docs: update Trae workflow manual`
- `refactor: simplify database connection logic`

## 🛠️ คำสั่งที่ใช้บ่อยใน Trae Terminal
- `git status` : เช็คสถานะไฟล์
- `git add .` : เตรียมไฟล์ทั้งหมด
- `git commit -m "message"` : บันทึกการเปลี่ยนแปลง
- `git push origin main` : ส่งโค้ดขึ้น GitHub
