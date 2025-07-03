# So sánh SSR, SSG, ISR và CSR trong Next.js

Next.js hỗ trợ nhiều phương pháp render khác nhau, mỗi phương pháp có ưu và nhược điểm riêng, phù hợp với từng loại ứng dụng khác nhau.

---

## 1. SSR (Server-side Rendering)

### ✅ Đặc điểm:
- Trang được render **trên server mỗi lần có request**.
- Dữ liệu luôn **mới nhất**.
- Sử dụng `getServerSideProps`.

### ✅ Ưu điểm:
- Phù hợp với nội dung thay đổi liên tục theo người dùng (ví dụ: dashboard, profile, tin tức mới nhất).
- Tốt cho SEO vì nội dung có sẵn khi trình thu thập dữ liệu truy cập.

### ❌ Nhược điểm:
- Thời gian phản hồi lâu hơn vì phải render trên server mỗi lần.
- Tăng tải cho server.

### ✅ Ví dụ:
```tsx
export async function getServerSideProps(context) {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return { props: { data } };
}
```

---

## 2. SSG (Static Site Generation)

### ✅ Đặc điểm:
- Trang được render **một lần tại build time**.
- Nội dung là **tĩnh** và không đổi sau khi build.
- Sử dụng `getStaticProps`.

### ✅ Ưu điểm:
- Hiệu năng rất cao, tải nhanh vì là file HTML tĩnh.
- Chi phí server thấp.
- Tốt cho SEO.

### ❌ Nhược điểm:
- Không phù hợp với nội dung thay đổi thường xuyên.
- Phải **rebuild lại site** khi dữ liệu thay đổi.

### ✅ Ví dụ:
```tsx
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return { props: { data } };
}
```

---

## 3. ISR (Incremental Static Regeneration)

### ✅ Đặc điểm:
- Kết hợp ưu điểm của SSG và SSR.
- Trang được tạo **tĩnh ban đầu**, sau đó **tái tạo lại phía sau hậu trường** nếu có truy cập và dữ liệu đã cũ.
- Sử dụng `revalidate` trong `getStaticProps`.

### ✅ Ưu điểm:
- Cập nhật dữ liệu mới mà không cần build lại toàn bộ site.
- Truy cập lần đầu sẽ dùng dữ liệu cũ (tĩnh), lần sau có thể dùng bản mới.

### ❌ Nhược điểm:
- Dữ liệu có thể **chưa được cập nhật ngay lập tức** cho người dùng đầu tiên.

### ✅ Ví dụ:
```tsx
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
    revalidate: 60, // tái tạo mỗi 60 giây
  };
}
```

---

## 4. CSR (Client-side Rendering)

### ✅ Đặc điểm:
- Trang được render hoàn toàn **trên trình duyệt** sau khi tải JavaScript.
- Dữ liệu được fetch bằng `useEffect` hoặc các thư viện như SWR, React Query.

### ✅ Ưu điểm:
- Phù hợp với các phần **không cần SEO** hoặc phụ thuộc vào **người dùng đăng nhập**.
- Tránh tải server khi dữ liệu không cần sẵn trên HTML.

### ❌ Nhược điểm:
- SEO không tốt (trừ khi dùng hydrate sau).
- Trải nghiệm người dùng ban đầu kém hơn nếu dữ liệu tải lâu.

### ✅ Ví dụ:
```tsx
import { useEffect, useState } from 'react';

export default function Page() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('/api/data')
      .then((res) => res.json())
      .then(setData);
  }, []);

  if (!data) return <p>Loading...</p>;

  return <div>{data.title}</div>;
}
```

---

## 📝 Tóm tắt so sánh

| Cách render | Thời điểm render      | SEO tốt | Dữ liệu mới | Hiệu năng |
|-------------|------------------------|---------|-------------|------------|
| **SSR**     | Mỗi request (server)   | ✅      | ✅          | ❌         |
| **SSG**     | Lúc build              | ✅      | ❌ (cũ)     | ✅✅✅     |
| **ISR**     | Build + tái tạo định kỳ| ✅      | ✅ (trễ)    | ✅✅       |
| **CSR**     | Trình duyệt            | ❌      | ✅          | ✅ (sau khi tải JS) |

---

## ✅ Khi nào dùng cái nào?

- **SSR**: Trang dashboard, dữ liệu theo user đăng nhập.
- **SSG**: Trang blog, landing page ít thay đổi.
- **ISR**: Blog/tin tức cập nhật thường xuyên nhưng không yêu cầu cập nhật ngay lập tức.
- **CSR**: Trang nội bộ, không cần SEO như trang quản lý, trang cài đặt người dùng.