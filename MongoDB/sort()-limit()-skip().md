# .sort(), .limit(), .skip()

## ✅ 1. .sort(): Sắp xếp kết quả
- Sắp xếp các document trả về theo trường (1: tăng dần, -1: giảm dần).
- Cú pháp:
```js
	db.collection.find().sort({ field: 1 | -1 })
```
- Ví dụ:
```js
	// Sắp xếp tăng dần theo tuổi (age)
	db.users.find().sort({ age: 1 });

	// Sắp xếp giảm dần theo tên (name)
	db.users.find().sort({ name: -1 });

	// Sắp xếp theo nhiều trường (tuổi tăng dần, sau đó tên giảm dần)
	db.users.find().sort({ age: 1, name: -1 });
```
## ✅ 2. .limit(n): Giới hạn số lượng kết quả
- Giới hạn số lượng document trả về.
- Cú pháp:
```js
	db.collection.find().limit(number)
```
- Ví dụ:
```js
	// Lấy 5 user đầu tiên
	db.users.find().limit(5);

	// Kết hợp với sort (lấy 3 user có tuổi cao nhất)
	db.users.find().sort({ age: -1 }).limit(3);
```
## ✅ 3. .skip(n): Bỏ qua n kết quả đầu
- Bỏ qua N document đầu tiên (thường dùng để phân trang).
- Cú pháp:
```js
	db.collection.find().skip(number)
```
- Ví dụ:
```js
	// Bỏ qua 10 user đầu tiên, lấy các user tiếp theo
	db.users.find().skip(10);
	// Phân trang: Trang 2 (mỗi trang 5 user)
	const page = 2;
	const perPage = 5;
	db.users.find().skip((page - 1) * perPage).limit(perPage);
```

## Kết hợp .sort(), .limit(), .skip()
- Ví dụ: Lấy top 5 user có điểm cao nhất (bỏ qua 10 người đầu)
```js
	db.users.find()
	.sort({ score: -1 })  // Giảm dần theo điểm
	.skip(10)             // Bỏ qua 10 người đầu
	.limit(5);            // Lấy 5 người tiếp theos
```
- Ví dụ: Phân trang (trang 3, mỗi trang 10 kết quả)

```js
	const page = 3;
	const perPage = 10;

	db.products.find()
	.skip((page - 1) * perPage)
	.limit(perPage);
```
## Lưu ý khi sử dụng .skip() và .limit()
- Hiệu năng: .skip() có thể chậm với dữ liệu lớn (vì MongoDB phải đếm từng document).
- Cải thiện tốc độ:
  - Dùng _id hoặc trường đã index để phân trang thay vì .skip().

```js
// Thay vì dùng skip, lấy các document có _id > last_id
db.users.find({ _id: { $gt: last_id } }).limit(10);
```