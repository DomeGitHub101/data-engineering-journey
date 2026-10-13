# What is Data Engineering

## สรุป
**Data Engineering** คือการออกแบบ สร้าง และดูแลระบบที่ **เก็บรวบรวม จัดเก็บ แปลง และส่งมอบข้อมูล** ให้คนหรือระบบปลายทางนำไปใช้ได้ **อย่างถูกต้อง ตรงเวลา ปลอดภัย และคุ้มค่า**

> เปรียบเทียบง่าย ๆ: Data Engineer คือคนวางระบบท่อประปา ส่วน Data Analyst / Data Scientist คือคนที่เปิดก๊อกใช้น้ำ ถ้าท่อรั่วหรือน้ำสกปรก ปลายทางจะใช้งานไม่ได้เลย

## บทบาทในทีม Data
| Role | โฟกัส | คำถามที่ตอบ |
|---|---|---|
| **Data Engineer** | สร้าง pipeline และ infrastructure ให้ข้อมูลพร้อมใช้ | "ข้อมูลไหลมาถูกต้องและครบไหม?" |
| **Data Analyst** | วิเคราะห์ข้อมูล ทำ report และ dashboard | "เกิดอะไรขึ้น? ทำไม?" |
| **Data Scientist** | สร้าง model ทางสถิติและ ML | "จะเกิดอะไรขึ้นต่อไป?" |
| **Analytics Engineer** | แปลงข้อมูลใน warehouse ให้เป็น model ที่ธุรกิจใช้ (ใช้ dbt เป็นหลัก) | "นิยามตัวเลขนี้ถูกต้องและตรงกันทุกที่ไหม?" |
| **ML Engineer** | นำ model ขึ้น production | "model รันได้จริงและเสถียรไหม?" |

## งานหลักของ Data Engineer
- ดึงข้อมูลจากหลายแหล่ง (database, API, ไฟล์, event stream) → ETL vs ELT
- ออกแบบที่เก็บข้อมูล → Data Modeling, Data Warehouse Concepts, Data Lake & Lakehouse
- แปลงและทำความสะอาดข้อมูล → dbt, Apache Spark
- ตั้งเวลาและดูแล pipeline → Apache Airflow
- รักษาคุณภาพและความปลอดภัยของข้อมูล → Data Quality & Testing, Data Governance & Security

## ทักษะที่ต้องมี
- **Technical:** SQL, Python for Data Engineering, Linux & Command Line, database, cloud, big data tools
- **Functional:** แปลง requirement ธุรกิจเป็นงานด้านเทคนิค, เข้าใจ data lifecycle
- **Soft skills:** สื่อสารกับ stakeholder, ทำงานเป็นทีม, แก้ปัญหา

## 🧠 Self-check (ลองตอบเองก่อนเปิดดูเฉลย)
<details>
<summary><b>1. Data Engineer ต่างจาก Data Scientist อย่างไร?</b></summary>

DE สร้างและดูแล **ระบบ** ที่ทำให้ข้อมูลพร้อมใช้ (pipeline, storage, quality) ส่วน DS ใช้ข้อมูลนั้นสร้าง **model / insight** ถ้าไม่มีงานของ DE ฝั่ง DS จะต้องใช้เวลาส่วนใหญ่ไปกับการหาและทำความสะอาดข้อมูล

</details>

<details>
<summary><b>2. ทำไมบอกว่า "ข้อมูลที่ดี" ต้องมีมากกว่าแค่ถูกต้อง?</b></summary>

ต้อง **ครบ** (completeness), **ทันเวลา** (timeliness), **สม่ำเสมอ** (consistency), **ปลอดภัย** และ **เข้าถึงได้** ด้วย ข้อมูลถูกต้องแต่มาช้าไป 2 วันก็อาจไร้ประโยชน์ → ดู Data Quality & Testing

</details>

<details>
<summary><b>3. ยกตัวอย่างงานที่ DE ทำในบริษัท e-commerce 1 อย่าง</b></summary>

เช่น ดึงข้อมูล order จาก database ของเว็บทุกชั่วโมง → ทำความสะอาด → โหลดเข้า warehouse เป็น Star Schema ให้ทีม BI ทำ dashboard ยอดขายได้

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
