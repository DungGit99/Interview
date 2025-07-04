# 🧠 Query trong MongoDB" là gì?

## 📘 Các kiểu query phổ biến:
1. 🎯 Tìm theo điều kiện chính xác
```js
	db.users.find({ name: "Bối Bối" })
```
2. 🔢 So sánh giá trị
-  Toán tử logic
| Toán tử | Ý nghĩa                   | Ví dụ                              |
|---------|--------------------------|-----------------------------------|
| `$eq`   | Bằng                     | `db.users.find({ age: { $eq: 25 } })` |
| `$ne`   | Không bằng               | `db.users.find({ age: { $ne: 25 } })` |
| `$gt`   | Lớn hơn                  | `db.users.find({ age: { $gt: 20 } })` |
| `$gte`  | Lớn hơn hoặc bằng        | `db.users.find({ age: { $gte: 18 } })` |
| `$lt`   | Nhỏ hơn                  | `db.users.find({ age: { $lt: 30 } })` |
| `$lte`  | Nhỏ hơn hoặc bằng        | `db.users.find({ age: { $lte: 30 } })` |

-  Toán tử logic
| Toán tử | Ý nghĩa                   | Ví dụ                                                                 |
|---------|--------------------------|-----------------------------------------------------------------------|
| `$and`  | VÀ (AND)                 | `db.users.find({ $and: [{ age: 25 }, { name: "Alice" }] })`           |
| `$or`   | HOẶC (OR)                | `db.users.find({ $or: [{ age: 25 }, { age: 30 }] })`                  |
| `$not`  | PHỦ ĐỊNH (NOT)           | `db.users.find({ age: { $not: { $lt: 20 } } })`                       |
| `$nor`  | PHỦ ĐỊNH HOẶC (NOR)      | `db.users.find({ $nor: [{ age: 25 }, { name: "Bob" }] })`             |
- Toán tử mảng
| Toán tử | Ý nghĩa                     | Ví dụ                                      |
|---------|----------------------------|--------------------------------------------|
| `$in`   | Giá trị thuộc mảng          | `db.users.find({ age: { $in: [20, 25, 30] } })` |
| `$nin`  | Giá trị không thuộc mảng    | `db.users.find({ age: { $nin: [20, 25] } })` |
| `$all`  | Tất cả giá trị trong mảng   | `db.users.find({ tags: { $all: ["vip", "admin"] } })` |
| `$size` | Độ dài mảng                | `db.users.find({ tags: { $size: 2 } })`    |
-  Toán tử trên trường nested/object
| Toán tử        | Ý nghĩa                          | Ví dụ                                                                 |
|----------------|---------------------------------|-----------------------------------------------------------------------|
| `$elemMatch`   | Khớp phần tử trong mảng          | `db.users.find({ orders: { $elemMatch: { price: { $gt: 100 } } } })`  |
| Dot Notation   | Truy cập trường con              | `db.users.find({ "address.city": "Hanoi" })`                          |

## 🔍 Tìm gần đúng bằng Regex
```js
	db.users.find({ name: { $regex: "bối", $options: "i" } })
// Tìm tên chứa "bối", không phân biệt chữ hoa
```
##  🎯 Tìm trong danh sách với $in
```js
	db.users.find({ role: { $in: ["ADMIN", "MOD"] } })
```
## 🔄 Kết hợp điều kiện ($or, $and)
```js
	db.users.find({
		$or: [
			{ age: { $lt: 18 } },
			{ role: "GUEST" }
		]
})

```
## Projection - Chọn trường hiển thị
- 1: Hiển thị trường (ngoại trừ _id, mặc định hiển thị nếu không loại trừ).
- 0: Ẩn trường.
```js
	// Chỉ hiển thị 'name' và 'email' (ẩn '_id')
	db.users.find({}, { name: 1, email: 1, _id: 0 });

	// Ẩn trường 'password'
	db.users.find({}, { password: 0 });
```