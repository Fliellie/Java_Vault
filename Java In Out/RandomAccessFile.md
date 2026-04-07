Chào mừng bạn đến với "kẻ ngoại đạo" thú vị nhất trong hệ thống Nhập/Xuất (I/O) của Java: **`java.io.RandomAccessFile`**.

Nếu các luồng dữ liệu (Streams) mà chúng ta bàn nãy giờ hoạt động giống như một **băng cassette** (bạn phải nghe tuần tự từ đầu đến cuối, muốn nghe bài giữa phải tua mỏi tay), thì `RandomAccessFile` hoạt động giống như một chiếc **đĩa CD** hay **ổ cứng HDD**. Bạn có thể nhảy "bụp" đến bất kỳ vị trí nào trong tệp để đọc hoặc ghi dữ liệu ngay lập tức.

---

### 1. Đặc điểm "độc nhất vô nhị"

- **Vừa đọc vừa ghi:** Các lớp Stream thông thường chia rạch ròi: `InputStream` chỉ để đọc, `OutputStream` chỉ để ghi. Riêng `RandomAccessFile` "cân" được cả hai. Nó implements cả hai interface `DataInput` và `DataOutput`.
    
- **Không thuộc họ hàng Stream:** Dù nằm trong gói `java.io`, nó không kế thừa từ `InputStream` hay `OutputStream` mà kế thừa trực tiếp từ siêu lớp `Object`.
    
- **Thao tác theo byte/dữ liệu nguyên thủy:** Nó cung cấp các hàm y hệt như `DataInputStream` và `DataOutputStream` (như `writeInt()`, `readDouble()`, `readUTF()`, v.v.).
    

---

### 2. Linh hồn của RandomAccessFile: "Con trỏ tệp" (File Pointer)

Để nhảy cóc được, `RandomAccessFile` duy trì một thứ gọi là **Con trỏ tệp**. Bạn có thể tưởng tượng nó giống hệt như **dấu nháy chuột (cursor)** khi bạn soạn thảo văn bản trong Word.

- Khi bạn mới mở file, con trỏ nằm ở vị trí số `0` (đầu file).
    
- Mỗi khi bạn đọc hoặc ghi dữ liệu, con trỏ sẽ tự động tiến lên phía trước một khoảng tương ứng với số byte vừa xử lý (ví dụ ghi một số `int` thì con trỏ nhích lên 4 byte).
    
- **Quyền năng tối thượng:** Bạn có thể dùng code để "bốc" con trỏ này đặt vào bất kỳ vị trí byte nào bạn muốn.
    

---

### 3. Các "Phép thuật" (Phương thức) quan trọng nhất

|Phương thức|Chức năng|Ý nghĩa thực tế|
|---|---|---|
|**`seek(long position)`**|Di chuyển con trỏ tệp đến vị trí byte thứ `position`.|Đây là hàm làm nên tên tuổi của lớp này. Nó giúp bạn "nhảy cóc".|
|**`getFilePointer()`**|Trả về vị trí hiện tại của con trỏ (tính bằng byte từ đầu tệp).|Giúp bạn biết mình đang đứng ở đâu trong file.|
|**`length()`**|Trả về tổng kích thước của file (số byte).|Giúp bạn dễ dàng di chuyển con trỏ đến cuối file (để ghi nối thêm).|
|**`setLength(long newLength)`**|Cắt xén hoặc mở rộng kích thước file.|Dùng để xóa bớt dữ liệu ở đuôi file nếu cần.|

---

### 4. Chế độ mở file (Access Modes)

Khi khởi tạo `RandomAccessFile`, bạn bắt buộc phải truyền vào một chuỗi (String) để khai báo "chế độ" (mode) mà bạn muốn mở:

- **`"r"` (Read-only):** Chỉ đọc. Nếu cố tình gọi hàm ghi sẽ bị ném lỗi `IOException`.
    
- **`"rw"` (Read-Write):** Vừa đọc vừa ghi. Nếu file chưa tồn tại, Java sẽ tự động tạo file mới cho bạn.
    
- Ngoài ra còn có `"rws"` và `"rwd"` (dùng để yêu cầu hệ điều hành đồng bộ hóa dữ liệu xuống ổ cứng ngay lập tức, thường dùng trong các ứng dụng cần độ an toàn dữ liệu cực cao như database).
    

---

### 5. Ví dụ thực tế: Sửa điểm của sinh viên

Giả sử bạn có một file lưu điểm của sinh viên (mỗi điểm là một số `double` tốn 8 bytes). Bạn muốn sửa điểm của sinh viên thứ 3 (không quan tâm các sinh viên khác).

Java

```java

import java.io.RandomAccessFile;
import java.io.IOException;

public class TestRandomAccess {
    public static void main(String[] args) {
        String fileName = "diem_thi.dat";

        // Mở file ở chế độ vừa đọc vừa ghi ("rw")
        try (RandomAccessFile raf = new RandomAccessFile(fileName, "rw")) {
            
            // 1. Tạo dữ liệu giả: Ghi điểm cho 3 sinh viên
            raf.writeDouble(5.5); // SV 1 (Byte 0-7)
            raf.writeDouble(8.0); // SV 2 (Byte 8-15)
            raf.writeDouble(4.0); // SV 3 (Byte 16-23)
            
            System.out.println("Kích thước file: " + raf.length() + " bytes");

            // 2. Nhảy cóc đến vị trí của sinh viên thứ 3 để sửa điểm
            // Vì mỗi số double chiếm 8 byte, điểm SV 3 bắt đầu từ byte số 16 (2 * 8)
            raf.seek(16); 
            
            // Ghi đè điểm mới (từ 4.0 sửa thành 9.5)
            raf.writeDouble(9.5); 
            
            // 3. Nhảy ngược lại đầu file để đọc kiểm tra
            raf.seek(0);
            System.out.println("Điểm SV 1: " + raf.readDouble());
            System.out.println("Điểm SV 2: " + raf.readDouble());
            System.out.println("Điểm SV 3 (Đã sửa): " + raf.readDouble());

        } catch (IOException e) {
            System.err.println("Lỗi thao tác file: " + e.getMessage());
        }
    }
}
```

Nhờ `seek()`, bạn không cần phải tải toàn bộ file vào bộ nhớ (RAM) rồi lưu lại từ đầu như khi dùng Streams. Tính năng này cực kỳ hữu ích khi bạn phải làm việc với các file dữ liệu khổng lồ (vài chục GB) hoặc khi bạn đang tự tay viết một hệ quản trị cơ sở dữ liệu (DBMS) cơ bản.