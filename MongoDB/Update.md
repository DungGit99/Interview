# Sửa dữ liệu (Update)

- Cập nhật một document
```js
	db.collection.updateOne(
  		{ filter },
  		{ $set: { field: newValue } }
	)
	db.users.updateOne(
  		{ name: "Alice" },
  		{ $set: { age: 26 } } // Đổi tuổi Alice thành 26
	)
```
- Cập nhật nhiều documents
```js
	db.collection.updateMany(
		{ filter },
		{ $set: { field: newValue } }
	)
	db.users.updateMany(
		{ age: { $lt: 20 } },
		{ $set: { status: "teen" } } // Gán status="teen" cho tất cả user < 20 tuổi
	)
```
| Toán tử   | Công dụng                  | Ví dụ                          |
|-----------|----------------------------|--------------------------------|
| `$set`    | Gán giá trị mới            | `{ $set: { age: 26 } }`        |
| `$unset`  | Xóa trường                 | `{ $unset: { password: "" } }` |
| `$inc`    | Tăng/giảm giá trị          | `{ $inc: { age: 1 } }`         |
| `$push`   | Thêm phần tử vào mảng      | `{ $push: { tags: "vip" } }`   |
| `$pull`   | Xóa phần tử khỏi mảng      | `{ $pull: { tags: "vip" } }`   |