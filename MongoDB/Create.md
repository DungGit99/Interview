# Thêm dữ liệu (Create)

- Thêm dữ liệu (Create)
```js
	db.collection.insertOne({ field: value })

	db.users.insertOne({
		name: "Alice",
		age: 25,
		email: "alice@example.com"
	})
```
- Thêm nhiều documents
```js
	db.collection.insertMany([{ doc1 }, { doc2 }])
	db.users.insertMany([
		{ name: "Bob", age: 30 },
		{ name: "Charlie", age: 22 }
	])

```