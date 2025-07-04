
# Next.js là gì? Nó khác gì với React.js?

## 1. Next.js là gì?

Next.js là một **framework React** được phát triển bởi Vercel, giúp phát triển các ứng dụng web dễ dàng hơn nhờ tích hợp sẵn các tính năng như:

- Server Side Rendering (SSR)
- Static Site Generation (SSG)
- Incremental Static Regeneration (ISR)
- Routing theo file
- API routes
- Tối ưu hóa hình ảnh và performance

## 2. React.js là gì?

React.js là một **thư viện JavaScript** được phát triển bởi Facebook dùng để xây dựng giao diện người dùng (UI). React tập trung vào phần View trong mô hình MVC.

## 3. So sánh Next.js và React.js

| Tiêu chí                | React.js                        | Next.js                                      |
|------------------------|----------------------------------|----------------------------------------------|
| Loại                   | Thư viện (Library)              | Framework                                    |
| Routing                | Cần thư viện ngoài (react-router)| Tích hợp sẵn                                 |
| SSR/SSG                | Không hỗ trợ mặc định           | Hỗ trợ sẵn                                   |
| API Routes             | Không có                        | Có thể tạo trong `/pages/api`                |
| SEO                    | Khó tối ưu                      | Dễ dàng nhờ SSR/SSG                          |
| Tối ưu hóa             | Thủ công                       | Tự động (code splitting, lazy load,...)      |
| Triển khai             | Tự cấu hình                     | Dễ triển khai qua Vercel hoặc server bất kỳ  |

## 4. Khi nào nên dùng Next.js?

- Cần SEO tốt
- Cần SSR hoặc SSG
- Ứng dụng production-ready
- Tối ưu performance

## 5. Khi nào nên dùng React.js?

- Dự án nhỏ hoặc vừa
- Single Page Application (SPA)
- Muốn kiểm soát toàn bộ stack

## 6. Kết luận

React.js là nền tảng để xây dựng UI còn Next.js mở rộng React để xây dựng web app hoàn chỉnh. Nếu bạn bắt đầu với React, học Next.js sẽ giúp bạn phát triển ứng dụng hiệu quả hơn trong môi trường thực tế.
