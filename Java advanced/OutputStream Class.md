Trong Java, `OutputStream` là một **lớp trừu tượng (abstract class)**, đóng vai trò là "cha đẻ" của tất cả các lớp đại diện cho luồng xuất dữ liệu dưới dạng **byte** (8-bit). Nếu bạn muốn ghi hình ảnh, âm thanh, hoặc văn bản vào file, `OutputStream` chính là công cụ bạn cần.

---

## 1. Bản chất của OutputStream

`OutputStream` hoạt động theo cơ chế **ghi tuần tự**. Bạn đẩy dữ liệu vào luồng, và luồng đó sẽ dẫn dữ liệu tới đích (như file, mảng byte, hoặc kết nối mạng).

**Đặc điểm quan trọng:**

- **Hướng Byte (Byte-oriented):** Nó xử lý dữ liệu theo từng byte đơn lẻ. Đối với văn bản thuần túy (Unicode/Char), người ta thường dùng `Writer`, nhưng với dữ liệu thô (binary), `OutputStream` là vua.
    
- **Abstract Class:** Bạn không thể khởi tạo trực tiếp `new OutputStream()`, mà phải dùng các lớp con thực thi nó.
    

---

## 2. Các phương thức "xương sống" của OutputStream

Dù bạn đang ghi vào đâu, bạn sẽ luôn xoay quanh các phương thức sau:

|**Phương thức**|**Chức năng**|
|---|---|
|`write(int b)`|Ghi **một byte** duy nhất vào luồng xuất.|
|`write(byte[] b)`|Ghi **toàn bộ mảng byte** vào luồng. (Nhanh hơn ghi từng byte).|
|`flush()`|**Đẩy dữ liệu đi ngay lập tức**. Đôi khi dữ liệu còn kẹt trong bộ đệm (buffer), `flush()` sẽ ép nó phải xuất ra đích.|
|`close()`|**Đóng luồng** và giải phóng tài nguyên hệ thống. Đây là việc cực kỳ quan trọng để tránh rò rỉ bộ nhớ.|

---

## 3. Các lớp con (Implementations) phổ biến nhất

Để thực hiện công việc cụ thể, bạn cần chọn đúng lớp con:

### 📁 `FileOutputStream`

Dùng để ghi dữ liệu trực tiếp vào một tệp tin (`.txt`, `.png`, `.exe`,...).



```java
OutputStream os = new FileOutputStream("output.txt");
```

### ⚡ `BufferedOutputStream`

Lớp này không ghi trực tiếp mà dùng một **bộ đệm (buffer)** bên trong. Thay vì ghi từng byte vào đĩa (rất chậm), nó gom lại một mớ rồi ghi một thể.

> **Mẹo:** Luôn bọc `FileOutputStream` vào trong `BufferedOutputStream` để tăng hiệu năng lên gấp nhiều lần.

### 📦 `ObjectOutputStream`

Dùng để ghi các **đối tượng Java** (Object) vào luồng (thường gọi là Serialization). Bạn có thể lưu cả một Object xuống file và sau đó đọc lại nguyên vẹn.

---

## 4. Code thực chiến: Ghi chuỗi vào File

Vì `OutputStream` chỉ làm việc với byte, nên khi muốn ghi một chuỗi (`String`), bạn phải đổi nó sang mảng byte bằng phương thức `.getBytes()`.



```java
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.io.IOException;

public class OutputStreamExample {
    public static void main(String[] args) {
        String data = "Chào mừng bạn đến với Java I/O!";
        
        // Sử dụng try-with-resources để tự động đóng luồng
        try (OutputStream os = new FileOutputStream("demo.txt")) {
            
            // Chuyển chuỗi thành mảng byte
            byte[] bytes = data.getBytes();
            
            // Ghi dữ liệu
            os.write(bytes);
            
            System.out.println("Ghi file thành công!");
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 💡 Lưu ý "sống còn":

1. **Ghi đè vs Ghi nối tiếp:** * `new FileOutputStream("file.txt")`: Sẽ xóa sạch nội dung cũ của file để ghi mới.
    
    - `new FileOutputStream("file.txt", true)`: Sẽ ghi tiếp vào cuối file (Append mode).
        
2. **Đừng bao giờ quên `close()`:** Nếu không dùng `try-with-resources`, hãy luôn đặt `os.close()` trong khối `finally`. Nếu không, file đó có thể bị khóa và các chương trình khác không thể truy cập được.
    

Bạn có muốn tìm hiểu nốt về **`InputStream`** để hoàn tất quy trình "Đọc - Ghi" file, hay muốn thử thách với việc ghi một Object phức tạp xuống đĩa bằng **`ObjectOutputStream`**?