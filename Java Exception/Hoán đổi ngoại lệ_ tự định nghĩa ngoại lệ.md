## 1. Hoán đổi ngoại lệ (Checked $\rightarrow$ Unchecked)

Đây là kỹ thuật "đóng gói" một ngoại lệ khó tính (Checked) vào trong một cái bao dễ tính hơn (Unchecked).

### Tại sao phải làm vậy?

- **Khi bạn chưa biết xử lý thế nào:** Bạn đang ở một hàm trung gian, lỗi xảy ra nhưng bạn không muốn ép các hàm gọi phía trên phải viết `try-catch` liên tục (vì Checked Exception bắt buộc phải khai báo `throws`).
    
- **Tuân thủ chữ ký hàm (Method Signature):** Khi bạn ghi đè một phương thức từ Interface hoặc lớp cha mà phương thức gốc không khai báo `throws`, bạn không thể ném Checked Exception ra ngoài. Cách duy nhất là biến nó thành Unchecked.
    

### Cách thực hiện (Wrapping):

Bạn bắt ngoại lệ gốc, sau đó ném ra một `RuntimeException` và truyền ngoại lệ gốc vào làm **nguyên nhân (cause)**.



```java
void wrapException() {
    try {
        // Một đoạn code gây ra IOException (Checked)
        throw new IOException("Lỗi ổ đĩa");
    } catch (IOException e) {
        // "Bọc" nó lại và ném ra dưới dạng Unchecked
        throw new RuntimeException(e); 
    }
}
```

### Cách lấy lại lỗi gốc (Unwrapping):

Sử dụng phương thức `.getCause()` để truy tìm "thủ phạm" ban đầu nằm bên trong cái bọc.

---

## 2. Tự định nghĩa ngoại lệ (Custom Exception)

Đôi khi những ngoại lệ có sẵn của Java như `ArithmeticException` hay `NullPointerException` không đủ để mô tả lỗi trong nghiệp vụ của bạn (ví dụ: lỗi `SoDuKhongDuException` khi rút tiền).

### Quy tắc tạo lớp ngoại lệ:

- **Kế thừa:** Phải kế thừa từ lớp `Exception` (nếu muốn tạo Checked Exception) hoặc `RuntimeException` (nếu muốn tạo Unchecked Exception).
    
- **Constructor:** Thông thường chúng ta cung cấp 2 hàm khởi tạo:
    
    1. **Mặc định:** Không tham số.
        
    2. **Có tham số:** Nhận một `String message` để mô tả lỗi và dùng `super(msg)` để gửi nó lên lớp cha.
        

### Ví dụ minh họa:


```java
// 1. Định nghĩa lớp ngoại lệ riêng
class TuoiKhongHopLeException extends Exception {
    public TuoiKhongHopLeException() {
        super();
    }

    public TuoiKhongHopLeException(String msg) {
        super(msg); // Truyền thông báo lỗi vào lớp Exception cha
    }
}

// 2. Sử dụng trong chương trình
public class TestCustomException {
    static void kiemTraTuoi(int tuoi) throws TuoiKhongHopLeException {
        if (tuoi < 18) {
            throw new TuoiKhongHopLeException("Bạn chưa đủ 18 tuổi để vào rạp!");
        }
    }

    public static void main(String[] args) {
        try {
            kiemTraTuoi(16);
        } catch (TuoiKhongHopLeException e) {
            System.out.println("Bắt được lỗi: " + e.getMessage());
        }
    }
}
```

---

### Tổng kết từ slide:

- **Hoán đổi:** Giúp code sạch hơn, tránh việc lạm dụng `throws` ở khắp nơi khi chưa thực sự cần xử lý lỗi.
    
- **Tự định nghĩa:** Giúp code mang tính "nghiệp vụ" hơn, dễ đọc, dễ hiểu và dễ bảo trì hơn cho các dự án thực tế.
    

Bạn có muốn mình hướng dẫn cách tạo một Custom Exception mà có thể chứa thêm các thông tin bổ sung (như `errorCode` hay `timestamp`) không?