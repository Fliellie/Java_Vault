Nếu `List` giống như một cuốn sổ ghi chép bạn có thể viết vào bất cứ trang nào, thì `Queue` lại mô phỏng chính xác việc **xếp hàng mua vé xem phim** ngoài đời thực.

---

## 1. Bản chất của Queue (Hàng đợi)

Nguyên tắc tối thượng của `Queue` gói gọn trong 4 chữ: **FIFO (First-In-First-Out - Vào trước Ra trước)**.

- Người nào xếp hàng đầu tiên sẽ được phục vụ và rời khỏi hàng đầu tiên.
    
- Người mới đến bắt buộc phải đứng vào cuối hàng.
    
- Bạn không thể chen ngang vào giữa hàng (trừ một số trường hợp đặc biệt như hàng đợi ưu tiên).
    

---

## 2. Các phương thức "sinh tử" của Queue

Khi làm việc với `Queue`, bạn phải đặc biệt chú ý vì các phương thức của nó được chia làm **2 nhóm rõ rệt**. Một nhóm sẽ văng lỗi (Exception) nếu có sự cố, nhóm còn lại sẽ xử lý êm đẹp bằng cách trả về `false` hoặc `null`.

Hãy ghi nhớ bảng đối chiếu cực kỳ quan trọng sau đây:

|Thao tác|Nhóm văng lỗi (Ném Exception)|Nhóm êm đẹp (Trả về giá trị đặc biệt)|Ý nghĩa|
|---|---|---|---|
|**Thêm vào cuối hàng**|`add(e)`|`offer(e)`|Thêm phần tử `e` vào cuối hàng đợi. Nếu hàng đợi bị giới hạn dung lượng và đang đầy, `add` ném lỗi `IllegalStateException`, còn `offer` trả về `false`.|
|**Lấy ra và Xóa (Phục vụ)**|`remove()`|`poll()`|Lấy phần tử đứng đầu hàng và **xóa nó** khỏi hàng đợi. Nếu hàng rỗng, `remove` ném lỗi `NoSuchElementException`, còn `poll` trả về `null`.|
|**Chỉ nhìn (Không xóa)**|`element()`|`peek()`|Xem phần tử đứng đầu hàng là ai nhưng **không xóa**. Nếu hàng rỗng, `element` ném lỗi, còn `peek` trả về `null`.|

> **Mẹo thực chiến:** Trong 99% trường hợp thực tế, lập trình viên ưu tiên sử dụng nhóm **`offer()`, `poll()`, `peek()`** để chương trình chạy mượt mà, không bị sập (crash) đột ngột khi hàng đợi rỗng hoặc đầy.

---

## 3. Các "Gương mặt đại diện" (Implementations) của Queue

Giao diện `Queue` không thể tự khởi tạo. Dưới đây là 3 lớp con (class) thường được dùng nhất để tạo ra một hàng đợi:

### 🎟️ `LinkedList` (Hàng đợi tiêu chuẩn)

- **Đặc điểm:** Bất ngờ chưa? `LinkedList` vừa là `List`, lại vừa triển khai giao diện `Queue`. Khi bạn dùng nó với vai trò `Queue`, nó hoạt động như một hàng đợi FIFO chuẩn mực.
    
- **Khi nào dùng:** Khi bạn cần một hàng đợi cơ bản, xử lý các tác vụ theo đúng thứ tự thời gian chúng đến (ví dụ: hàng đợi in tài liệu của máy in, xử lý request của server).
    

### 👑 `PriorityQueue` (Hàng đợi ưu tiên)

- **Đặc điểm:** Phá vỡ quy tắc FIFO truyền thống! Các phần tử khi thêm vào sẽ tự động được sắp xếp theo **Mức độ ưu tiên**. Người đến sau nhưng có thẻ VIP vẫn có thể được đẩy lên đầu hàng.
    
- **Khi nào dùng:** Khi xử lý các bài toán cần ưu tiên (ví dụ: phòng cấp cứu bệnh viện ưu tiên bệnh nhân nặng, hệ thống điều phối xe cứu hỏa). Mặc định nó sắp xếp số từ bé đến lớn hoặc theo bảng chữ cái.
    

### ↔️ `ArrayDeque` (Hàng đợi hai đầu)

- **Đặc điểm:** Chữ "Deque" viết tắt của _Double Ended Queue_. Nó cho phép bạn thêm và lấy phần tử ở **cả 2 đầu** (đầu hàng và cuối hàng). Nó dùng mảng động bên dưới.
    
- **Khi nào dùng:** Khi bạn cần một cấu trúc linh hoạt vừa làm Queue (xếp hàng) vừa làm Stack (ngăn xếp - LIFO). Nó hoạt động nhanh hơn `LinkedList` rất nhiều khi làm Queue và nhanh hơn `Stack` cổ điển.
    

---

Bạn muốn xem một đoạn code ví dụ minh họa cách `PriorityQueue` đưa phần tử "VIP" lên đầu hàng, hay muốn chúng ta chuyển sang khám phá nhánh cuối cùng là giao diện `Set` (Tập hợp không trùng lặp)?