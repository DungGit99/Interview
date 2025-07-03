
# Tổng quan các phương thức fetch data trong Next.js

Next.js cung cấp nhiều phương thức để lấy dữ liệu, phù hợp với từng tình huống khác nhau: từ tĩnh (SSG), động (SSR), hoặc phía client (CSR). Dưới đây là tổng quan và hướng dẫn sử dụng:

---

## 📌 `getStaticProps` (SSG - Static Site Generation)

### ✅ Dùng khi:
- Dữ liệu **ít thay đổi**
- Muốn build trang **trước khi user truy cập**
- Tốc độ phản hồi nhanh (được cache CDN)

### 🛠 Ví dụ:

```ts
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  return {
    props: { posts },
    revalidate: 60, // ISR: tái tạo sau mỗi 60s nếu có truy cập
  };
}
```

---

## 🔁 `getServerSideProps` (SSR - Server Side Rendering)

### ✅ Dùng khi:
- Dữ liệu thay đổi **mỗi lần người dùng truy cập**
- Cần xử lý cookie, session, query...
- Cần dữ liệu chính xác tại thời điểm hiện tại

### 🛠 Ví dụ:

```ts
export async function getServerSideProps(context) {
  const res = await fetch('https://api.example.com/user', {
    headers: {
      Authorization: `Bearer ${context.req.cookies.token}`,
    },
  });
  const user = await res.json();

  return {
    props: { user },
  };
}
```

---

## ⚖️ So sánh `getStaticProps` vs `getServerSideProps`

| Tiêu chí                | `getStaticProps` (SSG)         | `getServerSideProps` (SSR)         |
|------------------------|--------------------------------|------------------------------------|
| Thời điểm chạy         | Lúc build (hoặc revalidate)    | Mỗi lần request                    |
| Dữ liệu động           | ❌ Không phù hợp                | ✅ Phù hợp                          |
| Tốc độ phản hồi        | ⚡ Rất nhanh                    | 🐢 Chậm hơn                         |
| Context request        | ❌ Không có                     | ✅ Có (cookies, headers, query...) |
| SEO                    | ✅ Tốt                          | ✅ Tốt                              |

---

## 🔄 ISR (Incremental Static Regeneration)

### ✅ Dùng khi:
- Trang tĩnh cần được cập nhật định kỳ
- Không muốn rebuild toàn site

### 🛠 Dùng với `getStaticProps`:

```ts
export async function getStaticProps() {
  const data = await fetch(...).then((res) => res.json());
  return {
    props: { data },
    revalidate: 60, // tái tạo sau mỗi 60 giây
  };
}
```

---

## 🧭 `getStaticPaths`

### ✅ Dùng khi:
- Trang có **đường dẫn động** (`[id].tsx`, `[slug].tsx`)
- Cần kết hợp với `getStaticProps`

### 🛠 Ví dụ:

```ts
export async function getStaticPaths() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  const paths = posts.map((post) => ({
    params: { slug: post.slug },
  }));

  return {
    paths,
    fallback: 'blocking', // hoặc true / false
  };
}
```

### ⚠️ fallback:
- `false`: chỉ build các path được liệt kê, còn lại 404
- `true`: build khi có request, hiện "Loading..."
- `'blocking'`: chờ build xong rồi trả về (không có loading)

---

## 🖥️ CSR (Client Side Rendering)

### ✅ Dùng khi:
- Dữ liệu **không cần SEO**
- Chỉ cần fetch sau khi render client
- Dùng trong dashboard, trang cá nhân

### 🛠 Ví dụ:

```tsx
import { useEffect, useState } from 'react';

export default function Page() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('/api/data')
      .then((res) => res.json())
      .then(setData);
  }, []);

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}
```

---

## 📊 Tổng so sánh

| Cách render        | SEO  | Tốc độ lần đầu | Dữ liệu động | Khi nào dùng                                  |
|--------------------|------|----------------|---------------|-----------------------------------------------|
| `getStaticProps`   | ✅    | ⚡ Rất nhanh     | ❌            | Nội dung tĩnh, ít thay đổi                    |
| `getStaticProps` + ISR | ✅ | ⚡ Nhanh        | ✅ (theo thời gian) | Nội dung tĩnh cần cập nhật định kỳ     |
| `getServerSideProps`| ✅   | 🐢 Chậm hơn     | ✅ (real-time) | Nội dung thay đổi theo từng request           |
| CSR (`useEffect`)  | ❌    | ⚡ Nhanh (HTML shell) | ✅ (sau khi load) | Dữ liệu không cần SEO, ví dụ: dashboard |
