# Python Data Structures

> [!NOTE]
> **สรุป Module 2 (จากหน้าสรุปของคอร์ส)**
> **Tuple** `( )`
> - collection ที่ **มีลำดับ (ordered)** และ **แก้ไขไม่ได้ (immutable)** ใช้จัดกลุ่มข้อมูลที่เกี่ยวข้องกัน
> - ใส่ได้หลายชนิดปนกัน เข้าถึงด้วย index บวก/ลบ ทำ concatenation และ slicing ได้
> - แก้ไขไม่ได้ ถ้าอยากเปลี่ยนต้องสร้าง tuple ใหม่
> - ซ้อนกันได้ (nesting) และเข้าถึงข้างในด้วย index หลายชั้น
>
> **List** `[ ]`
> - collection ที่ **มีลำดับ** และ **แก้ไขได้ (mutable)** ใส่ได้หลายชนิดปนกันและซ้อน list ได้
> - เข้าถึงด้วย index บวก/ลบ; เพิ่ม ลบ แก้ไข และ `split` string เป็น list ด้วย delimiter ได้
> - **Aliasing:** ตัวแปรหลายชื่อชี้ไปที่ object เดียวกัน / **Cloning:** สร้าง list ใหม่แยกออกมา
>
> **Dictionary** `{key: value}`
> - เก็บข้อมูลเป็นคู่ key-value ดึงค่าด้วย key
> - **key** ต้อง immutable และไม่ซ้ำ / **value** เป็นอะไรก็ได้และซ้ำได้
> - เพิ่ม ลบ ตรวจว่ามี key ไหม (`in` → `True`/`False`) และดึง `keys()` / `values()` ได้
>
> **Set** `{ }`
> - collection ของค่าที่ **ไม่ซ้ำ** และ **ไม่มีลำดับ**
> - `set(list)` = ตัดค่าซ้ำ; `add` / `remove` / `in`
> - `&` = intersection (ค่าที่มีร่วมกัน), `union()` = รวมทั้งหมด, `issubset()` = เช็กว่าเป็นเซตย่อยไหม

> [!WARNING]
> - *"Lists possess mutability, rendering them akin to tuples"* → ที่ถูกคือ mutability ทำให้ list **ต่าง** จาก tuple (list แก้ได้ tuple แก้ไม่ได้)
> - *"Dictionaries consist of keys and values, both composed of string elements"* → ไม่จำเป็นต้องเป็น string — key เป็นอะไรก็ได้ที่ immutable (`int`, `str`, `tuple`) ส่วน value เป็นอะไรก็ได้
> - *"Concatenating or appending a list will result in the modification of the same list"* → `append()`, `extend()`, `+=` แก้ list เดิม แต่ `a + b` **สร้าง list ใหม่**
> - *"combine sets using &"* → `&` ไม่ใช่การรวม แต่คือ **intersection** (เอาเฉพาะที่ซ้ำกัน) การรวมคือ `|` หรือ `union()`
> - *"color highlighting"* → เป็นแค่สีที่ editor แสดง ไม่ใช่ความสามารถของ Python

## ภาพรวม: เลือกใช้อะไรเมื่อไหร่
| | Tuple | List | Dictionary | Set |
|---|---|---|---|---|
| สัญลักษณ์ | `(1, 2)` | `[1, 2]` | `{"a": 1}` | `{1, 2}` |
| มีลำดับ | ✅ | ✅ | ✅ (ตามลำดับที่ใส่ ตั้งแต่ Python 3.7) | ❌ |
| แก้ไขได้ | ❌ | ✅ | ✅ | ✅ |
| ค่าซ้ำ | ได้ | ได้ | key ห้ามซ้ำ | ❌ |
| เข้าถึงด้วย | index | index | key | ทำไม่ได้ (ใช้ `in`) |
| ใช้ในงาน DE | แถวข้อมูลที่ไม่ควรเปลี่ยน, ผลจาก DB cursor | รวบรวม record, batch | 1 record (JSON), config, lookup | ตัดค่าซ้ำ, เทียบ 2 ชุดข้อมูล |

## 1. Tuple
```python
row = ("ORD001", "Bangkok", 1500.5)
row[0]          # 'ORD001'
row[-1]         # 1500.5
row[0:2]        # ('ORD001', 'Bangkok')
row + ("paid",) # ('ORD001', 'Bangkok', 1500.5, 'paid')  ← สร้าง tuple ใหม่
row[0] = "X"    # ❌ TypeError: tuple แก้ไม่ได้

order_id, city, amount = row          # unpacking ← ใช้บ่อยมาก
nested = (1, ("a", "b"), 3)
nested[1][0]    # 'a'
single = ("x",) # tuple 1 ตัวต้องมี comma! ("x") เป็นแค่ string
```

## 2. List
```python
cities = ["Bangkok", "Chiang Mai"]
cities.append("Phuket")          # เพิ่มท้าย 1 ตัว → แก้ list เดิม
cities.extend(["Krabi", "Nan"])  # เพิ่มหลายตัว → แก้ list เดิม
cities[0] = "BKK"                # แก้ค่า
del cities[1]                    # ลบตาม index
cities.remove("Nan")             # ลบตามค่า
cities.pop()                     # ดึงตัวท้ายออก
cities.sort()                    # เรียง (แก้ list เดิม)
len(cities)

"a,b,c".split(",")               # ['a', 'b', 'c'] ← string → list ด้วย delimiter
```

### Aliasing vs Cloning ⭐ (ต่อจากเรื่องที่ค้างไว้ใน [Python Basics](01-python-basics.md))
```python
a = [1, 2, 3]
b = a            # aliasing: a กับ b ชี้ list เดียวกัน
b.append(4)
print(a)         # [1, 2, 3, 4]  ← a เปลี่ยนตาม!

c = a[:]         # cloning (หรือ a.copy() / list(a))
c.append(5)
print(a)         # [1, 2, 3, 4]  ← a ไม่เปลี่ยน
```
> ในงาน DE ถ้าส่ง list เข้า function แล้ว function แก้ list นั้น ข้อมูลต้นฉบับจะเปลี่ยนไปด้วย — bug ที่หายากมาก

## 3. Dictionary
```python
order = {"order_id": "ORD001", "city": "Bangkok", "amount": 1500.5}
order["city"]                 # 'Bangkok'
order.get("discount", 0)      # 0  ← ไม่มี key ก็ไม่ error (ใช้บ่อยมากกับ JSON จาก API)
order["status"] = "paid"      # เพิ่ม / แก้
del order["amount"]           # ลบ
"city" in order               # True  (เช็กที่ key ไม่ใช่ value)
order.keys()                  # dict_keys(['order_id', 'city', 'status'])
order.values()
order.items()                 # คู่ (key, value) ← ใช้กับ for loop

for key, value in order.items():
    print(key, value)
```
> JSON ที่ได้จาก API แปลงเป็น Python ได้เป็น **dict + list ซ้อนกัน** — นี่คือโครงสร้างที่ DE เจอทุกวัน

## 4. Set
```python
ids = [1, 2, 2, 3, 3, 3]
unique = set(ids)            # {1, 2, 3}  ← ตัดค่าซ้ำ
unique.add(4)
unique.remove(1)
2 in unique                  # True  (เร็วกว่าการหาใน list มาก)

yesterday = {"A", "B", "C"}
today     = {"B", "C", "D"}
today & yesterday            # {'B', 'C'}  intersection: มีทั้ง 2 วัน
today | yesterday            # {'A','B','C','D'}  union: รวมทั้งหมด
today - yesterday            # {'D'}  difference: ใหม่วันนี้
yesterday - today            # {'A'}  หายไปวันนี้
{"B"}.issubset(today)        # True
empty = set()                # ⚠️ {} คือ dict ว่าง ไม่ใช่ set
```

## 🏋️ แบบฝึกหัด (สไตล์งาน DE)
- [ ] มี `customer_ids = [101, 102, 101, 103, 102, 104]` หาว่ามีลูกค้าไม่ซ้ำกี่คน
- [ ] มีข้อมูล 2 ชุด `source_ids = {1, 2, 3, 4, 5}` และ `target_ids = {1, 2, 3, 5}` หาว่า id ไหนโหลดไม่เข้า target
- [ ] แปลง `line = "ORD002,Chiang Mai,980.0"` เป็น dict `{"order_id": ..., "city": ..., "amount": ...}` โดย amount เป็น `float`
- [ ] จาก `api_response = {"data": {"user": {"name": "Dome", "skills": ["SQL", "Python"]}}}` ดึง skill ตัวที่ 2 ออกมา

<details>
<summary><b>เฉลย</b></summary>

```python
# 1
len(set(customer_ids))                      # 4

# 2
source_ids - target_ids                     # {4}

# 3
order_id, city, amount = line.split(",")
record = {"order_id": order_id, "city": city, "amount": float(amount)}

# 4
api_response["data"]["user"]["skills"][1]   # 'Python'
```

</details>

## 🧠 Self-check
<details>
<summary><b>1. Tuple กับ List ต่างกันอย่างไร? เมื่อไหร่ควรใช้ tuple?</b></summary>

List แก้ไขได้ tuple แก้ไม่ได้ ใช้ tuple กับข้อมูลที่ไม่ควรเปลี่ยน เช่น พิกัด (lat, lon) หรือแถวข้อมูลที่อ่านมาจาก database และ tuple ใช้เป็น key ของ dict ได้ ส่วน list ใช้ไม่ได้

</details>

<details>
<summary><b>2. `b = a` กับ `b = a[:]` ต่างกันอย่างไร?</b></summary>

`b = a` คือ aliasing (ชี้ที่ list เดียวกัน แก้ตัวหนึ่ง อีกตัวเปลี่ยนตาม) ส่วน `b = a[:]` คือ cloning (สร้าง list ใหม่แยกกัน)

</details>

<details>
<summary><b>3. ใช้ `list` เป็น key ของ dict ได้ไหม? เพราะอะไร?</b></summary>

ไม่ได้ เพราะ key ต้อง immutable (hashable) list แก้ไขได้ ถ้าต้องการ key หลายค่า ให้ใช้ tuple แทน เช่น `{("BKK", 2026): 100}`

</details>

<details>
<summary><b>4. `d["x"]` กับ `d.get("x")` ต่างกันอย่างไรเมื่อไม่มี key `"x"`?</b></summary>

`d["x"]` จะเกิด `KeyError` ส่วน `d.get("x")` คืนค่า `None` (หรือค่า default ที่กำหนด) ไม่ error

</details>

<details>
<summary><b>5. `{}` สร้างอะไร?</b></summary>

**dict ว่าง** ไม่ใช่ set ถ้าต้องการ set ว่างต้องใช้ `set()`

</details>

<details>
<summary><b>6. `&`, `|`, `-` ของ set ทำอะไร?</b></summary>

`&` = intersection (ค่าที่มีร่วมกัน), `|` = union (รวมทั้งหมด), `-` = difference (มีในตัวแรกแต่ไม่มีในตัวหลัง)

</details>

## Notes (เขียนสรุปด้วยคำพูดตัวเอง)
