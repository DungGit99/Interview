
# Catch-all Routes và Optional Catch-all Routes trong Next.js

## 🧩 Catch-all Routes là gì?

Catch-all routes cho phép bạn tạo ra một route động có thể khớp với nhiều phần của URL.

### 📌 Cú pháp:

```bash
/pages/post/[...slug].tsx
```

Ví dụ URL phù hợp:

- `/post/a`
- `/post/a/b`
- `/post/a/b/c`

### 🎯 Cách sử dụng trong Component:

```tsx
import { useRouter } from 'next/router'

export default function Post() {
  const router = useRouter()
  const { slug } = router.query

  return <div>Slug: {JSON.stringify(slug)}</div>
}
```

Giá trị `slug` sẽ là một mảng, ví dụ: `['a', 'b']`

---

## 🧩 Optional Catch-all Routes là gì?

Optional catch-all routes là một biến thể của catch-all nhưng **cho phép đường dẫn rỗng** (tức là route gốc cũng hợp lệ).

### 📌 Cú pháp:

```bash
/pages/docs/[[...params]].tsx
```

### Ví dụ URL phù hợp:

- `/docs`
- `/docs/a`
- `/docs/a/b`

> Nếu người dùng vào `/docs`, biến `params` sẽ là `undefined` hoặc `[]`

### 🎯 Cách sử dụng:

```tsx
import { useRouter } from 'next/router'

export default function Docs() {
  const router = useRouter()
  const { params } = router.query

  return <div>Params: {JSON.stringify(params)}</div>
}
```

---

## 🛠 So sánh nhanh

| Route Pattern         | Loại              | URL Hợp lệ                     | Giá trị query |
|-----------------------|-------------------|---------------------------------|----------------|
| `[...slug]`           | Catch-all         | `/a/b/c`                        | `['a','b','c']` |
| `[[...slug]]`         | Optional Catch-all| `/`, `/a/b`, `/slug`            | `undefined` hoặc `['a']` |

---

## 📚 Tài liệu tham khảo

- [Catch-all Routes - Next.js Docs](https://nextjs.org/docs/routing/dynamic-routes#catch-all-routes)
