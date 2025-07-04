# 🔄 ISR – Incremental Static Regeneration là gì?
## ISR là cơ chế kết hợp giữa SSG (Static Site Generation) và SSR (Server-Side Rendering) giúp bạn:
- là một tính năng trong Next.js cho phép bạn cập nhật các trang tĩnh (static pages) sau khi ứng dụng đã được build
- Nhưng vẫn có thể tái tạo (re-generate) trang sau một khoảng thời gian định sẵn mà không cần rebuild lại toàn bộ project.
- ✅ Mục tiêu: vừa nhanh như SSG, vừa cập nhật dữ liệu mới như SSR, và vẫn tốt cho SEO.

## 🛠 Cách hoạt động
1. Tạo trang tĩnh ban đầu: Khi build ứng dụng, Next.js tạo ra các trang tĩnh (SSG - Static Site Generation).
2. Tự động cập nhật sau một khoảng thời gian: Khi có request tới trang, Next.js kiểm tra xem trang đã cũ chưa (dựa trên thời gian revalidate). Nếu cần, nó sẽ tạo lại trang với dữ liệu mới.
3. Phục vụ người dùng ngay lập tức: Trong khi trang được tạo lại ở phía server, người dùng vẫn thấy phiên bản cũ. Sau khi hoàn thành, phiên bản mới sẽ được phục vụ cho các request tiếp theo.

## 📦 Cách dùng ISR trong Next.js
```tsx
// pages/posts/[id].tsx
export async function getStaticProps(context) {
  const res = await fetch(`https://api.example.com/posts/${context.params.id}`);
  const post = await res.json();

  return {
    props: { post },
    revalidate: 60, // Tái tạo trang sau mỗi 60 giây
  };
}

```
- Ví dụ: *revalidate*: 60 nghĩa là trang sẽ được kiểm tra và cập nhật mỗi 60 giây nếu có request mới.

## 🔹 Lợi ích của ISR
- ✅ Hiệu suất cao: Giống SSG, trang được tạo sẵn và cache.
- ✅ Dữ liệu luôn mới: Tự động cập nhật mà không cần rebuild toàn bộ ứng dụng.
- ✅ Tối ưu cho SEO: Trang tĩnh được index dễ dàng hơn so với CSR (Client-Side Rendering).