# Data dictionary

| Column | Type | Meaning |
|---|---|---|
| SourceRow | integer | เลขแถว Excel ต้นฉบับ เริ่ม 2 เพราะแถว 1 เป็นหัวตาราง |
| InvoiceNo | string | เลขใบแจ้งหนี้ เก็บเป็นข้อความ C คือยกเลิก |
| StockCode | string | รหัสสินค้า เก็บอักษรและตัวเลข |
| Description | string | ชื่อสินค้าหลังจัดช่องว่าง หากว่างใช้ Unknown product พร้อมธง |
| Quantity | integer | จำนวนสินค้า ติดลบในรายการยกเลิก |
| InvoiceDate | ISO local datetime | YYYY-MM-DDTHH:mm:ss ไม่มี timezone |
| UnitPrice | number | ราคาต่อหน่วย GBP เก็บความละเอียดต้นฉบับ |
| CustomerID | nullable string | รหัสลูกค้า ห้ามเติมศูนย์หรือเดารหัส |
| Country | string | ประเทศตามต้นฉบับ ตัดช่องว่างหัวท้าย |
| DescriptionMissing | boolean | ชื่อสินค้าขาดหายก่อนใส่ป้าย Unknown product |
| CustomerMissing | boolean | ไม่มีรหัสลูกค้า |
| CancellationFlag | boolean | InvoiceNo ขึ้นต้น C |
| RecordType | enum | sale, cancellation, review |
| ReviewReason | string | เหตุผลที่แยกตรวจสอบ คั่นด้วย semicolon |
| LineAmountMilliGBP | integer | Quantity × UnitPrice × 1000 ใช้รวมยอดอย่างแม่นยำ |
| LineAmountGBP | number | มูลค่ารายการ GBP ก่อนปัดแสดงผล |
| Date | string | YYYY-MM-DD |
| YearMonth | string | YYYY-MM |
| QuantityOutlier | boolean | ขนาดจำนวนอยู่นอกเกณฑ์ IQR ของรายการขาย |
| PriceOutlier | boolean | ราคาอยู่นอกเกณฑ์ IQR ของรายการขาย |
| NonStandardStockCode | boolean | รหัสไม่ตรง regex ตัวเลข 5 หลักตามด้วยตัวอักษร A-Z ตั้งแต่ 0 ตัว |

ไฟล์ JSON รายเดือนมีคอลัมน์ย่อยตาม manifest ซึ่งเป็นแหล่งอ้างอิงลำดับจริง คอลัมน์คำนวณวันที่บางส่วนตัดออกเพื่อลดขนาด สามารถใช้ InvoiceDate.slice(0, 10) และ slice(0, 7) ได้

ตารางสรุป: SalesMilliGBP คือยอดขายบวก, CancellationMilliGBP คือยอดยกเลิกติดลบ, NetMilliGBP คือผลรวมสองค่า, SalesUnits คือจำนวนขาย, CancellationUnits คือขนาดจำนวนยกเลิกเป็นบวก, Lines คือจำนวนรายการสินค้า ตารางรายวัน/เดือนแยกตามประเทศ และ products สรุปรวมทุกช่วงเวลา ไม่มีการนับจำนวนลูกค้าจากผลบวกข้ามกลุ่ม
