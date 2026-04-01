Nếu Mảng (Array) làm bạn đau đầu vì kích thước cố định, thì `List` sinh ra để đập tan giới hạn đó. Dưới đây là trọn bộ "bí kíp" về giao diện `List`.

---

## 1. 3 Đặc quyền "độc nhất" của List

Vì `List` kế thừa từ `Collection`, nó có tất cả các tính năng cơ bản như thêm, xóa, kiểm tra kích thước. Tuy nhiên, `List` mang 3 đặc điểm "cốt lõi" khiến nó khác biệt hoàn toàn với `Set` hay `Queue`:

1. **Có thứ tự (Ordered):** `List` duy trì tuyệt đối **thứ tự chèn (insertion order)**. Bạn thêm phần tử A vào trước, B vào sau, thì khi in ra chắc chắn A sẽ đứng trước B.
    
2. **Cho phép trùng lặp (Duplicates Allowed):** Bạn có thể nhét 100 chữ "Java" hoặc thậm chí nhiều giá trị `null` vào trong một `List` mà không gặp lỗi gì.
    
3. **Truy cập qua Chỉ số (Index-based):** Giống hệt mảng, các phần tử trong `List` được đánh số thứ tự từ `0` đến `size() - 1`. Bạn có thể chỉ đích danh: _"Lấy cho tôi phần tử ở vị trí số 3!"_
    

---

## 2. Các phương thức "chỉ List mới có"

Nhờ đặc tính quản lý bằng **Index (Chỉ số)**, giao diện `List` cung cấp thêm một loạt các phương thức cực mạnh mà `Collection` thông thường không có:

|Phương thức|Ý nghĩa|Ví dụ (Giả sử list là `["A", "B"]`)|
|---|---|---|
|`add(int index, E element)`|Chèn phần tử vào một vị trí cụ thể.|`list.add(1, "C")` → `["A", "C", "B"]`|
|`get(int index)`|Lấy ra phần tử ở vị trí `index`.|`list.get(0)` → `"A"`|
|`set(int index, E element)`|Thay thế phần tử ở vị trí `index`.|`list.set(1, "X")` → `["A", "X"]`|
|`remove(int index)`|Xóa phần tử tại vị trí `index`.|`list.remove(0)` → `["X"]`|
|`indexOf(Object o)`|Trả về vị trí **đầu tiên** tìm thấy phần tử.|`list.indexOf("X")` → `0` (Nếu không có trả về `-1`)|

---

## 3. Các "Hảo thủ" triển khai (Implementations) giao diện List

`List` chỉ là bản thiết kế (interface). Để dùng được, bạn phải khởi tạo thông qua các lớp con của nó. Dưới đây là 3 gương mặt cộm cán nhất:

### 🚀 `ArrayList` (Mảng động)

- **Bản chất:** Sử dụng một mảng (Array) bên dưới vỏ bọc. Khi mảng đầy, nó tự động tạo một mảng mới to hơn (gấp rưỡi mảng cũ) và copy dữ liệu sang.
    
- **Ưu điểm:** Tốc độ truy xuất dữ liệu (đọc) cực kỳ nhanh `O(1)` vì dùng index trực tiếp.
    
- **Nhược điểm:** Thêm hoặc xóa phần tử ở **đầu hoặc giữa** danh sách rất chậm, vì nó phải dịch chuyển (shift) tất cả các phần tử phía sau lùi lại hoặc tiến lên.
    

### 🔗 `LinkedList` (Danh sách liên kết kép)

- **Bản chất:** Các phần tử (Node) không nằm liền kề nhau trong bộ nhớ. Mỗi Node sẽ lưu giá trị của nó kèm theo 2 "sợi dây" trỏ tới Node đứng trước và Node đứng sau nó.
    
- **Ưu điểm:** Thêm hoặc xóa phần tử cực nhanh `O(1)` (nếu đã biết vị trí), vì chỉ cần "cắt dây" và nối lại, không cần dịch chuyển ai cả.
    
- **Nhược điểm:** Truy cập ngẫu nhiên rất chậm `O(n)`. Muốn lấy phần tử thứ 1000, nó phải đi bộ từ phần tử đầu tiên, dò từng dây nối cho đến vị trí 1000.
    

### 🛡️ `Vector` (Lão tướng)

- **Bản chất:** Hoạt động y hệt `ArrayList`, nhưng các phương thức của nó được **đồng bộ hóa (Synchronized)**.
    
- **Ưu / Nhược:** Rất an toàn trong môi trường đa luồng (Multi-threading), nhưng bù lại tốc độ xử lý rất chậm. Hiện nay, lập trình viên thường dùng `ArrayList` kết hợp với các công cụ bọc luồng hiện đại hơn thay vì dùng `Vector`.
    

---

## 4. Cuộc chiến kinh điển: Khi nào dùng ArrayList, khi nào dùng LinkedList?

Đây là câu hỏi "ăn điểm" trong mọi buổi phỏng vấn Java Core. Hãy nhớ quy tắc ngón tay cái này:

- **Chọn `ArrayList` khi:** Ứng dụng của bạn chủ yếu là **ĐỌC / TÌM KIẾM** dữ liệu, ít khi phải thêm/xóa phần tử ở giữa chừng. (Thực tế: 90% các trường hợp bạn sẽ dùng `ArrayList`).
    
- **Chọn `LinkedList` khi:** Ứng dụng của bạn liên tục phải **THÊM / XÓA** dữ liệu ở đầu và giữa danh sách, và ít khi cần truy xuất ngẫu nhiên qua index.
    

Bạn muốn xem thử một đoạn code thực tế chứng minh sự chênh lệch tốc độ giữa `ArrayList` và `LinkedList` khi xử lý hàng triệu dữ liệu, hay muốn đi tiếp sang nhánh `Set` để khám phá tính năng lọc phần tử trùng lặp?