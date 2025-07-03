
# Tối Ưu SEO Trong Next.js

## 🎯 Mục tiêu

Tối ưu hóa SEO giúp trang web tăng thứ hạng tìm kiếm trên Google, cải thiện trải nghiệm người dùng và tăng lưu lượng truy cập.

---

## 🧠 Các Kỹ Thuật Tối Ưu SEO trong Next.js

### 1. ✅ Sử dụng `next/head` để thêm meta tags

```tsx
import Head from 'next/head'

export default function HomePage() {
  return (
    <>
      <Head>
        <title>Trang chủ - My Website</title>
        <meta name="description" content="Mô tả ngắn gọn về nội dung trang web" />
        <meta name="keywords" content="nextjs, seo, web development" />
        <meta property="og:title" content="Trang chủ - My Website" />
        <meta property="og:description" content="Mô tả dùng cho mạng xã hội" />
        <meta property="og:image" content="/og-image.jpg" />
      </Head>
      <main>...</main>
    </>
  )
}
```

---

### 2. 🧭 Tối ưu URL & Routing

- Sử dụng URL thân thiện: `/bai-viet/huong-dan-seo-trong-nextjs`
- Tránh query strings phức tạp như `/post?id=123`
- Dùng dynamic routes `/blog/[slug].tsx`

---

### 3. ⚡ SSR, SSG giúp SEO hiệu quả hơn

Next.js hỗ trợ:

- **getServerSideProps**: tốt cho nội dung cập nhật thường xuyên
- **getStaticProps**: tốt cho nội dung tĩnh (blog, landing page)

```ts
export async function getStaticProps() {
  const data = await fetchData()
  return {
    props: { data },
  }
}
```

---

### 4. 🔗 Thêm sitemap & robots.txt

Dùng gói `next-sitemap`:

```bash
npm install next-sitemap
```

Cấu hình `next-sitemap.config.js`:

```js
module.exports = {
  siteUrl: 'https://yourwebsite.com',
  generateRobotsTxt: true,
}
```

Thêm vào `package.json`:

```json
"scripts": {
  "postbuild": "next-sitemap"
}
```

---

### 5. 🖼 Tối ưu hình ảnh

- Dùng `<Image />` từ `next/image`
- Thêm thuộc tính `alt` mô tả nội dung ảnh
- Kích thước ảnh phù hợp và lazy loading tự động

```tsx
import Image from 'next/image'

<Image src="/banner.jpg" alt="Banner chính" width={800} height={400} />
```

---

### 6. 🧾 Thêm structured data (Schema.org)

Giúp Google hiểu nội dung trang:

```tsx
<script
  type="application/ld+json"
  dangerouslySetInnerHTML={{
    __html: JSON.stringify({
      "@context": "https://schema.org",
      "@type": "Organization",
      "name": "My Website",
      "url": "https://mywebsite.com",
      "logo": "https://mywebsite.com/logo.png"
    })
  }}
/>
```

---

### 7. 📱 Tối ưu tốc độ và Mobile-friendly

- Kiểm tra bằng [PageSpeed Insights](https://pagespeed.web.dev/)
- Dùng lazy loading, giảm bundle size
- Ưu tiên trải nghiệm mobile (responsive, tốc độ tải nhanh)

---

## 📚 Tài liệu tham khảo

- [Next.js SEO](https://nextjs.org/learn/seo/introduction)
- [next-seo](https://github.com/garmeeh/next-seo)
- [next-sitemap](https://www.npmjs.com/package/next-sitemap)
