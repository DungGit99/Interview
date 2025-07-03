# Server Actions trong Next.js

## 🔍 Server Actions là gì?

Server Actions là một **tính năng mới trong Next.js App Router** (từ v13.4 trở lên) cho phép bạn **gọi hàm server-side trực tiếp từ client** mà không cần tạo thủ công các API route như `/api/xyz`.

Nó giúp đơn giản hóa việc xử lý form, mutation (POST, PUT, DELETE) mà vẫn giữ được:

- Bảo mật (code chạy trên server)
- Tránh viết boilerplate (tạo API route riêng)
- Tối ưu hóa hiệu suất (gọi trực tiếp server mà không cần roundtrip JSON logic thủ công)

---

## ✅ Tại sao Server Actions quan trọng?

| Ưu điểm | Mô tả |
|--------|-------|
| 🔐 **Bảo mật hơn** | Logic xử lý chạy **trên server**, nên không lộ ra trên client |
| 🚫 **Không cần API route** | Không phải tạo `/api/user`, thay vào đó gọi `await addUser(formData)` là xong |
| ✨ **Tích hợp mượt với form** | Server Actions hoạt động cực tốt với `<form action={serverAction}>...</form>` |
| ⚡ **Giảm client-side JS** | Không cần thêm thư viện như Axios/Fetch trên client |
| 🧩 **Gắn chặt với RSC (React Server Component)** | Kết hợp với Server Component để tối ưu performance |

---

## 🛠️ Ví dụ: Submit form với Server Actions

## 🧠 Điểm cần lưu ý
- Server Actions chỉ hoạt động trong App Router (thư mục app/)

- Hàm server phải có 'use server' và được export

- Chỉ dùng được trong React Server Components hoặc form action=...

- Nếu bạn muốn gọi Server Action từ client (JS event), cần dùng useTransition hoặc startTransition
### 1. Tạo Server Action
```tsx
// app/actions.ts (server file)
'use server';

import { revalidatePath } from 'next/cache';

export async function addTodo(formData: FormData) {
  const todo = formData.get('todo')?.toString();

  if (!todo) return;

  // Giả sử bạn insert vào DB ở đây
  console.log('Adding todo:', todo);

  // Tự động revalidate lại UI
  revalidatePath('/todos');
}
