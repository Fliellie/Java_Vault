## Lớp `InputStream` trong Java: Cánh Cửa Đón Dữ Liệu Thô (Byte)

Tiếp nối khái niệm về luồng byte, **`java.util.InputStream`** chính là "Lớp cha tối cao" (Abstract Class) của tất cả các lớp làm nhiệm vụ **đọc dữ liệu dạng byte** vào chương trình Java của bạn.

Bạn có thể tưởng tượng `InputStream` giống như một ống nước được cắm vào một nguồn chứa (như một file trên ổ cứng, một máy chủ trên mạng, hoặc một vùng nhớ). Khi bạn mở van, dữ liệu sẽ chảy qua ống nước này dưới dạng từng giọt (từng byte) để đi vào chương trình.

---

### 1. Đặc Điểm Cốt Lõi Của `InputStream`

- **Tính Trừu Tượng (Abstract):** Vì là một lớp abstract, bạn **không thể** khởi tạo trực tiếp nó bằng từ khóa `new InputStream()`. Thay vào đó, bạn phải sử dụng các lớp con cụ thể của nó (như `FileInputStream` để đọc file).
    
- **Xử Lý Tuần Tự:** Dữ liệu chảy qua `InputStream` theo một chiều duy nhất và tuần tự từ đầu đến cuối. Bạn đọc byte số 1, rồi đến byte số 2... Bạn hiếm khi có thể quay ngược lại để đọc (trừ khi dùng các luồng có hỗ trợ bộ đệm và đánh dấu).
    
- **Làm Việc Với Máy:** Nó đọc mọi thứ dưới dạng các con số nhị phân (từ 0 đến 255). Nó không quan tâm nội dung đó là văn bản, hình ảnh `.jpg` hay video `.mp4`.
    

---

### 2. Các "Vũ Khí" (Phương thức) Quan Trọng Nhất

Dù bạn dùng lớp con nào của `InputStream`, bạn đều sẽ sử dụng chung một bộ phương thức tiêu chuẩn sau:

|Phương thức|Chức năng|Cần lưu ý|
|---|---|---|
|**`read()`**|Đọc **1 byte** tiếp theo từ nguồn dữ liệu. Trả về một số nguyên `int` (0-255).|Trả về **`-1`** nếu đã đọc đến cuối luồng (End Of File - EOF). Rất quan trọng để viết vòng lặp!|
|**`read(byte[] b)`**|Đọc một lượng lớn byte và nhét vào mảng `b` đã chuẩn bị sẵn. Trả về số lượng byte _thực tế_ đã đọc được.|Giúp tăng tốc độ đọc lên hàng nghìn lần so với việc dùng `read()` để đọc từng byte một.|
|**`close()`**|Đóng luồng và giải phóng mọi tài nguyên hệ thống (như file handle hay socket) liên kết với nó.|**Bắt buộc phải gọi!** Nếu quên, file có thể bị khóa hoặc bộ nhớ bị rò rỉ.|
|**`available()`**|Trả về số lượng ước tính các byte còn lại có thể đọc được ngay mà không bị chặn (block).|Ít dùng trong thực tế với file, thường dùng nhiều hơn với luồng mạng (Network stream).|
|**`skip(long n)`**|Bỏ qua (không đọc) `n` byte tiếp theo trong luồng.|Tiện dụng khi bạn muốn nhảy cóc đến một vị trí dữ liệu cụ thể.|

---

### 3. Các Lớp Con Phổ Biến Nhất Của `InputStream`

Tùy thuộc vào "nguồn" dữ liệu của bạn nằm ở đâu, bạn sẽ chọn "đầu cắm" tương ứng:

- **`FileInputStream`:** Dùng khi nguồn dữ liệu là một **File** trên ổ cứng.
    
- **`ByteArrayInputStream`:** Dùng khi nguồn dữ liệu là một **Mảng byte** đang nằm sẵn trong RAM (Memory).
    
- **`BufferedInputStream`:** Đây là một "lớp bọc" (Wrapper). Nó gắn thêm một **Bộ đệm (Buffer)** vào một `InputStream` khác để giảm thiểu số lần tương tác trực tiếp với ổ cứng/mạng, giúp tốc độ đọc nhanh hơn đáng kể.
    
- **`ObjectInputStream`:** Dùng để đọc các **Đối tượng Java (Objects)** đã được mã hóa (Serialization) từ trước.
    

---

### 4. Code Mẫu Thực Tế: Đọc Dữ Liệu Bằng `FileInputStream`

Dưới đây là kịch bản kinh điển nhất: Đọc một file bằng `FileInputStream` kết hợp với cú pháp `try-with-resources` để tự động đóng luồng mà không cần dùng `finally`.



```java
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.IOException;

public class DocFileByte {
    public static void main(String[] args) {
        
        // Sử dụng try-with-resources để InputStream (is) tự động close() khi xong việc
        try (InputStream is = new FileInputStream("du_lieu_cua_toi.txt")) {
            
            int duLieu;
            // Vòng lặp: Cứ đọc 1 byte gán vào biến duLieu. 
            // Nếu kết quả khác -1 (nghĩa là chưa hết file) thì tiếp tục chạy
            while ((duLieu = is.read()) != -1) {
                
                // Vì read() trả về số int (mã ASCII), ta ép kiểu (char) để in ra màn hình
                // (Lưu ý: Cách ép kiểu này chỉ hoạt động tốt với tiếng Anh không dấu)
                System.out.print((char) duLieu);
            }
            
        } catch (IOException e) {
            System.err.println("Đã xảy ra lỗi khi đọc file: " + e.getMessage());
        }
    }
}
```

**🚨 Lời Khuyên Của Chuyên Gia (Best Practice):** Việc dùng vòng lặp `is.read()` để đọc _từng byte một_ như ví dụ trên chỉ để minh họa cơ chế hoạt động. Trong các dự án thực tế, người ta **luôn luôn** dùng `is.read(byte[] b)` hoặc bọc nó trong một `BufferedInputStream` để đọc từng cục dữ liệu lớn (ví dụ: 4KB hoặc 8KB mỗi lần). Việc gọi hệ điều hành để xin đọc từng byte một sẽ làm chương trình của bạn chạy chậm như rùa bò.




Trong Java, `OutputStream` là một **lớp trừu tượng (abstract class)**, đóng vai trò là "cha đẻ" của tất cả các lớp đại diện cho luồng xuất dữ liệu dưới dạng **byte** (8-bit). Nếu bạn muốn ghi hình ảnh, âm thanh, hoặc văn bản vào file, `OutputStream` chính là công cụ bạn cần.

---

## 1.  OutputStream

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