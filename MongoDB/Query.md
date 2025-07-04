# 🧠 Query trong MongoDB" là gì?

## 📘 Các kiểu query phổ biến:
1. 🎯 Tìm theo điều kiện chính xác
```js
	db.users.find({ name: "Bối Bối" })
```
2. 🔢 So sánh giá trị
| Toán tử | Ý nghĩa           |
| ------- | ----------------- |
| `$gt`   | lớn hơn           |
| `$lt`   | nhỏ hơn           |
| `$gte`  | lớn hơn hoặc bằng |
| `$lte`  | nhỏ hơn hoặc bằng |
| `$ne`   | khác              |

3. 🔍 Tìm gần đúng bằng Regex
```js
	db.users.find({ name: { $regex: "bối", $options: "i" } })
// Tìm tên chứa "bối", không phân biệt chữ hoa
```
4. 🎯 Tìm trong danh sách với $in
```js
	db.users.find({ role: { $in: ["ADMIN", "MOD"] } })
```
5. 🔄 Kết hợp điều kiện ($or, $and)
```js
	db.users.find({
		$or: [
			{ age: { $lt: 18 } },
			{ role: "GUEST" }
		]
})

```