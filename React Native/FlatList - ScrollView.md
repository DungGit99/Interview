# So sánh FlatList và ScrollView trong React Native
| Tiêu chí                 | `FlatList`                                             | `ScrollView`                                       |
| ------------------------ | ------------------------------------------------------ | -------------------------------------------------- |
| **Chức năng chính**      | Dành cho danh sách dài, có khả năng tối ưu hiệu suất   | Dành cho nội dung có kích thước cuộn được          |
| **Cơ chế render**        | **Lazy load (theo batch)** – chỉ render item thấy được | Render **toàn bộ** nội dung 1 lần                  |
| **Hiệu năng**            | Tốt hơn với danh sách dài                              | Tốn RAM, dễ crash với danh sách lớn                |
| **Hỗ trợ header/footer** | Có `ListHeaderComponent`, `ListFooterComponent`        | Không hỗ trợ mặc định, phải tự chèn                |
| **Props đặc biệt**       | `keyExtractor`, `initialNumToRender`, `onEndReached`   | `horizontal`, `contentContainerStyle`, `onScroll`  |
| **Tái sử dụng item**     | Có (dùng `recycle`, `renderItem`)                      | Không có tái sử dụng item                          |
| **Dễ sử dụng**           | Cần truyền `data`, `renderItem` rõ ràng                | Có thể wrap mọi thứ mà không cần cấu trúc đặc biệt |
| **Tùy chỉnh danh sách**  | Dễ thêm phân cách (`ItemSeparatorComponent`)           | Phải tự thêm phân cách                             |

## ✔ Dùng FlatList khi:
- Danh sách lớn (1000+ items)
- Dữ liệu động, có thể thay đổi
- Cần tối ưu hiệu năng (lazy loading, virtualization)
- Cần tích hợp pull-to-refresh, infinite scroll

## ✔ Dùng ScrollView khi:
- Danh sách ngắn (<20 items)
- Layout đơn giản, không cần lazy loading
- Muốn render tất cả ngay từ đầu (phù hợp cho màn hình tĩnh)

## Ví dụ minh họa

1.  ScrollView (không tối ưu cho danh sách dài)
```jsx
<ScrollView>
  {data.map((item) => (
    <View key={item.id}>
      <Text>{item.name}</Text>
    </View>
  ))}
</ScrollView>
```
❌ Vấn đề: Render tất cả items cùng lúc → lag nếu data lớn.

1.  FlatList (tối ưu cho danh sách lớn)
```jsx
<FlatList
  data={data}
  renderItem={({item}) => <Text>{item.name}</Text>}
  keyExtractor={(item) => item.id}
  initialNumToRender={10} // Chỉ render 10 item đầu
  windowSize={5} // Giữ 5 màn hình trong bộ nhớ
  maxToRenderPerBatch={5} // Mỗi lần render thêm 5 item
/>
```

# Ứng dụng của bạn bị giật lag khi hiển thị danh sách 1000+ item. Bạn sẽ tối ưu thế nào?

1. Sử dụng FlatList thay cho ScrollView:
- FlatList chỉ render các item đang hiển thị trên màn hình (lazy loading)
- ScrollView render tất cả items cùng lúc → tốn hiệu năng

2. Tối ưu renderItem với React.memo:
3.