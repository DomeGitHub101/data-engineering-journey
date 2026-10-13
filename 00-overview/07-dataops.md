# DataOps

## DataOps คืออะไร
แนวทางทำงานร่วมกันด้านการจัดการข้อมูล ที่มุ่ง **ปรับปรุงการสื่อสาร การ integrate และการทำ automation** ของ data flow ระหว่างผู้ดูแลข้อมูลกับผู้ใช้ข้อมูลในองค์กร

> แนวคิดคือนำหลัก **DevOps** และ **Agile** ของฝั่ง software มาใช้กับงาน data: ส่งมอบเร็วขึ้น ทำซ้ำได้ ทดสอบอัตโนมัติ และ monitor ตลอดเวลา

## 3 เฟสหลักของ DataOps
> [!NOTE]
> 3 เฟสนี้ยังต้องทบทวนกับเนื้อหาในคอร์สอีกครั้ง

1. **Establish DataOps (วางรากฐาน):** วางระบบที่จำเป็น เช่น สร้าง data catalog, กำหนด policy และ governance, ตั้ง version control ของโค้ด และกำหนด metric ที่จะวัด
2. **Iterate DataOps (ทำซ้ำเป็นรอบ):** ส่งมอบงานเป็นรอบสั้น ๆ (sprint) ทุกรอบผ่าน automated testing (Data Quality & Testing) และ CI/CD (CI-CD for Data) แล้วจัดลำดับความสำคัญของงานรอบถัดไป
3. **Improve DataOps (ปรับปรุงต่อเนื่อง):** วัดผลจาก metric, หาคอขวด, ปรับปรุงกระบวนการและ automation อย่างต่อเนื่อง

## หลักการที่ใช้จริง
- ทุกอย่างอยู่ใน Git → Git & Version Control
- ทดสอบข้อมูลอัตโนมัติ → Data Quality & Testing
- Deploy ผ่าน CI/CD → CI-CD for Data
- Infrastructure เป็นโค้ด → Infrastructure as Code
- Monitor และแจ้งเตือน → [Performance Tuning & Monitoring](05-performance-tuning-and-monitoring.md)
- รู้ที่มาของข้อมูล → Data Lineage

## 🧠 Self-check
<details>
<summary><b>1. DataOps ต่างจาก DevOps อย่างไร?</b></summary>

DevOps เน้นการส่งมอบ **software** ส่วน DataOps นำหลักการเดียวกันมาใช้กับ **data pipeline และตัวข้อมูล** ซึ่งมีความท้าทายเพิ่ม เพราะต่อให้โค้ดไม่เปลี่ยน ข้อมูลก็เปลี่ยนได้ทุกวัน จึงต้องทดสอบตัวข้อมูลด้วย ไม่ใช่แค่ทดสอบโค้ด

</details>

<details>
<summary><b>2. ยกตัวอย่าง automation ในแนวทาง DataOps 1 อย่าง</b></summary>

เช่น ทุกครั้งที่เปิด Pull Request แก้ dbt model, GitHub Actions จะรัน `dbt build` บน environment ทดสอบ ถ้า test ไม่ผ่านจะ merge ไม่ได้

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
