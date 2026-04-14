Trong Java, mọi rủi ro đều bắt nguồn từ một "cụ tổ" mang tên lớp **`Throwable`**. Từ đây, nó chia làm hai thái cực hoàn toàn khác nhau: **`Error`** và **`Exception`**.

---

## 1. Lớp `Error` (Lỗi hệ thống) là gì?

Nếu `Exception` là những rắc rối do code của bạn viết sai, thì **`Error`** là những thảm họa cấp hệ thống, liên quan trực tiếp đến môi trường chạy Java (JVM - Java Virtual Machine) hoặc phần cứng.

**Đặc điểm cốt lõi của Error:**

- **Không thể cứu vãn (Unrecoverable):** Khi `Error` xảy ra, chương trình của bạn chắc chắn sẽ "bay màu" (crash). Bạn **KHÔNG NÊN** và **KHÔNG THỂ** dùng khối `try-catch` để bắt và xử lý `Error`.
    
- **Nằm ngoài tầm kiểm soát:** Nó thường xảy ra do thiếu tài nguyên hệ thống (hết RAM, tràn bộ nhớ), những thứ mà một lập trình viên bình thường không thể can thiệp bằng code.
    
- **Thuộc loại Unchecked:** Trình biên dịch (Compiler) sẽ không ép bạn phải xử lý chúng lúc viết code.
    

---

## 2. Các `Error` "khét tiếng" nhất thường gặp

Dù không thể xử lý, nhưng bạn chắc chắn sẽ gặp những `Error` này trong đời lập trình. Nhận diện được chúng sẽ giúp bạn tìm ra nguyên nhân cực nhanh:

|**Tên Error**|**Nguyên nhân gây ra**|**Cách khắc phục**|
|---|---|---|
|`OutOfMemoryError`|Chương trình ngốn quá nhiều RAM, JVM không còn vùng nhớ (Heap Space) để cấp phát cho các đối tượng (Object) mới.|Tìm xem có chỗ nào tạo đối tượng liên tục (như trong vòng lặp vô hạn) mà không giải phóng, hoặc tăng dung lượng RAM cho JVM.|
|`StackOverflowError`|Xảy ra khi bộ nhớ Stack bị tràn. Nguyên nhân kinh điển nhất là do viết hàm **đệ quy vô hạn** (hàm gọi lại chính nó mãi mãi mà không có điểm dừng).|Kiểm tra lại điều kiện dừng (base case) của các hàm đệ quy.|
|`VirtualMachineError`|Lỗi nội tại của JVM bị hỏng hoặc thiếu tài nguyên để tiếp tục hoạt động.|Thường phải khởi động lại hệ thống hoặc cấu hình lại server.|

---

## 3. Bảng phân biệt "Sinh tử": Error vs Exception

Đây là một trong những câu hỏi lý thuyết thường xuyên xuất hiện nhất khi phỏng vấn vị trí Java Developer:

|**Tiêu chí**|**Error**|**Exception**|
|---|---|---|
|**Bản chất**|Là thảm họa hệ thống (Môi trường, JVM).|Là lỗi logic chương trình (Do lập trình viên hoặc dữ liệu đầu vào).|
|**Khả năng phục hồi**|**Không thể phục hồi.** App chắc chắn sập.|**Có thể phục hồi.** App vẫn chạy tiếp nếu bắt được lỗi.|
|**Cách xử lý**|Sửa lại logic tận gốc hoặc nâng cấp hệ thống. KHÔNG dùng `try-catch`.|Xử lý triệt để bằng khối `try-catch-finally` hoặc từ khóa `throws`.|
|**Ví dụ tiêu biểu**|`OutOfMemoryError`, `StackOverflowError`|`NullPointerException`, `IOException`, `ArithmeticException` (chia cho 0)|

---

Tóm lại, với `Error`, chúng ta chỉ có thể phòng tránh bằng cách viết code tối ưu và cấp đủ tài nguyên cho máy chủ, chứ không thể "chữa" bằng code khi nó đã xảy ra.

Vì chủ đề "lỗi" khá rộng, bạn muốn chúng ta đi sâu vào khám phá người anh em **`Exception`** và cách sử dụng bộ công cụ quyền lực **`try - catch - finally`** để "bảo kê" cho chương trình không bị sập giữa chừng chứ?