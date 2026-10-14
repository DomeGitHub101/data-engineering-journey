# Python Basics

> [!NOTE]
> **สรุป Module 1 (จากหน้าสรุปของคอร์ส)**
> **Data Types**
> - Python แยกชนิดข้อมูลได้: `int`, `float`, `str`, `bool`
> - `int` = จำนวนเต็ม บวกหรือลบก็ได้ / `float` = ตัวเลขที่มีจุดทศนิยม แทนได้ทั้งค่าเต็มและเศษส่วน (เช่น `5.0`, `0.25`)
> - Typecasting: แปลง `int` ↔ `float` ได้, แปลงตัวเลขเป็น `str` ได้, แปลงตัวเลขเป็น `bool` ได้ (`0` → `False`, ค่าอื่นที่ไม่ใช่ 0 → `True`)
>
> **Expressions & Variables**
> - Expression = ค่า + operator ที่คำนวณออกมาเป็นผลลัพธ์เดียว
> - `//` = หารแบบปัดเศษทิ้ง (integer division)
> - คำนวณตามลำดับ **BODMAS**: วงเล็บ → ยกกำลัง → คูณ/หาร → บวก/ลบ
> - `=` คือ assignment operator; กำหนดค่าใหม่ให้ตัวแปรเดิม = เขียนทับค่าเก่า
> - แก้ค่าตัวแปรหนึ่งจะกระทบอีกตัวแปร **เฉพาะเมื่อทั้งคู่อ้างถึง mutable object เดียวกัน** (ดูข้อ 2)
>
> **Strings**
> - เขียนใน `" "` หรือ `' '` ใส่ได้ทั้งตัวอักษร ช่องว่าง ตัวเลข สัญลักษณ์ เป็น **ordered sequence** ของตัวอักษร
> - ทำ indexing (บวก/ลบ), slicing และ stride ได้
> - Concatenation (`+`) และ replication (`*`) ได้ string ใหม่; `len()` ได้ตัวเลข
> - String เป็น **immutable** แก้ไขของเดิมไม่ได้ — method ทุกตัว (`upper`, `replace`, `find`, ...) คืนค่า string ใหม่
> - Escape sequences: `\n` ขึ้นบรรทัดใหม่, `\t` tab, `\\` backslash

## 1. Data Types พื้นฐาน
| Type | ตัวอย่าง | ใช้กับข้อมูลแบบไหน |
|---|---|---|
| `int` | `42`, `-7` | จำนวนเต็ม เช่น จำนวน order |
| `float` | `3.14`, `1e6` | ทศนิยม เช่น ราคา |
| `str` | `"Bangkok"`, `'abc'` | ข้อความ |
| `bool` | `True`, `False` | ใช่ / ไม่ใช่ |

```python
type(42)        # <class 'int'>
type(3.14)      # <class 'float'>
type("hello")   # <class 'str'>
type(True)      # <class 'bool'>
```

### Type Casting (แปลงชนิดข้อมูล)
```python
int("123")      # 123      ← ข้อมูลจาก CSV มักเป็น str ต้องแปลงก่อนคำนวณ
float("19.99")  # 19.99
str(100)        # "100"
int(3.99)       # 3        ← ตัดทศนิยมทิ้ง ไม่ได้ปัดเศษ!
bool(0)         # False   (0, "", None → False)
bool("abc")     # True
int("12.5")     # ❌ ValueError
```

## 2. Expressions & Variables
```python
total = 100 + 50 * 2    # 200 (คูณก่อนบวก)
avg = 10 / 4            # 2.5   ← / ได้ float เสมอ
whole = 10 // 4         # 2     ← หารปัดลง
remainder = 10 % 4      # 2     ← เศษ
power = 2 ** 10         # 1024
```
- ตั้งชื่อตัวแปรแบบ `snake_case` และสื่อความหมาย เช่น `total_sales` ไม่ใช่ `x`
- ตัวแปรเปลี่ยนค่าได้: `count = count + 1` หรือ `count += 1`

### ลำดับการคำนวณ (BODMAS)
```python
2 + 3 * 4       # 14   ← คูณก่อน
(2 + 3) * 4     # 20   ← วงเล็บก่อน
2 ** 3 * 2      # 16   ← ยกกำลังก่อนคูณ
```
> เขียนวงเล็บให้ชัดไว้ก่อนเสมอ ทั้งอ่านง่ายและกันพลาด

### ตัวแปรสองตัวอ้างถึงของชิ้นเดียวกัน (Reference)
```python
# immutable (int, float, str): เปลี่ยน a แล้ว b ไม่เปลี่ยน
a = 10
b = a
a = 20
print(b)        # 10

# mutable (เช่น list ที่จะเรียนใน Module 2): แก้ผ่านตัวหนึ่ง อีกตัวเปลี่ยนตาม!
x = [1, 2, 3]
y = x           # y ชี้ไปที่ list เดียวกับ x
x.append(4)
print(y)        # [1, 2, 3, 4]
y = x.copy()    # ถ้าอยากได้สำเนาแยก ต้อง copy
```

## 3. String Operations
### Indexing & Slicing
```python
city = "Bangkok"
city[0]      # 'B'
city[-1]     # 'k'      ← index ติดลบนับจากท้าย
city[0:4]    # 'Bang'   ← [start:end] ไม่รวม end
city[::2]    # 'Bnkk'   ← stride ทีละ 2
city[::-1]   # 'kokgnaB' ← กลับด้าน
len(city)    # 7
```

### Concatenation & Replication
```python
"Data" + " " + "Engineer"   # 'Data Engineer'
"-" * 20                    # '--------------------'  ← ใช้ทำเส้นคั่นใน log
"Rows: " + 100              # ❌ TypeError ต้องเป็น "Rows: " + str(100) หรือใช้ f-string
```

### Methods ที่ใช้บ่อย
```python
s = "  Hello, Data Engineer  "
s.upper()                 # '  HELLO, DATA ENGINEER  '
s.lower()
s.strip()                 # 'Hello, Data Engineer'   ← ตัดช่องว่างหน้า-หลัง (ใช้บ่อยมากตอน clean data)
s.replace("Hello", "Hi")
s.find("Data")            # 9  (ไม่เจอ = -1)
"a,b,c".split(",")        # ['a', 'b', 'c']   ← แยก CSV
"-".join(["2026", "10", "14"])  # '2026-10-14'
```
> String เป็น **immutable** — method ทุกตัวคืนค่า string ใหม่ ต้องเอาไปเก็บใส่ตัวแปร เช่น `s = s.strip()`

### Escape Sequences & f-strings
```python
print("line1\nline2")     # \n ขึ้นบรรทัดใหม่
print("col1\tcol2")       # \t tab
print("C:\\data\\raw")    # \\ = backslash 1 ตัว → C:\data\raw
print(r"C:\new\data")     # r"" = raw string ไม่แปล \

name, rows = "orders", 1500
print(f"Loaded {rows:,} rows into {name}")   # Loaded 1,500 rows into orders
```

## 🏋️ แบบฝึกหัด (สไตล์งาน DE)
- [ ] มี `raw = "  2026-10-14,ORD001,  Bangkok ,1500.50  "` ให้ `strip` แล้ว `split(",")` แยกเป็นวันที่, order id, เมือง (ตัดช่องว่างด้วย) และยอดเงิน (แปลงเป็น `float`)
- [ ] จาก `filename = "sales_2026_10_14.csv"` ให้ดึงปี เดือน วัน ออกมาด้วย slicing หรือ `split`
- [ ] พิมพ์ log `"[INFO] Loaded 1,500 rows from orders.csv"` ด้วย f-string

<details>
<summary><b>เฉลยข้อ 1</b></summary>

```python
raw = "  2026-10-14,ORD001,  Bangkok ,1500.50  "
date, order_id, city, amount = raw.strip().split(",")
city = city.strip()
amount = float(amount)
print(date, order_id, city, amount)  # 2026-10-14 ORD001 Bangkok 1500.5
```

</details>

<details>
<summary><b>เฉลยข้อ 2</b></summary>

```python
filename = "sales_2026_10_14.csv"
_, year, month, day = filename.replace(".csv", "").split("_")
# หรือ slicing: filename[6:10], filename[11:13], filename[14:16]
```

</details>

## 🧠 Self-check
<details>
<summary><b>1. `int("3.7")` ได้ผลอะไร?</b></summary>

**Error** (ValueError) เพราะ `int()` แปลง string ที่มีจุดทศนิยมไม่ได้ ต้องทำ `int(float("3.7"))` ซึ่งได้ `3`

</details>

<details>
<summary><b>2. `10 / 2` กับ `10 // 2` ต่างกันอย่างไร?</b></summary>

`10 / 2` ได้ `5.0` (float เสมอ) ส่วน `10 // 2` ได้ `5` (int, หารปัดลง)

</details>

<details>
<summary><b>3. `"Python"[1:4]` ได้อะไร?</b></summary>

`'yth'` (index 1, 2, 3 ไม่รวม 4)

</details>

<details>
<summary><b>4. ทำไม `s.upper()` แล้ว `print(s)` ยังเป็นตัวเล็กอยู่?</b></summary>

เพราะ string เป็น immutable `upper()` คืน string ใหม่ แต่ไม่ได้แก้ `s` ต้องเขียน `s = s.upper()`

</details>

<details>
<summary><b>5. `2 + 3 * 2 ** 2` ได้เท่าไหร่?</b></summary>

`14` เพราะยกกำลังก่อน (`2 ** 2 = 4`) → คูณ (`3 * 4 = 12`) → บวก (`2 + 12`)

</details>

<details>
<summary><b>6. `a = "hi"`, `b = a`, `a = a.upper()` แล้ว `b` เป็นอะไร?</b></summary>

ยังเป็น `'hi'` เพราะ string เป็น immutable `a.upper()` สร้าง string ใหม่แล้วให้ `a` ชี้ไปที่ของใหม่ ส่วน `b` ยังชี้ของเดิม

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
