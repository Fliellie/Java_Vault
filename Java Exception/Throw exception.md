Chào bạn! Việc "ném" ngoại lệ (Throwing Exceptions) trong Java giống như việc bạn phát một tín hiệu cảnh báo: _"Này, có gì đó không đúng ở đây, tôi không xử lý được, ai gọi tôi thì lo mà giải quyết nhé!"_

Để làm chủ kỹ thuật này, bạn cần phân biệt rạch ròi giữa việc **tự tay ném** (`throw`) và việc **đăng ký ném** (`throws`).

---

## 1. Từ khóa `throw`: "Tự tay ném lựu đạn"

Bạn dùng `throw` khi chính logic code của bạn phát hiện ra một điều kiện không hợp lệ (ví dụ: tuổi âm, số dư không đủ).

- **Vị trí:** Nằm bên trong thân hàm (method body).
    
- **Đối tượng:** Theo sau là một **đối tượng** cụ thể của lớp ngoại lệ (thường dùng từ khóa `new`).
    

Java

```
public void rutTien(double soTien) {
    if (soTien > balance) {
        // Tự tay tạo và ném ra một ngoại lệ
        throw new ArithmeticException("Số dư không đủ để rút!");
    }
    balance -= soTien;
}
```

---

## 2. Từ khóa `throws`: "Lời cảnh báo trước"

Bạn dùng `throws` ở phần khai báo hàm để nói với thế giới rằng: _"Hàm này tiềm ẩn nguy hiểm, ai dùng nó thì phải chuẩn bị tâm lý (dùng try-catch) hoặc tiếp tục ném nó đi."_

- **Vị trí:** Nằm ở chữ ký của hàm (method signature).
    
- **Đối tượng:** Theo sau là **tên các lớp** ngoại lệ.
    

Java

```java
// Cảnh báo rằng hàm này có thể gây ra lỗi đọc file
public void moFile(String path) throws FileNotFoundException, IOException {
    FileReader fr = new FileReader(path);
    // ...
}
```

---

## 3. So sánh nhanh `throw` và `throws`

|**Đặc điểm**|**throw**|**throws**|
|---|---|---|
|**Mục đích**|Dùng để thực sự ném một ngoại lệ ra.|Dùng để khai báo rằng hàm có thể văng lỗi.|
|**Vị trí**|Bên trong hàm.|Sau tên hàm, trước dấu `{`.|
|**Số lượng**|Chỉ ném được **1** đối tượng ngoại lệ mỗi lần.|Có thể khai báo **nhiều** loại ngoại lệ (cách nhau bởi dấu phẩy).|
|**Cú pháp**|`throw new ExceptionInstance();`|`void method() throws Exc1, Exc2 {}`|

---

## 4. Tự tạo ngoại lệ (Custom Exception)

Đôi khi những lỗi có sẵn của Java (như `ArithmeticException`, `NullPointerException`) không diễn tả hết được ý nghĩa bài toán của bạn. Lúc này, bạn nên tự tạo class ngoại lệ riêng.

**Cách làm:** Tạo một class mới kế thừa từ `Exception` (Checked - bắt buộc xử lý) hoặc `RuntimeException` (Unchecked - không bắt buộc).


```java
// 1. Định nghĩa lớp ngoại lệ riêng
class AgeException extends Exception {
    public AgeException(String message) {
        super(message);
    }
}

// 2. Sử dụng
public class Main {
    static void checkAge(int age) throws AgeException {
        if (age < 18) {
            throw new AgeException("Bạn chưa đủ 18 tuổi để truy cập trang web này!");
        }
    }

    public static void main(String[] args) {
        try {
            checkAge(16);
        } catch (AgeException e) {
            System.out.println("Bị chặn: " + e.getMessage());
        }
    }
}
```

---

## 💡 Quy tắc "Vàng" khi ném ngoại lệ

1. **Đừng ném `Exception` chung chung:** Thay vì `throw new Exception()`, hãy dùng loại cụ thể nhất có thể (ví dụ: `IllegalArgumentException`). Điều này giúp người gọi hàm biết chính xác chuyện gì đã xảy ra.
    
2. **Đừng lạm dụng:** Chỉ dùng ngoại lệ cho những tình huống **bất thường**. Đừng dùng ngoại lệ để điều khiển luồng logic thay cho `if-else` (vì tạo ra một Exception tốn tài nguyên hệ thống hơn nhiều so với một lệnh `if`).
    
3. **Tận dụng `RuntimeException`:** Nếu lỗi đó do lập trình viên viết sai (như truyền tham số sai), hãy dùng `RuntimeException` để không làm bẩn code bằng quá nhiều khối `try-catch`.
    

Bạn đã bao giờ gặp trường hợp app bị "văng" do lỗi mà không biết nó nằm ở dòng nào chưa? Đó chính là lúc ta cần học cách đọc **Stack Trace** – "bản đồ" dẫn đến nơi xảy ra lỗi đấy!




---
![[Pasted image 20260401192452.png]]
đây chính là kỹ thuật **Rethrowing Exception (Ném lại ngoại lệ)**. Bạn có thể hình dung nó giống như việc một nhân viên cấp dưới gặp một rắc rối: anh ta ghi chú lại lỗi đó vào sổ nhật ký (log), xử lý tạm thời một chút, nhưng sau đó vẫn phải "báo cáo lên sếp" (ném lại lỗi) vì vấn đề đó vượt quá thẩm quyền hoặc sếp cần biết để có hướng xử lý khác.

Dưới đây là những điểm "đáng tiền" nhất về kỹ thuật này mà tấm hình của bạn đang đề cập:

---

## 1. Tại sao phải "mất công" bắt rồi lại ném?

Nhiều bạn thắc mắc: _"Nếu định ném tiếp thì ngay từ đầu đừng dùng `try-catch` nữa cho xong?"_. Thực tế, chúng ta ném lại lỗi khi muốn thực hiện 2 việc cùng lúc:

- **Xử lý tại chỗ (Local handling):** Ghi log lỗi, đóng kết nối database, hoặc hoàn tác (rollback) một giao dịch để dữ liệu không bị hỏng.
    
- **Thông báo cho cấp trên (Notify caller):** Để phương thức gọi nó biết rằng tác vụ đã thất bại và có phương án dự phòng (ví dụ: hiển thị thông báo lỗi lên màn hình cho người dùng).
    

---

## 2. Cách hoạt động của Call Stack khi ném lại lỗi

Khi bạn thực hiện `throw e;` trong khối `catch`, Java sẽ không dừng chương trình ngay tại đó. Thay vào đó, nó sẽ:

1. Dừng thực hiện các dòng code tiếp theo trong phương thức hiện tại.
    
2. Tìm kiếm phương thức đã gọi phương thức này trong **Call Stack** để xem có khối `try-catch` nào khác đang chờ đợi không.
    
3. Nếu không ai bắt, lỗi sẽ leo dần lên tận hàm `main()` và làm sập chương trình.
    

---

## 3. Nâng cao: Ném lại vs. Gói lại (Exception Wrapping)

Trong thực tế, thay vì ném chính xác đối tượng `e` vừa bắt được, người ta thường **gói (wrap)** nó vào một ngoại lệ mới mang tính chuyên môn hơn. Điều này gọi là **Chained Exception**.

```java
try {
    // Code đọc dữ liệu từ Database
} catch (SQLException e) {
    // Ghi log chi tiết kỹ thuật cho lập trình viên
    logger.log(e.toString());
    
    // Ném ra một lỗi mang ý nghĩa nghiệp vụ cho người dùng
    throw new DatabaseConnectionException("Không thể kết nối đến máy chủ dữ liệu!", e);
}
```

> **Tại sao làm vậy?** Bằng cách truyền `e` vào constructor của lỗi mới, bạn vẫn giữ được **"nguyên nhân gốc rễ" (Root Cause)** giúp việc debug sau này dễ dàng hơn nhiều.

---

## ⚠️ Lưu ý về mặt cú pháp

Nếu bạn ném lại một **Checked Exception** (những lỗi mà Java bắt buộc phải xử lý như `IOException`, `SQLException`), bạn **vẫn phải khai báo `throws`** ở tên phương thức đó, cho dù bạn đã có khối `catch` ở bên trong.



```java
public void readFile() throws IOException { // Vẫn phải khai báo throws ở đây
    try {
        // ...
    } catch (IOException e) {
        // làm gì đó...
        throw e; // Ném tiếp
    }
}
```

Bạn có muốn mình demo một ví dụ thực tế về việc "Rollback" dữ liệu khi gặp lỗi rồi mới ném tiếp không? Đây là tình huống cực kỳ hay gặp khi làm việc với Database đấy!