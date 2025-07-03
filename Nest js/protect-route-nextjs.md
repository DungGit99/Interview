
# Bảo vệ Route trong Next.js bằng Middleware

## 🛡 Mục tiêu

Hướng dẫn cách bảo vệ route (chỉ cho phép truy cập nếu đã đăng nhập) trong Next.js bằng cách sử dụng Middleware.

---

## 📦 Yêu cầu

- Next.js 13+ (App Router hoặc Pages Router đều được)
- Sử dụng cookie/token để xác thực (ví dụ: JWT hoặc session token)

---

## 🧩 Tạo Middleware trong Next.js

### 1. Tạo file middleware.ts (hoặc .js) ở thư mục gốc của `app/` hoặc project:

```ts
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

// Danh sách các route cần bảo vệ
const protectedRoutes = ['/dashboard', '/profile', '/admin']

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token')?.value

  const isProtected = protectedRoutes.some((path) =>
    request.nextUrl.pathname.startsWith(path)
  )

  if (isProtected && !token) {
    // Nếu không có token, redirect về trang đăng nhập
    return NextResponse.redirect(new URL('/login', request.url))
  }

  return NextResponse.next()
}
```

---

### 2. Cấu hình matcher (tùy chọn):

Trong `middleware.ts`, thêm export matcher để giới hạn phạm vi middleware:

```ts
export const config = {
  matcher: ['/dashboard/:path*', '/profile/:path*', '/admin/:path*'],
}
```

---

## 🔐 Lưu Token ở đâu?

- Dùng `cookies()` trong `app` route để lấy token trong server
- Dùng `document.cookie` hoặc `js-cookie` cho Pages Router (client)

---

## 📁 Ví dụ thư mục:

```
/app
  /dashboard
    page.tsx
  /login
    page.tsx
middleware.ts
```

---

## 📌 Lưu ý:

- Middleware chỉ chạy **ở phía server** (Edge Runtime)
- Không thể truy cập `localStorage` trong Middleware
- Chỉ nên kiểm tra logic đơn giản (ví dụ: có JWT hay không)
- Middleware chạy trước render, nên rất hiệu quả cho bảo vệ route

---

## ✅ Bonus: Dùng Middleware với NextAuth

Nếu bạn dùng `next-auth`, có thể kiểm tra session như sau:

```ts
import { getToken } from 'next-auth/jwt'

export async function middleware(req: NextRequest) {
  const token = await getToken({ req, secret: process.env.NEXTAUTH_SECRET })

  const isAuthPage = req.nextUrl.pathname === '/login'

  if (!token && !isAuthPage) {
    return NextResponse.redirect(new URL('/login', req.url))
  }

  if (token && isAuthPage) {
    return NextResponse.redirect(new URL('/dashboard', req.url))
  }

  return NextResponse.next()
}
```

---

## 📚 Tài liệu tham khảo

- [Next.js Middleware Docs](https://nextjs.org/docs/app/building-your-application/routing/middleware)
- [NextAuth Middleware](https://next-auth.js.org/configuration/nextjs#middleware)
