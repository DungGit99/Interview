
# Cách Lazy Loading Component hoặc Image trong Next.js

## 🟦 1. Lazy Loading Component trong Next.js

### ✅ Dùng `next/dynamic`

Next.js cung cấp `dynamic()` để tải **component một cách động**, chỉ khi cần thiết (khi component xuất hiện trên màn hình hoặc khi người dùng tương tác).

### 🔧 Ví dụ:
```tsx
// pages/index.tsx
import dynamic from 'next/dynamic';

// Component này sẽ chỉ được tải khi được render
const LazyComponent = dynamic(() => import('../components/MyComponent'), {
  loading: () => <p>Đang tải...</p>, // (tuỳ chọn) loading fallback
  ssr: false, // (tuỳ chọn) tắt render phía server nếu cần
});

export default function Home() {
  return (
    <div>
      <h1>Trang chính</h1>
      <LazyComponent />
    </div>
  );
}
```

### 🧠 Ghi chú:
- `ssr: false`: Nếu bạn **không muốn SSR** (component chỉ có client)
- Tốt cho component phụ: modal, chart, map, video player…

---

## 🟨 2. Lazy Loading Image với `next/image`

`next/image` hỗ trợ **lazy loading mặc định** cho ảnh bằng thuộc tính `loading="lazy"` (tự động hoặc thủ công).

### 🔧 Ví dụ:
```tsx
import Image from 'next/image';

export default function Avatar() {
  return (
    <Image
      src="/avatar.png"
      alt="Avatar người dùng"
      width={200}
      height={200}
      priority={false} // Mặc định sẽ lazy load
    />
  );
}
```

### 🔍 Giải thích:
| Thuộc tính | Ý nghĩa |
|------------|--------|
| `priority={true}` | Không lazy load, dùng cho ảnh quan trọng (hero image) |
| `priority={false}` hoặc không dùng | Sẽ lazy load mặc định |
| `placeholder="blur"` | Làm mờ khi tải ảnh |
| `loading="lazy"` | Bổ sung nếu dùng thẻ `<img>` thủ công |

---

## 🧪 So sánh: Lazy loading component vs image

| Tính năng | Component | Image |
|----------|-----------|-------|
| Công cụ | `next/dynamic` | `next/image` |
| Lazy mặc định? | ❌ Không | ✅ Có |
| SSR hỗ trợ? | ✅ Có (nếu không tắt `ssr`) | ✅ Có |
| Thường dùng cho | Biểu đồ, Modal, Form phức tạp | Ảnh trong danh sách, gallery |

---

## ✅ Kết luận

- Dùng `next/dynamic()` cho component ít quan trọng, tải sau khi user cuộn đến.
- Dùng `next/image` để ảnh được lazy load mặc định, tiết kiệm băng thông.
- Giúp giảm **initial load**, tăng hiệu suất và trải nghiệm người dùng.
