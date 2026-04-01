
Thưa bạn, nếu `Object` là "Ông Tổ" của mọi lớp trong Java, thì **`java.lang.Throwable`** chính là "Cụ Tổ" của mọi rắc rối.

Đây là lớp cao nhất trong sơ đồ phả hệ của tất cả các lỗi và ngoại lệ. Bất kỳ thứ gì bạn có thể "ném" ra bằng từ khóa `throw` hoặc "bắt" lấy bằng khối `catch` thì bắt buộc phải là con cháu của `Throwable`.

---

### 1. Hai "nhánh" con của `Throwable`

Dưới trướng của `Throwable` có hai quân đoàn hoàn toàn khác biệt về tính chất:

#### **A. Lớp `Error` (Lỗi hệ thống)**

- **Đặc điểm:** Đây là những vấn đề cực kỳ nghiêm trọng liên quan đến môi trường thực thi (JVM) hoặc phần cứng.
    
- **Thái độ:** "Vô phương cứu chữa". Lập trình viên thường không nên cố gắng `catch` các `Error` này vì nếu chúng xảy ra, chương trình của bạn thường đã rơi vào trạng thái không thể hồi phục.
    
- **Ví dụ:** `OutOfMemoryError` (Hết RAM), `StackOverflowError` (Tràn ngăn xếp do đệ quy vô tận).
    

#### **B. Lớp `Exception` (Ngoại lệ)**

- **Đặc điểm:** Đây là những tình huống sai sót có thể dự đoán hoặc xảy ra do logic người dùng, dữ liệu đầu vào.
    
- **Thái độ:** "Có thể khắc phục". Bạn bắt buộc (hoặc nên) sử dụng `try-catch` để xử lý chúng.
    
- **Ví dụ:** `NullPointerException`, `IOException`, `SQLException`.
    

---

### 2. Các phương thức "gia bảo" của `Throwable`

Vì mọi `Exception` và `Error` đều kế thừa từ `Throwable`, chúng đều sở hữu những phương thức cực kỳ hữu ích để chúng ta "vạch mặt" kẻ gây lỗi:

|Phương thức|Ý nghĩa thực tế|
|---|---|
|**`getMessage()`**|Trả về một chuỗi mô tả ngắn gọn về lỗi (Ví dụ: "/ by zero").|
|**`getLocalizedMessage()`**|Tương tự `getMessage`, nhưng có thể được ghi đè để hỗ trợ đa ngôn ngữ.|
|**`getCause()`**|Trả về nguyên nhân gốc rễ (nếu có) dẫn đến ngoại lệ này (thường dùng trong lồng ngoại lệ).|
|**`printStackTrace()`**|In toàn bộ dấu vết từ lúc lỗi xảy ra lùi về hàm `main` ra màn hình (Hộp đen của chương trình).|
|**`getStackTrace()`**|Trả về một mảng các đối tượng `StackTraceElement`, giúp bạn truy cập thông tin lỗi bằng code thay vì chỉ in ra.|
|**`toString()`**|Trả về tên đầy đủ của lớp ngoại lệ kèm theo thông điệp từ `getMessage()`.|

---

### 3. Ví dụ minh họa sức mạnh của `Throwable`

Bạn có thể dùng `Throwable` làm "tấm lưới" lọc cuối cùng nếu bạn không chắc chắn lỗi gì sẽ xảy ra (tuy nhiên, trong thực tế, bắt cụ thể từng loại lỗi vẫn tốt hơn).

Java

```
public class TestThrowable {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // Gây lỗi ArithmeticException
        } catch (Throwable t) { 
            // Dùng Throwable ở đây có thể bắt được CẢ Exception và Error
            System.out.println("Tên lỗi: " + t.getClass().getName());
            System.out.println("Thông điệp: " + t.getMessage());
            
            System.out.println("\nChi tiết dấu vết:");
            t.printStackTrace();
        }
    }
}
```

### 4. Một lưu ý nhỏ về kiến trúc

Dù bạn có thể viết `catch (Throwable t)`, nhưng các chuyên gia Java luôn khuyên: **Đừng làm thế trừ khi bạn có lý do cực kỳ đặc biệt.** Lý do là vì `Throwable` bao gồm cả `Error`. Nếu bạn bắt cả `Error`, bạn vô tình che giấu những lỗi hệ thống nghiêm trọng mà lẽ ra chương trình nên dừng lại để bảo vệ toàn vẹn dữ liệu. Hãy ưu tiên bắt `Exception` hoặc các lớp con cụ thể của nó.

Bạn có muốn tìm hiểu về cách tự tạo một lớp ngoại lệ mang tên chính mình bằng cách kế thừa `Throwable` không?