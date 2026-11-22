# SAKO ADMISSION 2570 — GitHub Pages + Google Apps Script

**ไม่ใช้ Node.js / PHP**. GitHub Pages เป็นหน้าเว็บไซต์ PWA ส่วน Google Apps Script เป็น Web App รับสมัครจริงและบันทึก Google Sheets/Drive

## 1) ติดตั้ง Backend
1. เข้า https://script.google.com สร้างโปรเจกต์ใหม่ในบัญชีที่มีสิทธิ์แก้ไขโฟลเดอร์ Google Drive เป้าหมาย
2. นำเนื้อหา `apps-script-backend/Code.gs` ไปวางในไฟล์ `Code.gs` และสร้าง HTML file ชื่อ `Index` นำเนื้อหา `apps-script-backend/Index.html` ไปวาง
3. ตรวจสอบ FOLDER_ID ใน Code.gs = `1wtahyDl8MA-mcIiHoH38h1rfH9X6oPo9`
4. เรียก `setup()` จาก Apps Script editor ครั้งเดียว เพื่อสร้าง Google Sheet ในโฟลเดอร์และอนุญาตสิทธิ์
5. เรียก `setupAdmin('Admin1','รหัสผ่านใหม่ที่แข็งแรงอย่างน้อย12ตัว')` และ `setupAdmin('Admin2','รหัสผ่านอีกชุด')` จาก Apps Script editor; **อย่าบันทึกรหัสผ่านจริงในไฟล์หรือ GitHub**. Admin1 คือ นายไพรัช บินเสะ (ผู้อำนวยการ) และ Admin2 คือ นางนูรรานี มูลลาวัล (เจ้าหน้าที่รับสมัคร)
6. Deploy > New deployment > Web app > Execute as Me; เลือกสิทธิ์เข้าถึงตามนโยบายโรงเรียนและทดสอบการเปิดจากผู้ปกครองที่ไม่ล็อกอิน Google. การเปิด Anyone ให้เข้าถึง Web App เป็นสาธารณะ ต้องมีมาตรการป้องกัน abuse และทดสอบความปลอดภัยก่อนใช้งานจริง
7. คัดลอก Web App URL ที่ลงท้าย `/exec`

## 2) ติดตั้ง GitHub Pages
1. แก้ไข `config.js` ใส่ URL ที่คัดลอก เช่น `window.SAKO_BACKEND_URL = 'https://script.google.com/macros/s/XXXX/exec';`
2. อัปโหลดไฟล์ `index.html`, `style.css`, `app.js`, `config.js`, `manifest.webmanifest`, `sw.js`, `.nojekyll` และโฟลเดอร์ `assets/` ไปที่ root ของ GitHub repository. **ไม่ต้องอัปโหลดโฟลเดอร์ `apps-script-backend` ก็ได้**
3. Settings > Pages > Deploy from a branch > main > /(root)
4. เปิด https://USERNAME.github.io/REPOSITORY/ ทดสอบปุ่มเข้าสมัครซึ่งจะเปิดหน้า Apps Script

## ข้อจำกัดที่ต้องทราบ
- GitHub Pages ไม่สามารถรัน Apps Script หรือเก็บฐานข้อมูลเอง จึงต้อง Deploy Backend แยก
- ปุ่มบน GitHub จะนำไปยัง Web App ของ Google Apps Script ซึ่งมีหน้ารับสมัครและผู้ดูแล ไม่ใช่การส่งข้อมูลจาก JavaScript บน GitHub โดยตรง
- Backend ที่รวมเป็น **ต้นแบบสำหรับทดสอบ** ยังต้องผ่านการตรวจสอบความปลอดภัยก่อนรับข้อมูลเด็กจริง: rate limiting/anti-bot, การจัดการ session, การป้องกันการส่งข้อมูลซ้ำ, logging, file validation, การเข้าถึงเอกสาร, การเก็บรักษา/ลบข้อมูล, ประกาศความเป็นส่วนตัว, การตรวจสอบขั้นตอนอนุมัติ
- อย่าใช้รหัสผ่านที่เคยส่งในแชตเป็นรหัสจริง ให้เปลี่ยนเป็นรหัสใหม่ที่ต่างกัน
- การพิมพ์ A4 TH Sarabun 20/18/16 pt ยังไม่ได้รวมใน Backend นี้อย่างครบถ้วน ต้องพัฒนาและทดสอบเพิ่ม
- รูปแบนเนอร์เป็นภาพสร้างใหม่จากภาพอ้างอิง ควรตรวจความถูกต้องของข้อความ/บุคคลก่อนเผยแพร่
- หากยังไม่ Deploy Backend ปุ่มสมัคร/ติดตาม/Admin จะถูกปิดไว้ เพื่อไม่ให้ผู้ปกครองเข้าใจผิดว่าส่งใบสมัครได้แล้ว


## ข้อกำหนดการแนบไฟล์ (ปรับปรุงล่าสุด)
- บังคับแนบ **รูปถ่ายผู้สมัคร** 1 รูป (JPG, PNG หรือ WEBP ขนาดไม่เกิน 4 MB)
- ไม่มีช่องแนบหลักฐานหรือเอกสารอื่น และ backend ไม่บันทึกเอกสารประกอบ
- ลายมือชื่อผู้ปกครองยังคงเป็นช่องลงลายมือชื่อบนหน้าจอ ไม่ใช่การแนบเอกสาร


## เพิ่มระดับชั้นอนุบาล 1 (3 ขวบ)
ระบบรองรับ อ.1 (3 ขวบ) ทั้งหน้ารับสมัครและการตรวจสอบระดับชั้นฝั่ง Apps Script โดยยังบังคับแนบรูปถ่ายผู้สมัครเพียงอย่างเดียว
