## Checked Exception trong Java: Sự Kỷ Luật Của Trình Biên Dịch

Trong Java, **Checked Exception (Ngoại lệ được kiểm tra)** là một cơ chế nhằm ép buộc lập trình viên phải đối mặt và xử lý các tình huống rủi ro có thể dự đoán trước (như mất kết nối mạng, file không tồn tại, sai định dạng dữ liệu) ngay từ lúc viết code.

### 1. Bản Chất Của Checked Exception

Đặc điểm lớn nhất của Checked Exception là nó bị **trình biên dịch (Compiler) kiểm tra gắt gao ngay lúc biên dịch (Compile-time)**.

Nếu bạn gọi một phương thức có khả năng ném ra một Checked Exception, trình biên dịch của Java (như Eclipse, IntelliJ) sẽ lập tức gạch chân màu đỏ báo lỗi. Code của bạn sẽ **không thể biên dịch và không thể chạy được** cho đến khi bạn giải quyết xong ngoại lệ đó.

Về mặt phả hệ, Checked Exception là những lớp kế thừa trực tiếp từ lớp `Exception`, nhưng **KHÔNG** kế thừa từ `RuntimeException`.

**Các Checked Exception phổ biến nhất:**

- `IOException`: Lỗi khi đọc/ghi file hoặc luồng dữ liệu.
    
- `FileNotFoundException`: Cố gắng mở một file không tồn tại (là con của IOException).
    
- `SQLException`: Lỗi khi thao tác với cơ sở dữ liệu.
    
- `ClassNotFoundException`: Không tìm thấy lớp được yêu cầu lúc runtime.
    
- `ParseException`: Lỗi khi phân tích cú pháp (ví dụ: chuyển chuỗi thành ngày tháng).
    

---

### 2. Quy Tắc "Bắt Hoặc Ném" (Catch or Declare Rule)

Khi đối mặt với một Checked Exception, Java bắt buộc bạn phải chọn một trong hai con đường sau:

#### Cách 1: Bắt và Xử lý ngay lập tức (Try-Catch)

Bạn chịu trách nhiệm xử lý lỗi ngay tại chỗ bằng khối `try-catch`. Nếu có lỗi xảy ra, chương trình sẽ không bị sập mà sẽ nhảy vào khối `catch` để thực hiện hành động khắc phục (như in ra thông báo, dùng giá trị mặc định).

Java

```
import java.io.File;
import java.io.FileReader;
import java.io.FileNotFoundException;

public class XuLyNgoaiLe {
    public static void docFile() {
        File file = new File("du_lieu.txt");
        
        try {
            // Lệnh này có rủi ro ném ra FileNotFoundException (Checked)
            FileReader fr = new FileReader(file); 
            System.out.println("Đã mở file thành công!");
        } catch (FileNotFoundException e) {
            // Xử lý lỗi tại đây
            System.out.println("Lỗi: Không tìm thấy file. Vui lòng kiểm tra lại đường dẫn.");
        }
    }
}
```

#### Cách 2: Ném ngoại lệ cho người khác lo (Throws)

Nếu bạn không muốn xử lý lỗi tại hàm hiện tại, bạn có thể dùng từ khóa `throws` trên chữ ký của hàm để "đùn đẩy" trách nhiệm này cho hàm nào gọi đến nó.

Java

```
import java.io.File;
import java.io.FileReader;
import java.io.FileNotFoundException;

public class NemNgoaiLe {
    
    // Thêm 'throws' vào chữ ký hàm: "Tôi không xử lý, ai gọi hàm này thì tự đi mà bắt lỗi!"
    public static void docFile() throws FileNotFoundException {
        File file = new File("du_lieu.txt");
        FileReader fr = new FileReader(file); // Compiler không báo lỗi đỏ ở đây nữa
    }

    public static void main(String[] args) {
        // Hàm main gọi docFile(), nên bây giờ hàm main PHẢI xử lý bằng try-catch
        try {
            docFile();
        } catch (FileNotFoundException e) {
            System.out.println("Main đã bắt được lỗi: File không tồn tại.");
        }
    }
}
```

---

### 3. Phân Biệt Nhanh: Checked vs Unchecked Exceptions

Để không bị nhầm lẫn, đây là bảng so sánh cốt lõi giữa hai loại ngoại lệ trong Java:

|**Tiêu chí**|**Checked Exception**|**Unchecked Exception (Runtime Exception)**|
|---|---|---|
|**Kế thừa từ**|`Exception`|`RuntimeException` (là con của Exception)|
|**Thời điểm phát hiện**|**Compile-time** (Lúc biên dịch - gõ code)|**Runtime** (Lúc chương trình đang chạy)|
|**Bắt buộc xử lý?**|**Có.** Phải dùng `try-catch` hoặc `throws`, nếu không sẽ không build được.|**Không.** Trình biên dịch không ép buộc, bạn có thể bỏ qua.|
|**Nguyên nhân thường gặp**|Do các yếu tố bên ngoài (File mất, mạng đứt, DB sập).|Do lỗi logic của lập trình viên (Chia cho 0, `NullPointerException`, truy cập mảng quá giới hạn).|
|**Ví dụ điển hình**|`IOException`, `SQLException`|`NullPointerException`, `ArithmeticException`, `IndexOutOfBoundsException`|

**Tóm lại:** Checked Exception giống như một người bảo vệ khó tính. Mặc dù đôi khi việc phải viết `try-catch` liên tục gây ra sự dài dòng và rườm rà cho code (boilerplate code), nhưng nó đảm bảo rằng ứng dụng của bạn đã chuẩn bị sẵn kịch bản đối phó với những rủi ro khách quan từ môi trường bên ngoài, giúp hệ thống không bị sập một cách đột ngột.