# ระบบเงินเดือน (Google Apps Script)

ระบบเงินเดือนพนักงานรายชั่วโมง/Part-time บน Google Apps Script + Google Sheet + หน้าเว็บ
ไม่มีชื่อบริษัท/โลโก้ฝังในโค้ด — ตั้งเองในเมนู **ตั้งค่าระบบ** หลังติดตั้ง

| ไฟล์ | หน้าที่ |
|---|---|
| `Code.gs` | ฝั่งเซิร์ฟเวอร์: ชีต/โฟลเดอร์, API, คำนวณเงิน, สลิป, โลโก้ |
| `Index.html` | หน้าเว็บทั้งระบบ (16 เมนู) |
| `TimesheetOcr.gs` | AI (Gemini) อ่านรูปใบลงเวลา — ต้องตั้ง Script Property `GEMINI_API_KEY` |
| `appsscript.json` · `.claspignore` | manifest (Asia/Bangkok, V8, web app) · push เฉพาะ 4 ไฟล์ |
| `tools/make-preview.js` | `node tools/make-preview.js` → `preview.html` ดูหน้าเว็บด้วยข้อมูลสมมติ |

## ติดตั้ง
```bash
cp appsscript.json appsscript.json.bak
npx clasp login
npx clasp create-script --type standalone --title "ระบบเงินเดือน"
cp appsscript.json.bak appsscript.json
npx clasp push -f                              # ต้องเห็น "Pushed 4 files"
npx clasp create-deployment -d "ติดตั้งครั้งแรก"   # เก็บ deploymentId ไว้ redeploy ด้วย -i
```
1. เปิดเอดิเตอร์ → Run `setup` → อนุญาตสิทธิ์ (ก่อนทำ /exec จะตอบ 403)
2. เปิด URL /exec → ตั้งค่าระบบ: โลโก้ · ข้อมูลบริษัท · กติกาคำนวณ (ชม.ปกติ, ตัวคูณ OT, พัก, OT ขั้นต่ำ)
3. ตำแหน่งงาน → พนักงาน → พิมพ์ใบลงเวลา → เริ่มใช้งาน
4. (AI อ่านใบลงเวลา) สร้างคีย์ที่ https://aistudio.google.com/apikey ในโปรเจกต์ใหม่ →
   Script Properties `GEMINI_API_KEY` → หน้าตั้งค่ากด **ทดสอบคีย์ AI** ต้องขึ้น ✓
5. (บอท LINE) Run `setupBotSecret` → นำรหัสจาก Execution log ไปตั้งฝั่งบอท

> เว็บเปิดไม่ต้องล็อกอินและรันสิทธิ์เจ้าของ — แจกลิงก์ /exec เฉพาะคนที่ไว้ใจ

`docs/shot-*.png` = ภาพหน้าจอจากการทดสอบ UI บนพรีวิว
