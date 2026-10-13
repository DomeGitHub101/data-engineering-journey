# Data Engineering Ecosystem

## ภาพรวม
Ecosystem ของ DE ประกอบด้วย **ข้อมูล (data)**, **แหล่งข้อมูล (sources)**, **ที่เก็บ (repositories)**, **pipelines & integration**, **ภาษา (languages)** และ **เครื่องมือ/แพลตฟอร์ม (tools & platforms)**

## 1. ประเภทข้อมูล
| ประเภท | ลักษณะ | ตัวอย่าง |
|---|---|---|
| **Structured** | มี schema ชัดเจน เป็นแถวและคอลัมน์ | ตารางใน PostgreSQL, Excel |
| **Semi-structured** | มีโครงสร้างบางส่วน ใช้ tag หรือ key | JSON, XML, email |
| **Unstructured** | ไม่มีโครงสร้างตายตัว | รูปภาพ, วิดีโอ, PDF, ข้อความโซเชียล |

## 2. File Formats ที่พบบ่อย
CSV/TSV, JSON, XML, Excel, Parquet, Avro → เจาะลึกที่ File Formats

## 3. แหล่งข้อมูล (Data Sources)
- Relational database ของระบบงาน → Relational Databases
- API / Web services
- Web scraping
- Data streams (IoT, clickstream, log) → Apache Kafka
- Flat files และ spreadsheet
- Public / open data (เช่น data.go.th)

## 4. Data Repositories (ที่เก็บข้อมูล)
| Repository | ใช้ทำอะไร | ลิงก์ |
|---|---|---|
| **RDBMS** | งาน transaction ของระบบหน้าบ้าน (OLTP) | Relational Databases · OLTP vs OLAP |
| **NoSQL** | ข้อมูลยืดหยุ่น scale สูง (document, key-value, wide-column, graph) | NoSQL Databases |
| **Data Warehouse** | ข้อมูลที่ clean แล้ว พร้อมวิเคราะห์ (OLAP) | Data Warehouse Concepts |
| **Data Mart** | warehouse ย่อยสำหรับแผนกเดียว | Data Warehouse Concepts |
| **Data Lake** | ข้อมูลดิบทุกรูปแบบ เก็บไว้ก่อน ใช้ทีหลัง | Data Lake & Lakehouse |
| **Lakehouse** | รวมข้อดีของ lake และ warehouse | Data Lake & Lakehouse · Medallion Architecture |

## 5. Pipelines & Integration
- **ETL / ELT:** ดึง แปลง โหลด → ETL vs ELT
- **Data Pipeline:** กระบวนการย้ายข้อมูลอัตโนมัติ ทั้งแบบ batch และ streaming
- **Data Integration:** รวมข้อมูลจากหลายแหล่งให้เป็นภาพเดียวกัน

## 6. ภาษา
- **Query language:** SQL
- **Programming:** Python for Data Engineering, Java/Scala (สำหรับ Spark)
- **Shell & scripting:** Bash → Linux & Command Line

## 7. Big Data & Platforms
- Hadoop / HDFS, Apache Spark, Hive → Distributed Systems Basics
- Cloud: AWS, GCP, Azure → Cloud Fundamentals
- BI tools: Power BI, Tableau, Looker

## 🧠 Self-check
<details>
<summary><b>1. JSON จัดเป็นข้อมูลประเภทไหน? เพราะอะไร?</b></summary>

**Semi-structured** เพราะมี key-value ที่บอกโครงสร้าง แต่แต่ละ record อาจมี field ไม่เหมือนกันและซ้อนกันได้ (nested) ไม่ได้บังคับ schema ตายตัวแบบตาราง

</details>

<details>
<summary><b>2. Data Warehouse กับ Data Lake ต่างกันอย่างไร?</b></summary>

**Warehouse:** เก็บข้อมูลที่ clean และมี schema แล้ว (schema-on-write) เหมาะกับ BI
**Lake:** เก็บข้อมูลดิบทุกรูปแบบ กำหนด schema ตอนอ่าน (schema-on-read) ราคาถูก เหมาะกับ data science และเก็บไว้ใช้ภายหลัง
ปัจจุบันมี **Lakehouse** ที่รวมทั้งสองแบบเข้าด้วยกัน

</details>

<details>
<summary><b>3. ทำไมไม่ควรให้ทีม BI query ตรงจาก database ของระบบ production?</b></summary>

เพราะ production DB ออกแบบมาสำหรับ OLTP (ธุรกรรมเล็ก ๆ ที่ต้องเร็ว) query วิเคราะห์ขนาดใหญ่จะไปแย่งทรัพยากรจนแอปช้าหรือล่มได้ จึงต้องย้ายข้อมูลไปไว้ที่ warehouse ก่อน → OLTP vs OLAP

</details>

<details>
<summary><b>4. Data Mart คืออะไร?</b></summary>

ส่วนย่อยของ data warehouse ที่ทำขึ้นสำหรับแผนกหรือเรื่องใดเรื่องหนึ่ง เช่น mart ของฝ่ายขาย ทำให้ query ง่ายขึ้นและควบคุมสิทธิ์การเข้าถึงได้

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
