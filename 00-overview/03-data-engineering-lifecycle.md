# Data Engineering Lifecycle

## วงจรชีวิตของข้อมูล
```
Generation → Ingestion → Storage → Transformation → Serving
(ข้อมูลเกิด)   (ดึงเข้ามา)   (จัดเก็บ)   (แปลง/ทำความสะอาด)   (ส่งมอบให้ผู้ใช้)
```

| ขั้น | ทำอะไร | เรียนต่อที่ |
|---|---|---|
| **Generation** | ข้อมูลเกิดขึ้นที่ระบบต้นทาง (แอป, IoT, API) | Relational Databases · NoSQL Databases |
| **Ingestion** | ดึงข้อมูลเข้าแพลตฟอร์ม แบบ batch หรือ streaming | ETL vs ELT · Change Data Capture · Apache Kafka |
| **Storage** | เลือกที่เก็บให้เหมาะกับการใช้งานและงบประมาณ | Cloud Data Warehouses · Data Lake & Lakehouse · File Formats |
| **Transformation** | clean, wrangle, join, aggregate, model | dbt · Apache Spark · Data Modeling |
| **Serving** | ส่งให้ BI, analytics, ML หรือระบบอื่น | Star Schema · Medallion Architecture |

> คอร์สอาจอธิบายเป็น **layer ของ data platform** คือ Ingestion → Storage & Integration → Processing → Analysis & UI โดยมี Data Pipeline layer ครอบทุกชั้น เป็นเรื่องเดียวกันแต่มองจากมุมสถาปัตยกรรม

## Undercurrents — เรื่องที่อยู่ในทุกขั้น
- **Security:** CIA triad = Confidentiality, Integrity, Availability → Data Governance & Security
- **Data management & governance:** ใครเข้าถึงอะไรได้, PDPA/GDPR, lineage → Data Lineage
- **Orchestration:** ควบคุมลำดับและเวลาการรัน → Apache Airflow
- **DataOps:** monitoring, testing, CI/CD → Data Quality & Testing · CI-CD for Data
- **Performance tuning & troubleshooting:** ทำให้ pipeline และ query เร็วและเสถียร → Partitioning

## การเลือก Data Store — ปัจจัยที่ต้องคิด
- ประเภทข้อมูล (structured / semi-structured / unstructured)
- ปริมาณและความเร็วของข้อมูล (volume, velocity)
- รูปแบบการใช้งาน: transaction (OLTP) หรือ analytics (OLAP) → OLTP vs OLAP
- ความต้องการด้าน consistency → ACID · CAP Theorem
- ความปลอดภัย ข้อกำหนดทางกฎหมาย และค่าใช้จ่าย

## Data Wrangling — ขั้นตอนหลัก
1. **Discovery:** สำรวจว่าข้อมูลมีอะไร มีปัญหาอะไร
2. **Structuring:** จัดรูปแบบ เช่น flatten JSON, pivot, join
3. **Cleaning:** จัดการค่าว่าง ค่าซ้ำ ค่าผิดปกติ และ format ที่ไม่ตรงกัน
4. **Enriching:** เพิ่มข้อมูลจากแหล่งอื่นให้มีบริบทมากขึ้น
5. **Validating:** ตรวจว่าผลลัพธ์ถูกต้องตามกฎ

## เจาะลึกแต่ละเรื่อง
- [Querying & Data Wrangling with SQL](04-querying-and-data-wrangling-with-sql.md)
- [Performance Tuning & Monitoring](05-performance-tuning-and-monitoring.md)
- [Data Governance & Compliance](06-data-governance-and-compliance.md)
- [DataOps](07-dataops.md)

## 🧠 Self-check
<details>
<summary><b>1. เรียงขั้นของ lifecycle และอธิบายสั้น ๆ แต่ละขั้น</b></summary>

Generation (ข้อมูลเกิด) → Ingestion (ดึงเข้า) → Storage (เก็บ) → Transformation (แปลง) → Serving (ส่งมอบ) โดยมี security, governance, orchestration และ DataOps อยู่ในทุกขั้น

</details>

<details>
<summary><b>2. CIA triad คืออะไร? ยกตัวอย่างแต่ละตัว</b></summary>

- **Confidentiality:** คนที่ไม่มีสิทธิ์ต้องเห็นข้อมูลไม่ได้ เช่น mask เลขบัตรประชาชน
- **Integrity:** ข้อมูลต้องไม่ถูกแก้ไขโดยไม่ได้รับอนุญาตหรือเสียหาย เช่น checksum, access control
- **Availability:** ข้อมูลต้องพร้อมใช้เมื่อต้องการ เช่น backup, replication

</details>

<details>
<summary><b>3. ข้อมูล sensor IoT 1 ล้าน event ต่อนาที ควร ingest แบบไหน และเก็บที่ไหน?</b></summary>

Ingest แบบ **streaming** ผ่าน Apache Kafka (หรือ Kinesis / Pub/Sub) แล้วเก็บข้อมูลดิบลง **data lake** เป็น Parquet ที่ partition ตามเวลา (Partitioning) จากนั้นค่อย aggregate ส่งเข้า warehouse สำหรับ dashboard ไม่ควรเขียนลง RDBMS ตรง ๆ เพราะรับ write ปริมาณขนาดนี้ได้ยาก

</details>

<details>
<summary><b>4. Data Wrangling ต่างจาก ETL อย่างไร?</b></summary>

Wrangling คือ **กิจกรรม** แปลงข้อมูลดิบให้ใช้งานได้ (มักทำแบบสำรวจทีละ dataset) ส่วน ETL คือ **กระบวนการอัตโนมัติ** ที่ทำซ้ำได้ ในทางปฏิบัติ DE จะนำขั้นตอน wrangling ที่ได้ไปเขียนเป็น pipeline ใน ETL/ELT

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
