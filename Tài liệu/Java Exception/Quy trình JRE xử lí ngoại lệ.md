### Quy trình 5 bước của JRE khi xử lý ngoại lệ

#### Bước 1: Tạo đối tượng ngoại lệ (Creating an Exception Object)

Ngay khi một lỗi xảy ra (ví dụ: chia cho 0), phương thức đang thực hiện lệnh đó sẽ lập tức dừng lại. Nó tạo ra một **đối tượng ngoại lệ (Exception Object)**. Đối tượng này chứa:

- **Loại lỗi:** (Ví dụ: `ArithmeticException`).
    
- **Trạng thái chương trình:** Các giá trị biến lúc đó.
    
- **Vị trí lỗi:** Dòng code nào, file nào (Stack Trace).
    

#### Bước 2: Ném ngoại lệ (Throwing the Exception)

Sau khi tạo xong đối tượng, phương thức đó "vứt" đối tượng này cho JRE. Hành động này gọi là **Throwing an Exception**. Từ lúc này, luồng thực thi bình thường của chương trình bị ngắt quãng hoàn toàn.

#### Bước 3: Truy vết ngăn xếp (Searching the Call Stack)

JRE bắt đầu cầm đối tượng lỗi đó đi tìm "người chịu trách nhiệm". Nó nhìn vào **Call Stack** (Danh sách các phương thức đang gọi nhau theo thứ tự).

- Đầu tiên, nó kiểm tra ngay tại phương thức vừa xảy ra lỗi xem có khối `try-catch` nào bao quanh không.
    
- Nếu **không có**, nó "nhảy ngược" về phương thức đã gọi phương thức lỗi đó để tìm tiếp.
    
- Quá trình này lặp lại liên tục cho đến khi tìm thấy một khối `catch` phù hợp với loại lỗi đang cầm trên tay.
    

#### Bước 4: Bắt ngoại lệ (Catching the Exception)

Nếu JRE tìm thấy một khối `catch` phù hợp:

- Nó sẽ chuyển đối tượng ngoại lệ vào cho khối `catch` đó.
    
- Chương trình sẽ chạy các lệnh bên trong `catch` (xử lý lỗi).
    
- Sau khi chạy xong `catch` (và `finally` nếu có), chương trình **tiếp tục chạy** các dòng code bên dưới khối đó thay vì bị sập.
    

#### Bước 5: Trình xử lý mặc định (Default Exception Handler)

Nếu JRE đã tìm ngược lên tận phương thức `main()` mà vẫn **không thấy** bất kỳ khối `catch` nào xử lý lỗi này:

- JRE sẽ chuyển đối tượng lỗi cho **Default Exception Handler** (Bộ xử lý mặc định của hệ thống).
    
- Bộ xử lý này sẽ làm 2 việc:
    
    1. In toàn bộ **Stack Trace** (dấu vết lỗi) ra màn hình console để bạn biết đường mà sửa.
        
    2. **Chấm dứt (Terminate)** luồng (thread) đang chạy. Nếu đó là luồng chính, chương trình của bạn chính thức bị "sập".
        

---

### Ví dụ về "Sự đùn đẩy" trách nhiệm (Call Stack)

Hãy xem đoạn code sau để thấy JRE đi tìm lỗi như thế nào:


```java
public class QuyTrinhJRE {
    public static void main(String[] args) {
        methodA(); // 3. Main cũng không bắt -> JRE sập chương trình
    }

    static void methodA() {
        methodB(); // 2. MethodA không bắt lỗi của MethodB
    }

    static void methodB() {
        int result = 10 / 0; // 1. Lỗi xảy ra ở đây! JRE bắt đầu tìm từ đây.
    }
}
```

**Quy trình tìm kiếm của JRE:**

1. Lỗi ở `methodB` $\rightarrow$ Không thấy `catch` $\rightarrow$ Nhảy về `methodA`.
    
2. Ở `methodA` $\rightarrow$ Không thấy `catch` $\rightarrow$ Nhảy về `main`.
    
3. Ở `main` $\rightarrow$ Không thấy `catch` $\rightarrow$ Giao cho Default Handler $\rightarrow$ **In lỗi và Thoát**.
    

---

### Tóm lại

Quy trình của JRE là một nỗ lực **"tìm kiếm người đổ vỏ"**. Nếu bạn (lập trình viên) chủ động "đổ vỏ" bằng `try-catch`, chương trình sẽ an toàn. Nếu bạn im lặng, JRE sẽ tìm đến tận cùng và đành phải đóng chương trình để đảm bảo hệ thống không bị sai lệch dữ liệu thêm nữa.

