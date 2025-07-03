# Next.js Image Component

Next.js cung cấp component `<Image>` từ `next/image` để tối ưu hình ảnh tự động.

## Ưu điểm:

- Tự động lazy loading
- Tối ưu responsive
- Chuyển đổi định dạng WebP/AVIF
- Hỗ trợ placeholder mờ (blur)

---

## Sử dụng cơ bản

```tsx
import Image from 'next/image'

export default function MyImage() {
  return (
    <Image
      src="/images/example.jpg"
      alt="Mô tả ảnh"
      width={600}
      height={400}
      priority={false}
      placeholder="blur"
      blurDataURL="/images/placeholder.jpg"
    />
  )
}
```

**Giải thích:**

- `src`: đường dẫn ảnh
- `alt`: text thay thế
- `width` / `height`: kích thước
- `priority`: preload nếu đặt `true`
- `placeholder="blur"` + `blurDataURL`: hiệu ứng mờ khi tải

---

## Responsive layout

```tsx
<Image
  src="/images/example.jpg"
  alt="Mô tả"
  width={1200}
  height={800}
  sizes="(max-width: 768px) 100vw, 50vw"
/>
```
→ giúp responsive tối ưu theo viewport

## Fill layout

```tsx
<div style={{ position: 'relative', width: '100%', height: '400px' }}>
  <Image src="/images/example.jpg" alt="mô tả" fill />
</div>
```
→ fill giúp ảnh phủ kín container

---

## Notes

- Next.js Image luôn yêu cầu `width` và `height` hoặc `fill`.
- `priority` chỉ nên dùng cho ảnh trên màn hình đầu tiên.

---

# Hết
