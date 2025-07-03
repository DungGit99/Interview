# 🧠 SWR là gì?
SWR viết tắt của: Stale-While-Revalidate: Dữ liệu cũ vẫn được dùng trước, đồng thời gửi request mới để cập nhật.
- ✅ Tự động cache và revalidate dữ liệu
- ✅ Tự động re-fetch khi chuyển tab quay lại
- ✅ Dễ dùng, kết hợp tốt với Next.js CSR
## 🔁 Các tính năng nổi bật
- Revalidation tự động
- Khi window focus
- Khi reconnect
- Có thể cấu hình:
- useSWR('/api/user', fetcher, {
  refreshInterval: 5000, // re-fetch mỗi 5 giây
  revalidateOnFocus: true,
  revalidateOnReconnect: true,
})