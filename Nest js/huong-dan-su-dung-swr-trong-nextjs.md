
# 🚀 Sử dụng SWR trong Next.js

## 🧠 SWR là gì?

**SWR** (Stale-While-Revalidate) là thư viện fetch dữ liệu do Vercel phát triển. Nó giúp:

- ✅ Tự động cache và revalidate dữ liệu

- ✅ Tự động re-fetch khi chuyển tab quay lại

- ✅ Dễ dùng, kết hợp tốt với Next.js CSR

---

## 📦 Cài đặt

```bash
npm install swr
# hoặc
yarn add swr
```

---

## ⚙️ Cách dùng cơ bản

```tsx
// pages/example.tsx

import useSWR from 'swr'

// Hàm fetch dữ liệu
const fetcher = (url: string) => fetch(url).then(res => res.json())

export default function Example() {
  const { data, error, isLoading } = useSWR('/api/user', fetcher)

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Failed to load</div>

  return <div>Hello {data.name}!</div>
}
```

---

## 🔁 Revalidate và cấu hình nâng cao

```tsx
useSWR('/api/user', fetcher, {
  refreshInterval: 5000,           // Re-fetch mỗi 5s
  revalidateOnFocus: true,         // Khi chuyển tab quay lại
  revalidateOnReconnect: true      // Khi mất mạng và kết nối lại
})
```

---

## 🔒 Chỉ fetch khi có điều kiện (VD: có `id`)

```tsx
const { data } = useSWR(id ? `/api/user/${id}` : null, fetcher)
```

---

## 🧠 Dùng với TypeScript

```tsx
type User = {
  id: string;
  name: string;
}

const { data } = useSWR<User>('/api/user', fetcher)
```

---

## ♻️ Cập nhật dữ liệu với `mutate`

```tsx
import useSWR, { mutate } from 'swr'

const handleUpdate = async () => {
  await fetch('/api/user', {
    method: 'POST',
    body: JSON.stringify({ name: 'New Name' })
  })

  mutate('/api/user') // Re-fetch lại dữ liệu mới
}
```

---

## 🧪 Ví dụ đầy đủ: SWR + API Route Next.js

### 📄 API route: `/pages/api/user.ts`

```ts
export default function handler(req, res) {
  res.status(200).json({ name: 'Bối Bối' })
}
```

### 🧩 Component sử dụng SWR

```tsx
import useSWR from 'swr'

const fetcher = url => fetch(url).then(res => res.json())

export default function Home() {
  const { data, error, isLoading } = useSWR('/api/user', fetcher)

  if (isLoading) return <p>Đang tải...</p>
  if (error) return <p>Lỗi tải dữ liệu</p>

  return <p>Xin chào {data.name}</p>
}
```

---

## ✅ Khi nào nên dùng SWR?

| Trường hợp                          | Có nên dùng SWR? |
|-----------------------------------|------------------|
| CSR (Client Side Rendering)       | ✅ Có            |
| SSR với `getServerSideProps`      | ❌ Không         |
| Dữ liệu động / thường xuyên thay đổi | ✅ Có        |
| Dự án nhỏ gọn, không cần Redux     | ✅ Có            |

---

## 📚 Tài liệu tham khảo

- [SWR Docs](https://swr.vercel.app)
- [Next.js Docs](https://nextjs.org/docs)
