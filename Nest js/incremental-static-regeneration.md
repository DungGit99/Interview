
# Incremental Static Regeneration (ISR) trong Next.js

## 1. ISR là gì?

**Incremental Static Regeneration (ISR)** là một tính năng của Next.js cho phép bạn cập nhật các trang tĩnh đã được tạo ra bằng Static Site Generation (SSG) **mà không cần phải rebuild toàn bộ website**.

- ISR giúp bạn kết hợp hiệu năng cao của SSG với khả năng cập nhật nội dung động.
- Next.js sẽ phục vụ phiên bản cũ đã được cache, đồng thời tạo ra một phiên bản mới phía server nếu có request đến sau khoảng thời gian `revalidate`.

---

## 2. Cách dùng `revalidate` trong getStaticProps

Để sử dụng ISR, bạn chỉ cần thêm thuộc tính `revalidate` trong hàm `getStaticProps`:

```tsx
export async function getStaticProps(context) {
  const data = await fetchDataFromAPI();

  return {
    props: {
      data,
    },
    revalidate: 60, // tính bằng giây - sau 60 giây, trang sẽ được tạo lại khi có request mới
  };
}
```

### Giải thích:
- `revalidate: 60` có nghĩa là trang sẽ được tái tạo sau mỗi 60 giây (khi có request).
- Người dùng luôn nhận được trang tĩnh nhanh chóng.
- Phiên bản mới sẽ được cập nhật ở backend và thay thế bản cũ.

---

## 3. ISR kết hợp với dynamic routes

Trong các route động như blog `[slug].tsx`, bạn cần kết hợp với `getStaticPaths` và `fallback`.

```tsx
export async function getStaticPaths() {
  const posts = await getAllPosts();

  return {
    paths: posts.map(post => ({ params: { slug: post.slug } })),
    fallback: 'blocking',
  };
}
```

### fallback options:
- `'blocking'`: Đợi trang tạo xong mới trả về.
- `true`: Hiện trang loading trong khi chờ tạo trang.
- `false`: Chỉ dựng những trang trong `paths`.

---

## 4. Lưu ý

- ISR chỉ hoạt động trong môi trường production (VD: Vercel).
- Không hoạt động trong `next dev`.
- Phù hợp với các trang: blog, sản phẩm, bài viết CMS,...

