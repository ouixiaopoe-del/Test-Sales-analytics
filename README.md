# retail. — Online Retail Dashboard

เว็บวิเคราะห์ยอดขายภาษาไทยจาก UCI Online Retail พร้อมกราฟ 4 ประเภทและสองเวอร์ชัน: **Chart.js 4.4.8 / Plotly.js 3.0.1**

สถานะ: พัฒนาและทดสอบในเครื่องแล้ว ยังไม่ได้สร้าง GitHub repository หรือเผยแพร่ URL สาธารณะ

- Repository: **เติม URL จริงหลังเผยแพร่**
- Live website: **เติม URL จริงหลังเปิด GitHub Pages**
- Chart.js: `<LIVE_URL>/web/chartjs/`
- Plotly.js: `<LIVE_URL>/web/plotly/`

## เปิดใน VS Code

1. แตก ZIP แล้วเปิดโฟลเดอร์ `online-retail` ที่มี `index.html` ด้วย File → Open Folder
2. เปิด Terminal → New Terminal แล้วรัน `python -m http.server 8000` (Windows ใช้ `py -m http.server 8000` ได้)
3. เปิด http://localhost:8000 และหยุดเซิร์ฟเวอร์ด้วย Ctrl+C

Windows: ดับเบิลคลิก `START_WINDOWS.bat` ได้เมื่อมี Python หากหน้าเว็บเปิดก่อนเซิร์ฟเวอร์พร้อม ให้กด Refresh

ไม่ต้อง npm install หรือ build ไลบรารี/ฟอนต์อยู่ใน `web/shared/vendor/` ทั้งหมด ไม่ต้องโหลด CDN ภายนอก แต่ต้องเปิดผ่าน HTTP ไม่ใช่ file://

## ฟังก์ชัน

- กราฟเส้นยอดขายเทียบยอดสุทธิรายเดือน คลิกเดือนเพื่อเจาะดูข้อมูล
- กราฟโดนัทสัดส่วน 5 ประเทศแรกและกลุ่มอื่น คลิกประเทศเพื่อกรอง ยกเว้น Other markets ซึ่งเป็นกลุ่มรวม
- กราฟแท่งรหัสสินค้า/ค่าบริการ 10 อันดับ คลิกดูรายละเอียด
- Scatter plot ราคาเฉลี่ยถ่วงน้ำหนักเทียบจำนวนขายทุกรหัส มีสเกล log
- ตัวกรองประเทศ เดือนเริ่มต้น/สิ้นสุด และไม่รวม Outlier ส่งผลต่อ KPI กราฟ และตาราง
- ค้นหาชื่อ/รหัสสินค้าเฉพาะตาราง เรียงลำดับยอดขาย/ยอดสุทธิ/จำนวน และแบ่งหน้า
- Export CSV ตามตัวกรอง คำค้นหา และลำดับตาราง พร้อมป้องกันข้อความสูตร CSV injection
- โหมดสว่าง/มืด จดจำค่าในเครื่อง และ responsive สำหรับมือถือ
- Plotly เพิ่ม zoom, pan, reset axes และ download PNG ผ่าน toolbar กราฟ
- ส่วนคุณภาพข้อมูล CRISP-DM อ้างอิง และรายงาน PDF

## โครงสร้าง

| Path | หน้าที่ |
|---|---|
| `index.html` | หน้าเริ่มต้น Chart.js |
| `web/chartjs/index.html` | เวอร์ชัน Chart.js |
| `web/plotly/index.html` | เวอร์ชัน Plotly.js |
| `web/shared/style.css` | ธีม responsive และโหมดมืด |
| `web/shared/app.js` | UI ตัวกรอง การคำนวณ ตาราง และ export |
| `web/shared/charts.js` | ตัววาดกราฟทั้งสองไลบรารี |
| `web/shared/vendor/` | ไลบรารี ฟอนต์ ใบอนุญาต |
| `web-data/cubes/` | ข้อมูลรายเดือน/ประเทศ/สินค้า/Outlier สำหรับเว็บ ประมาณ 4.3 MB ทั้งชุด |
| `web-data/dashboard.json` | ชื่อสินค้าและสรุปพื้นฐาน |
| `web-data/months/` | รายละเอียดรายการขายและยกเลิกเต็ม |
| `data/raw/Online_Retail.xlsx` | ต้นฉบับ 541,909 แถว |
| `data/cleaned/` | CSV หลังจัดรูปแบบและแยกประเภท |
| `data/audit/` | แถวซ้ำ กลุ่ม review และผลตรวจ |
| `scripts/clean_data.py` | ทำความสะอาดจาก Excel |
| `scripts/build_web_data.py` | สร้างข้อมูลสรุปสำหรับเว็บ |
| `docs/Process_Report.pdf` | รายงานพร้อมภาพหน้าเว็บ |
| `docs/Process_Report.html` | ต้นฉบับรายงานที่แก้ไขได้ |
| `docs/*test-results.json` | ผลทดสอบเบราว์เซอร์ที่สำเร็จจริง |
| `CLEANING_REPORT.md` | รายละเอียดการทำความสะอาด |
| `DATA_DICTIONARY.md` | ความหมายคอลัมน์ |
| `DATA_README.md` | คู่มือชุดข้อมูล |
| `PROMPTS.md` | คำขอผู้ใช้และส่วนที่ AI ช่วย |

## ข้อมูลและนิยาม

Chen, D. (2015). Online Retail. UCI Machine Learning Repository. https://archive.ics.uci.edu/dataset/352/online+retail
DOI: https://doi.org/10.24432/C5BW33 — CC BY 4.0 ต้องอ้างอิงต้นทาง

ต้นฉบับ 541,909 แถว 8 คอลัมน์ แยกแถวซ้ำ 5,268 แถว เหลือ sale 524,878 แถว cancellation 9,251 แถว และ review 2,512 แถว เก็บ audit ของรายการที่แยกออกทั้งหมด

ผลรวมเริ่มต้น: sales 10,642,110.804 GBP, cancellation −893,979.730 GBP, net 9,748,131.074 GBP และจำนวนขาย 5,572,420 หน่วย แสดงเงิน 2 ตำแหน่ง แต่คำนวณด้วย integer milli-GBP

ยอดขายรวมรหัสค่าจัดส่ง/ค่าบริการ ไม่ใช่กำไร ยอดสุทธิ = ยอดขาย + มูลค่ายกเลิกที่เป็นลบ ไม่เดารหัสลูกค้าที่ว่าง Outlier เก็บไว้เป็นค่าเริ่มต้น การไม่รวม Outlier เป็นมุมมองเสริม ไม่ใช่การยืนยันว่าข้อมูลผิด ธันวาคม 2011 มีถึงวันที่ 9 เท่านั้น ไม่ควรเทียบกับเดือนเต็มโดยตรง

ปีบนกราฟภาษาไทยแสดงตาม locale ไทย (พ.ศ. 2553–2554) แต่คีย์ JSON เป็น ค.ศ. 2010–2011 ไม่มีการเปลี่ยน timezone

## ทำข้อมูลซ้ำ

ติดตั้ง Python พร้อม pandas, numpy, openpyxl แล้วรัน:

```bash
python scripts/clean_data.py --input data/raw/Online_Retail.xlsx
python scripts/build_web_data.py
```

สคริปต์ออกแบบสำหรับ Online Retail ที่แนบ มี assertions ตรวจยอดอ้างอิง หากเปลี่ยน dataset ต้องตรวจและปรับยอดอ้างอิงอย่างมีเหตุผล

## เผยแพร่ GitHub Pages

1. สร้าง repository ว่างในบัญชีตนเอง ตั้งเป็น **Public** เช่น `online-retail-dashboard`
2. เปิด Terminal ที่โฟลเดอร์โปรเจกต์ ต้องมี Git แล้วรัน (แทน YOUR_USERNAME ด้วยบัญชีจริง):

```bash
git init
git add .
git commit -m "Add retail dashboard, cleaned data and report"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/online-retail-dashboard.git
git push -u origin main
```

3. GitHub → **Settings → Pages → Build and deployment → Deploy from a branch** เลือก **main / (root)** แล้ว Save
4. เมื่อเผยแพร่สำเร็จ ใช้ URL จริงที่ GitHub แสดง รูปแบบ `https://YOUR_USERNAME.github.io/online-retail-dashboard/`
5. เปิดทดสอบแบบไม่ล็อกอิน ทั้งสองเวอร์ชันและไฟล์ PDF
6. เติม URL จริงใน README และ `docs/Process_Report.html` เปิดรายงานผ่านเบราว์เซอร์แล้วพิมพ์เป็น PDF ขนาด A4 เปิด Background graphics ปิด Headers and footers บันทึกทับ `docs/Process_Report.pdf`
7. commit/push ไฟล์ที่อัปเดต แล้วส่ง repository URL, live URL และ PDF

ใช้ Git push ส่ง CSV ขนาดใหญ่ ไม่อัปโหลด ZIP ทั้งก้อนเป็น source code และไม่ใส่ API keys/passwords ใน repository

โจทย์ระบุเวลาอัปโหลดครั้งแรกก่อนเที่ยงวันสอบ และส่งช้าหักนาทีละ 1 คะแนน ตรวจเวลาจริงกับผู้สอน

## การทดสอบ

เทียบ KPI กับ pandas อิสระสำหรับทุกข้อมูล, France ม.ค.–มี.ค. 2011, UK แบบไม่รวม Outlier และกรณีไม่มีข้อมูล พร้อมทดสอบค้นหา modal export โหมดมืด และมือถือ 390 px รายละเอียดเบราว์เซอร์และผลจริงอยู่ใน docs ไม่อ้างว่าผ่าน Chrome รุ่นล่าสุดหากผลระบุ Chromium 133

ก่อนส่งยังต้องเผยแพร่ GitHub จริง ทดสอบ Chrome/Firefox บนเครื่องผู้ส่งงาน เปิดลิงก์แบบไม่ล็อกอิน และเติมผู้จัดทำ/รุ่น GPT/URL ตามจริง

## อ้างอิง

- Chart.js 4.4.8 — MIT: https://www.chartjs.org/docs/latest/
- Plotly.js 3.0.1 — MIT: https://plotly.com/javascript/
- Sarabun — SIL Open Font License 1.1: https://fontsource.org/fonts/sarabun
- GitHub Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
- โค้ดหน้าเว็บเขียนสำหรับงานนี้ด้วย ChatGPT ไม่คัดลอกโครงการทั้งชุด ดู PROMPTS.md

เก็บไฟล์ใบอนุญาตใน web/shared/vendor/ ไว้เมื่อเผยแพร่
