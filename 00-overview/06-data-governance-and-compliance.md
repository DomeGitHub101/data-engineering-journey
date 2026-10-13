# Data Governance & Compliance

## กฎหมายและมาตรฐานสำคัญ
| กฎ | ขอบเขต | อุตสาหกรรม |
|---|---|---|
| **GDPR** | คุ้มครองข้อมูลส่วนบุคคลของคนในสหภาพยุโรป (EU) | ทุกอุตสาหกรรม |
| **HIPAA** | ความปลอดภัยของข้อมูลสุขภาพ (สหรัฐฯ) | Healthcare |
| **PCI DSS** | มาตรฐานความปลอดภัยข้อมูลบัตรชำระเงิน | Retail / Payment |
| **SOX** | ควบคุมความถูกต้องของรายงานทางการเงิน (สหรัฐฯ) | Finance |
| **PDPA** 🇹🇭 | พ.ร.บ.คุ้มครองข้อมูลส่วนบุคคลของไทย หลักการคล้าย GDPR | ทุกอุตสาหกรรม (ไม่มีในคอร์ส แต่ใช้จริงในไทย) |

## Data Lifecycle ในมุมของ Governance
1. **Data Acquisition:** กำหนดว่าต้องเก็บข้อมูลอะไร ปริมาณเท่าไหร่ และใช้เพื่ออะไร (**purpose limitation** — เก็บเท่าที่จำเป็นต่อวัตถุประสงค์)
2. **Data Processing:** ทำความสะอาดและแปลงข้อมูล
3. **Data Storage:** เก็บอย่างปลอดภัย (**data at rest** → เข้ารหัส, จำกัดสิทธิ์)
4. **Data Sharing:** กำหนดสิทธิ์ เงื่อนไข และข้อตกลงเมื่อให้ **third-party vendors** เข้าถึงข้อมูล
5. **Data Retention:** กำหนดว่าเก็บข้อมูลไว้นานเท่าไหร่ตามกฎหมาย
6. **Data Disposal:** ลบหรือทำลายข้อมูลถาวรแบบกู้คืนไม่ได้ (**data erasure**)

> [!NOTE]
> **เทียบกับ [Data Engineering Lifecycle](03-data-engineering-lifecycle.md)**
> Lifecycle ฝั่ง engineering (Generation → Ingestion → Storage → Transformation → Serving) เน้นว่า **ข้อมูลไหลอย่างไร** ส่วนฝั่ง governance เน้นว่า **ต้องควบคุมอะไรในแต่ละช่วง** จนถึงการลบทิ้ง

## Security & Privacy
- **Masking / Anonymization:** ซ่อนข้อมูลอ่อนไหวไม่ให้ผู้ใช้เห็น เช่น `081-XXX-1234`

> [!WARNING]
> คอร์สอธิบายว่า masking / anonymization "ซ่อนข้อมูลใน presentation layer โดยไม่เปลี่ยนข้อมูลจริงใน database" ซึ่ง **ถูกเฉพาะ dynamic masking** ในความจริงมีหลายแบบ:
> - **Dynamic masking:** ซ่อนตอนแสดงผลตามสิทธิ์ของผู้ใช้ ข้อมูลจริงไม่เปลี่ยน ✅ (ตรงกับที่คอร์สสอน)
> - **Static masking:** สร้างสำเนาข้อมูลที่ถูก mask ถาวร เช่น สำหรับ dev / test
> - **Anonymization (ตามนิยามของ GDPR):** เปลี่ยนข้อมูลแบบ **ย้อนกลับไม่ได้** จนระบุตัวบุคคลไม่ได้อีก
> - **Pseudonymization:** แทนที่ด้วยรหัส (เช่น hash) ซึ่งยังย้อนกลับได้ถ้ามี key
>
> ถ้าทำข้อสอบของคอร์ส ให้ตอบตามคอร์ส แต่ในงานจริงต้องแยกให้ออกว่าแบบไหนเป็นแบบไหน

## 🧠 Self-check
<details>
<summary><b>1. บริษัทเก็บข้อมูลบัตรเครดิตของลูกค้า ต้องปฏิบัติตามมาตรฐานไหน?</b></summary>

**PCI DSS** (ถ้าลูกค้าอยู่ใน EU ต้องดู GDPR ด้วย และในไทยต้องดู PDPA)

</details>

<details>
<summary><b>2. Purpose limitation คืออะไร? อยู่ในช่วงไหนของ lifecycle?</b></summary>

หลักการเก็บข้อมูลเฉพาะที่จำเป็นต่อวัตถุประสงค์ที่แจ้งไว้ อยู่ในช่วง **Data Acquisition** เช่น แอปสั่งอาหารไม่ควรเก็บข้อมูลศาสนาของลูกค้า

</details>

<details>
<summary><b>3. ช่วงไหนของ lifecycle ที่ต้องทำสัญญากับ vendor ภายนอก?</b></summary>

**Data Sharing**

</details>

<details>
<summary><b>4. ลบแถวด้วย `DELETE` ถือเป็น data disposal ที่สมบูรณ์แล้วหรือยัง?</b></summary>

ยังไม่แน่ เพราะข้อมูลอาจยังอยู่ใน backup, log, replica หรือสำเนาที่ส่งต่อไปที่อื่น disposal ที่ถูกต้องต้องตามลบให้ครบทุกที่และกู้คืนไม่ได้

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
