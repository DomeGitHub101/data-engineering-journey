# Python Programming Fundamentals

> [!NOTE]
> **สรุป Module 3 (จากหน้าสรุปของคอร์ส)**
> **Conditions & Branching**
> - `if` รันโค้ดตามเงื่อนไข True / False ที่ได้จาก comparison และ Boolean expression
> - Comparison operators: `==`, `!=`, `>`, `<`, `>=`, `<=` ใช้เทียบได้ทั้ง int, float, string
> - `if` → `elif` (เช็กต่อเมื่อเงื่อนไขก่อนหน้าเป็น False) → `else` (ทุกเงื่อนไขก่อนหน้าเป็น False, ไม่มีเงื่อนไข)
> - Boolean logic operators: `and`, `or`, `not`
>
> **Loops**
> - ทำงานซ้ำอัตโนมัติ และวนผ่าน list / dict ได้
> - `range(start, stop, step)` สร้างลำดับตัวเลข
> - `for` วนทีละตัวใน sequence (list, tuple, string) / `while` วนตราบใดที่เงื่อนไขยังเป็น True
>
> **Functions**
> - โค้ดที่นำกลับมาใช้ซ้ำได้ รับ parameter และมักคืนค่าผลลัพธ์
> - Built-in functions: `len()`, `sum()`, `sorted()` (คืน list ใหม่) vs `.sort()` (เรียง list เดิม)
> - สร้างเองได้ด้วย `def`; เขียน docstring ใน `""" """` และดูได้ด้วย `help()`
> - มีหลาย parameter ได้; ไม่มี `return` = คืน `None`; `pass` = placeholder
>
> **Scope**
> - Local: ประกาศในฟังก์ชัน ใช้ได้เฉพาะในฟังก์ชันนั้น / Global: ประกาศระดับบนสุด เข้าถึงได้ทุกที่
>
> **Exception Handling**
> - จัดการ error ไม่ให้โปรแกรมพัง: `try` → `except` (เมื่อ error) → `else` (เมื่อไม่ error) → `finally` (รันเสมอ)
>
> **Objects & Classes**
> - Object = instance ของ class ที่รวม data (attributes) กับ behavior (methods); ดูชนิดด้วย `type()`
> - Class = พิมพ์เขียวสำหรับสร้าง object; `__init__` ใช้กำหนดค่าเริ่มต้นของ attributes
> - Method = ฟังก์ชันใน class ที่ใช้/แก้ attributes ต้องมี `self` เป็น parameter แรก

> [!WARNING]
> - *"A function will usually perform more than one task"* → หลักที่ดีคือ **1 ฟังก์ชันทำ 1 งาน** (single responsibility) ทดสอบง่ายและนำกลับมาใช้ได้ง่ายกว่า
> - *"global variable ... any part of the code can access or modify"* → **อ่าน**ได้ทุกที่ แต่ถ้าจะ**แก้ค่า**ในฟังก์ชันต้องประกาศ `global` ก่อน (และในงานจริงควรหลีกเลี่ยง)
> - *"object's type usually remains the same"* → method ไม่เปลี่ยน type ของ object **เลย** ไม่ใช่แค่ "usually"
> - *"init"* → ชื่อเต็มคือ `__init__` (มี underscore 2 ตัวหน้าและหลัง)

## 1. Conditions & Branching
```python
amount = 1500
status = "paid"

if amount <= 0:
    print("invalid amount")
elif amount > 10000 and status == "paid":
    print("high-value order")
elif status != "paid":
    print("not paid yet")
else:
    print("normal order")
```
| Operator | ความหมาย | ตัวอย่าง |
|---|---|---|
| `and` | จริงทั้งคู่ | `amount > 0 and status == "paid"` |
| `or` | จริงอย่างน้อย 1 | `city == "BKK" or city == "Bangkok"` |
| `not` | กลับค่า | `not is_deleted` |
| `in` | อยู่ใน collection | `city in {"BKK", "CNX"}` |

> ⚠️ `=` คือกำหนดค่า, `==` คือเปรียบเทียบ — สับสนกันบ่อยมาก
> ⚠️ string เทียบแบบตัวพิมพ์เล็กใหญ่ต่างกัน: `"Bangkok" == "bangkok"` เป็น `False` → มัก `.lower()` ก่อนเทียบ

## 2. Loops
```python
list(range(5))          # [0, 1, 2, 3, 4]
list(range(2, 10, 3))   # [2, 5, 8]

files = ["orders.csv", "users.csv", "temp.tmp"]
for f in files:
    if not f.endswith(".csv"):
        continue        # ข้ามไปตัวถัดไป
    print(f"loading {f}")

for i, f in enumerate(files):        # ได้ทั้ง index และค่า
    print(i, f)

order = {"id": "ORD001", "city": "Bangkok"}
for key, value in order.items():     # วน dict
    print(key, value)

retries = 0
while retries < 3:                   # วนจนกว่าเงื่อนไขเป็น False
    retries += 1
    print(f"attempt {retries}")
```
- `break` = ออกจาก loop ทันที / `continue` = ข้ามรอบนี้ไปรอบถัดไป
- ⚠️ `while` ที่เงื่อนไขไม่มีวันเป็น False = **infinite loop** ต้องมีตัวนับหรือเงื่อนไขหยุดเสมอ

## 3. Functions
```python
def clean_city(city: str, default: str = "Unknown") -> str:
    """ตัดช่องว่างและจัดรูปแบบชื่อเมือง ถ้าว่างคืนค่า default"""
    city = city.strip().title()
    return city if city else default

clean_city("  bangkok ")    # 'Bangkok'
clean_city("")              # 'Unknown'
help(clean_city)            # แสดง docstring

def log(msg):
    print(msg)              # ไม่มี return → คืน None

def load_to_warehouse():
    pass                    # placeholder ทำทีหลัง
```

### `sorted()` vs `.sort()`
```python
nums = [3, 1, 2]
new = sorted(nums)   # new = [1, 2, 3], nums ยังเป็น [3, 1, 2]
nums.sort()          # nums = [1, 2, 3], คืนค่า None!
x = nums.sort()      # ❌ x เป็น None — bug ที่เจอบ่อย
```

### Scope
```python
row_count = 0                 # global

def count_rows(rows):
    total = len(rows)         # local — ใช้ได้แค่ในฟังก์ชันนี้
    return total

def bad_increment():
    global row_count          # ต้องประกาศถ้าจะแก้ global
    row_count += 1
```
> ในงาน DE ให้ส่งค่าเข้าทาง parameter และคืนค่าด้วย `return` แทนการแก้ global — pipeline จะทดสอบง่ายและไม่เกิด bug แปลก ๆ

## 4. Exception Handling ⭐ (ใช้ทุกวันในงาน DE)
```python
def parse_amount(value):
    try:
        amount = float(value)
    except ValueError:
        print(f"[WARN] invalid amount: {value!r}")
        return None
    else:
        return amount          # รันเมื่อไม่มี error
    finally:
        pass                   # รันเสมอ เช่น ปิดไฟล์ / ปิด connection

parse_amount("19.99")   # 19.99
parse_amount("abc")     # [WARN] invalid amount: 'abc' → None
```
- จับ **error ที่เจาะจง** (`ValueError`, `KeyError`, `FileNotFoundError`) ไม่ควรใช้ `except:` เปล่า ๆ เพราะจะกลืน error ทุกชนิดจนหาบั๊กไม่เจอ
- `finally` เหมาะกับการคืนทรัพยากร เช่น `conn.close()`

### ตัวอย่างจริง: retry เมื่อเรียก API ล้มเหลว
```python
import time

def fetch_with_retry(fetch, max_retries=3):
    for attempt in range(1, max_retries + 1):
        try:
            return fetch()
        except ConnectionError as e:
            print(f"attempt {attempt} failed: {e}")
            time.sleep(2 ** attempt)     # รอนานขึ้นเรื่อย ๆ (exponential backoff)
    raise RuntimeError("all retries failed")
```

## 5. Objects & Classes
```python
class Order:
    """แทน 1 order"""

    def __init__(self, order_id, amount, status="pending"):
        self.order_id = order_id     # data attributes
        self.amount = amount
        self.status = status

    def mark_paid(self):             # method — แก้ attribute ของ object
        self.status = "paid"

    def is_high_value(self, threshold=10000):
        return self.amount > threshold

o = Order("ORD001", 15000)      # สร้าง instance
o.mark_paid()
o.status                        # 'paid'
o.is_high_value()               # True
type(o)                         # <class '__main__.Order'>
```
> ในงาน DE จะเจอ class บ่อยตอนใช้ library เช่น `pd.DataFrame`, Airflow `DAG` / Operator และใช้ `dataclass` หรือ `pydantic` สร้าง model ของ record

## 🏋️ แบบฝึกหัด (สไตล์งาน DE)
- [ ] เขียนฟังก์ชัน `validate_record(record: dict) -> list` ที่คืน list ของ error เช่น ไม่มี `order_id`, `amount` ไม่ใช่ตัวเลข หรือ `amount` ติดลบ
- [ ] มี `rows = ["10", "20", "abc", "30", ""]` ให้รวมยอดเฉพาะค่าที่แปลงเป็นตัวเลขได้ และนับว่ามีกี่แถวที่เสีย (ใช้ `try/except`)
- [ ] เขียน class `Pipeline` ที่มี attribute `name` และ `rows_loaded` และ method `load(rows)` ที่บวกจำนวนแถวเข้าไปแล้วพิมพ์ log

<details>
<summary><b>เฉลยข้อ 1</b></summary>

```python
def validate_record(record: dict) -> list:
errors = []
if not record.get("order_id"):
errors.append("missing order_id")
try:
amount = float(record.get("amount"))
if amount < 0:
errors.append("negative amount")
except (TypeError, ValueError):
errors.append("invalid amount")
return errors

validate_record({"order_id": "A1", "amount": "-5"})   # ['negative amount']
validate_record({"amount": "x"})                       # ['missing order_id', 'invalid amount']
```

</details>

<details>
<summary><b>เฉลยข้อ 2</b></summary>

```python
total, bad = 0, 0
for r in rows:
try:
total += float(r)
except ValueError:
bad += 1
print(total, bad)   # 60.0 2
```

</details>

<details>
<summary><b>เฉลยข้อ 3</b></summary>

```python
class Pipeline:
def __init__(self, name):
self.name = name
self.rows_loaded = 0

def load(self, rows):
self.rows_loaded += len(rows)
print(f"[{self.name}] loaded {len(rows)} rows (total {self.rows_loaded})")

p = Pipeline("orders")
p.load([1, 2, 3])   # [orders] loaded 3 rows (total 3)
```

</details>

## 🧠 Self-check
<details>
<summary><b>1. `elif` ต่างจากการเขียน `if` หลายอันต่อกันอย่างไร?</b></summary>

`if / elif` จะเข้าแค่ **บล็อกแรก** ที่เงื่อนไขเป็น True แล้วข้ามที่เหลือ ส่วน `if` หลายอันต่อกันจะเช็ก **ทุกอัน** และอาจเข้าหลายบล็อก

</details>

<details>
<summary><b>2. `list(range(1, 10, 2))` ได้อะไร?</b></summary>

`[1, 3, 5, 7, 9]` (เริ่ม 1, หยุดก่อน 10, เพิ่มทีละ 2)

</details>

<details>
<summary><b>3. `x = [3, 1, 2].sort()` แล้ว `x` เป็นอะไร?</b></summary>

`None` เพราะ `.sort()` เรียง list เดิมและคืนค่า `None` ถ้าต้องการ list ใหม่ให้ใช้ `sorted()`

</details>

<details>
<summary><b>4. ฟังก์ชันที่ไม่มี `return` คืนค่าอะไร?</b></summary>

`None`

</details>

<details>
<summary><b>5. `else` กับ `finally` ใน try-except ต่างกันอย่างไร?</b></summary>

`else` รัน **เฉพาะเมื่อไม่มี error** ส่วน `finally` รัน **เสมอ** ไม่ว่าจะมี error หรือไม่

</details>

<details>
<summary><b>6. ทำไมไม่ควรเขียน `except:` เปล่า ๆ?</b></summary>

เพราะจะจับ error ทุกชนิด (รวมถึงตอนกด Ctrl+C) ทำให้บั๊กจริงถูกซ่อนไว้ ควรระบุชนิด error ที่คาดไว้ เช่น `except ValueError:`

</details>

<details>
<summary><b>7. `self` ใน method คืออะไร?</b></summary>

ตัวแทนของ object ที่เรียก method นั้น ใช้เข้าถึง attribute ของ object ตัวเอง เช่น `self.amount`

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
