# Querying & Data Wrangling with SQL

## คำสั่งพื้นฐานสำหรับสำรวจข้อมูล
| จุดประสงค์ | คำสั่ง | ใช้ทำอะไร |
|---|---|---|
| **นับ** | `COUNT(*)` | นับจำนวนแถวทั้งหมด |
| | `COUNT(DISTINCT col)` | นับจำนวนค่าที่ไม่ซ้ำ |
| **สรุปผล** | `SUM()`, `AVG()`, `STDDEV()` | ผลรวม ค่าเฉลี่ย ส่วนเบี่ยงเบนมาตรฐาน |
| **ค่าสุดขั้ว (Extreme values)** | `MAX()`, `MIN()` | หาค่าสูงสุดและต่ำสุดในคอลัมน์ |
| **Slicing** | `WHERE` | ตัดเฉพาะข้อมูลที่ตรงเงื่อนไข เช่น ช่วงราคา พื้นที่ |
| **เรียงลำดับ** | `ORDER BY ... ASC / DESC` | เรียงจากน้อยไปมาก / มากไปน้อย |
| **Filtering patterns** | `LIKE` + `%`, `_` | ค้นหาแบบ partial match |
| **จัดกลุ่ม** | `GROUP BY` | คำนวณ aggregate แยกตามกลุ่ม |

### Wildcards ของ `LIKE`
- `%` แทนตัวอักษรกี่ตัวก็ได้ (รวมถึง 0 ตัว) เช่น `LIKE '%bangkok%'`
- `_` แทนตัวอักษร 1 ตัวพอดี เช่น `LIKE 'A_C'` ตรงกับ `ABC`, `A1C`

## ตัวอย่าง: สำรวจตาราง `listings` (ประกาศขายบ้าน)
```sql
-- มีกี่แถว มีกี่เมือง
SELECT COUNT(*) AS total_rows,
       COUNT(DISTINCT city) AS total_cities
FROM listings;

-- ราคาต่ำสุด สูงสุด เฉลี่ย และความกระจายของแต่ละเมือง
SELECT city,
       MIN(price)    AS min_price,
       MAX(price)    AS max_price,
       AVG(price)    AS avg_price,
       STDDEV(price) AS sd_price
FROM listings
WHERE price BETWEEN 1000000 AND 10000000   -- slicing
GROUP BY city
ORDER BY avg_price DESC;

-- partial match: ชื่อโครงการที่มีคำว่า "condo"
SELECT * FROM listings
WHERE project_name LIKE '%condo%';
```

## Key Concepts
- **Outlier (ค่าผิดปกติ):** ข้อมูลที่ต่างจากข้อมูลส่วนใหญ่ในชุดเดียวกันอย่างมาก หาได้จาก `MIN` / `MAX` และ `STDDEV` เช่น ค่าที่ห่างจากค่าเฉลี่ยเกิน 3 เท่าของ SD
- **Partial match:** ทำได้ด้วย filtering patterns (`LIKE '%keyword%'`)

> [!TIP]
> **ใช้ในงาน DE อย่างไร**
> query ชุดนี้คือขั้น **Discovery** ของ data wrangling (ดู [Data Engineering Lifecycle](03-data-engineering-lifecycle.md)) ก่อนเขียน pipeline ต้องรู้ก่อนว่าข้อมูลมีค่าว่าง ค่าซ้ำ หรือ outlier ไหม และต่อยอดเป็น test อัตโนมัติได้ใน Data Quality & Testing

## 🧠 Self-check
<details>
<summary><b>1. `COUNT(*)` กับ `COUNT(DISTINCT customer_id)` ต่างกันอย่างไร?</b></summary>

`COUNT(*)` นับทุกแถว ส่วน `COUNT(DISTINCT customer_id)` นับจำนวนลูกค้าที่ไม่ซ้ำ ถ้าลูกค้า 1 คนสั่ง 5 order ค่าแรกจะนับ 5 ค่าหลังนับ 1
(เกร็ด: `COUNT(column)` จะไม่นับแถวที่คอลัมน์นั้นเป็น `NULL`)

</details>

<details>
<summary><b>2. ถ้าอยากหาอีเมลที่ลงท้ายด้วย `@gmail.com` เขียน `WHERE` อย่างไร?</b></summary>

`WHERE email LIKE '%@gmail.com'`

</details>

<details>
<summary><b>3. `WHERE` กับ `HAVING` ต่างกันอย่างไร?</b></summary>

`WHERE` กรอง **แถว** ก่อนจัดกลุ่ม ส่วน `HAVING` กรอง **กลุ่ม** หลัง `GROUP BY` เช่น `HAVING COUNT(*) > 10` (ยังไม่มีในเนื้อหาคอร์ส แต่เป็นคำถามสัมภาษณ์ยอดฮิต)

</details>

<details>
<summary><b>4. จะใช้ SQL หา outlier ของราคาได้อย่างไรแบบง่าย ๆ?</b></summary>

เทียบกับค่าเฉลี่ยและ SD เช่น หาแถวที่ `price > AVG(price) + 3 * STDDEV(price)` หรือดู `MIN` / `MAX` ว่ามีค่าที่เป็นไปไม่ได้ไหม (เช่น ราคาติดลบ)

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
