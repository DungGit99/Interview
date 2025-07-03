
# 📦 Code Splitting & Giảm Bundle Size trong Next.js

Next.js có sẵn nhiều tính năng hỗ trợ **code splitting tự động**, nhưng bạn vẫn có thể chủ động **tối ưu thêm** để cải thiện hiệu suất, đặc biệt với các app lớn.

---

## 🚀 1. Code Splitting là gì?

**Code Splitting** là kỹ thuật chia nhỏ code thành nhiều phần (chunk), để:
- Trình duyệt **chỉ tải những phần cần thiết** khi load trang.
- Giảm thời gian tải ban đầu.
- Tăng hiệu suất và UX.

Next.js hỗ trợ sẵn:
- **Automatic per-page code splitting**
- **Dynamic import**

---

## 📁 2. Mặc định: Code splitting theo từng trang

Mỗi file trong thư mục `pages/` được bundle riêng biệt.

📝 Ví dụ:

```
pages/
  index.tsx        --> chỉ bundle code của trang index
  about.tsx        --> chỉ bundle code của about
```

➡️ Người dùng truy cập `/about` sẽ **không tải code của `/index`**.

---

## 🧩 3. Dynamic Import (Import động)

Tự chia nhỏ component khi cần dùng.

### 🔧 Cách dùng:

```tsx
import dynamic from 'next/dynamic';

const HeavyComponent = dynamic(() => import('../components/HeavyComponent'), {
  loading: () => <p>Loading...</p>,
  ssr: false, // không render ở server nếu không cần
});
```

### ✅ Dùng khi:
- Component lớn, ít dùng.
- Chỉ hiển thị trong modal, tab, dialog,...

---

## 🎯 4. Chia nhỏ thư viện lớn

Ví dụ: chỉ import chức năng cần dùng thay vì cả thư viện.

```ts
// ❌ Tránh
import _ from 'lodash';

// ✅ Tốt hơn
import debounce from 'lodash/debounce';
```

---

## 📦 5. Phân tách Vendor & App Code

Next.js tự động tách các thư viện bên ngoài (`node_modules`) thành file riêng để cache lâu hơn.

Bạn có thể kiểm tra bằng công cụ **Next.js Analyzer**.

---

## 🛠 6. Phân tích Bundle Size

### Cài đặt plugin:
```bash
npm install @next/bundle-analyzer
```

### Cấu hình trong `next.config.js`:
```js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer({
  // cấu hình khác ở đây
});
```

### Chạy phân tích:
```bash
ANALYZE=true npm run build
```

Xem báo cáo tại: `http://localhost:3000/_next/static/analyze/...`

---

## 🧼 7. Xoá unused code & component

- Xoá component, hook, lib không còn dùng.
- Kiểm tra `tree-shaking`: tránh export tất cả từ 1 file.
- Tránh import `moment.js` nếu không cần → thay bằng `dayjs`.

---

## 🧠 8. Tối ưu ảnh và font

- Dùng `<Image>` của Next.js (hỗ trợ lazy load + responsive).
- Dùng font qua `@next/font` để **giảm font size**, tránh tải font không dùng.

```tsx
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });
```

---

## ✅ Tổng kết

| Kỹ thuật                        | Tác dụng                             |
|--------------------------------|--------------------------------------|
| Dynamic Import                 | Chia nhỏ component khi cần           |
| Per-page code splitting        | Mỗi trang chỉ tải code riêng         |
| Import selective từ thư viện   | Giảm dung lượng                      |
| Bundle Analyzer                | Xác định file nào nặng               |
| Dùng Image và Font của Next.js | Tối ưu ảnh, giảm font                |
| Tree-shaking và xóa unused code| Tránh tải code dư                    |

---

👉 Sử dụng đúng kỹ thuật sẽ giúp app Next.js của bạn **nhẹ hơn, nhanh hơn và mượt hơn**.
