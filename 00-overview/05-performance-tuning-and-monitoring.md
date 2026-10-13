# Performance Tuning & Monitoring

## Metrics หลักที่ต้องเฝ้าดู
| Metric | ความหมาย | ตัวอย่าง |
|---|---|---|
| **Latency** | ใช้เวลานานแค่ไหน | pipeline รายวันใช้ 40 นาที จากปกติ 10 นาที |
| **Failure rate** | ล้มเหลวบ่อยแค่ไหน | 3 ใน 100 run ล้มเหลว |
| **Resource utilization** | ใช้ CPU / memory / disk เท่าไหร่ | memory ใช้ 95% เสี่ยงล่ม |
| **Traffic volume** | ปริมาณข้อมูลหรือ request ที่วิ่งผ่าน | วันนี้ข้อมูลเข้ามาแค่ 10% ของปกติ → source มีปัญหา? |

## Database Tuning
- **Indexing:** สร้าง index ให้ database หาแถวที่ต้องการได้โดยไม่ต้องอ่านทั้งตาราง ลดการอ่าน disk → Relational Databases
- **Partitioning:** แบ่งตารางใหญ่เป็นส่วนย่อย query จึงอ่านเฉพาะส่วนที่เกี่ยวข้อง → Partitioning
- **Normalization:** ลดความซ้ำซ้อน ป้องกัน anomaly ตอน insert / update / delete → Normalization

> [!WARNING]
> - **Partitioning:** คอร์สอธิบายว่า "แบ่งตารางใหญ่เป็นตารางย่อย" แต่ในทางปฏิบัติ ผู้ใช้ยังเห็นเป็น **ตารางเดียว** ระบบแค่แบ่งเก็บข้อมูลไว้เป็นส่วน ๆ ข้างใน (เช่น แยกตามเดือน) แล้วข้ามส่วนที่ไม่เกี่ยวตอน query
> - **Normalization:** ช่วยเรื่อง **ความถูกต้อง** ของข้อมูลมากกว่าความเร็ว ฝั่ง analytics มักทำกลับกัน (denormalize เป็น Star Schema) เพื่อให้อ่านเร็ว เพราะลดจำนวน join
> - **Index:** ช่วยให้อ่านเร็วขึ้น แต่ทำให้ insert / update ช้าลง และกินพื้นที่เพิ่ม ไม่ควรสร้างทุกคอลัมน์

## Monitoring & Maintenance
- **Job-level runtime monitoring:** แบ่ง job ออกเป็น logical steps แล้วติดตามสถานะและเวลาของแต่ละขั้นจนเสร็จ ทำให้รู้ว่าช้าหรือพังที่ขั้นไหน (Airflow ทำแบบนี้ผ่านหน้า UI ของแต่ละ task → Apache Airflow)
- **Preventive maintenance:**
  - *Time-based:* ทำตามรอบเวลา เช่น rebuild index ทุกวันอาทิตย์
  - *Condition-based:* ทำเมื่อเข้าเงื่อนไข เช่น disk เต็มเกิน 80% แล้วค่อยล้างข้อมูลเก่า

## 🧠 Self-check
<details>
<summary><b>1. pipeline ปกติใช้ 10 นาที วันนี้ใช้ 2 ชั่วโมง ควรดู metric ไหนก่อน?</b></summary>

ดู **traffic volume** ว่าข้อมูลเข้ามามากผิดปกติไหม และดู **resource utilization** ว่า CPU / memory เต็มหรือไม่ จากนั้นใช้ **job-level monitoring** หาว่าขั้นไหนเป็นตัวที่ช้า

</details>

<details>
<summary><b>2. ทำไมไม่สร้าง index ทุกคอลัมน์ไปเลย?</b></summary>

index ทุกตัวต้องอัปเดตทุกครั้งที่ insert / update / delete ทำให้เขียนช้าลงและกินพื้นที่ disk ควรสร้างเฉพาะคอลัมน์ที่ใช้ใน `WHERE`, `JOIN` หรือ `ORDER BY` บ่อย

</details>

<details>
<summary><b>3. Time-based กับ condition-based maintenance ต่างกันอย่างไร?</b></summary>

Time-based ทำตามรอบเวลาที่กำหนด ไม่สนสภาพระบบ ส่วน condition-based ทำเมื่อระบบเข้าเงื่อนไขที่ตั้งไว้ (ต้องมี monitoring ก่อน)

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
