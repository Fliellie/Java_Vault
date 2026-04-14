Để bạn dễ hình dung nhất về cách dùng `BufferedInputStream` và `BufferedOutputStream` (được gọi chung là **Buffer Stream**), chúng ta hãy cùng đặt ra một bài toán thực tế nhé!

> **Bài toán:** Bạn cần copy một file ảnh nặng (hoặc một bài hát) từ thư mục này sang thư mục khác trong máy tính.

Mình sẽ viết code sử dụng luồng Byte truyền thống (`FileInputStream`), nhưng được bọc thêm lớp đệm `Buffered` bên ngoài để tăng tốc độ xử lý lên gấp hàng chục lần!

---

### 📝 Code mẫu: Copy file sử dụng Buffer Stream

Java

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFileBangBuffer {
    public static void main(String[] args) {
        String fileNguon = "anh_dep.png"; // File ảnh gốc của bạn
        String fileDich = "anh_dep_backup.png"; // File ảnh mới sẽ tạo ra

        System.out.println("Đang tiến hành copy file...");
        long startTime = System.currentTimeMillis(); // Bấm giờ bắt đầu

        // 1. Sử dụng Try-with-resources để tự động đóng luồng
        try (
            // Bọc FileInputStream vào trong BufferedInputStream
            BufferedInputStream bis = new BufferedInputStream(new FileInputStream(fileNguon));
            // Bọc FileOutputStream vào trong BufferedOutputStream
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(fileDich))
        ) {
            int data;
            
            // 2. Vừa đọc vừa ghi
            // bis.read() sẽ bốc 1 cục lớn từ ổ cứng vào RAM, rồi nhả ra từng byte cho bạn xử lý
            while ((data = bis.read()) != -1) {
                bos.write(data);
            }
            
            // Đổ hết dữ liệu còn sót lại trong bộ đệm xuống file
            bos.flush(); 

            long endTime = System.currentTimeMillis(); // Bấm giờ kết thúc
            System.out.println("Copy thành công!");
            System.out.println("Thời gian thực hiện: " + (endTime - startTime) + " ms");

        } catch (IOException e) {
            System.out.println("Đã xảy ra lỗi trong quá trình copy file!");
            e.printStackTrace();
        }
    }
}
```

---

### 🔍 Giải thích cơ chế "Thần tốc" của Buffer Stream

Tại sao việc bọc thêm cái vỏ `Buffered` này lại giúp code chạy nhanh "vù vù" như vậy?

- **Nếu không dùng Buffer (`FileInputStream` đứng một mình):** Cứ mỗi lần vòng lặp `while` chạy lệnh `read()`, Java lại phải trực tiếp thò tay xuống ổ cứng vật lý để bốc $1\text{ byte}$ dữ liệu lên. Hành động "đọc lắt nhắt" này cực kỳ tốn thời gian vì tốc độ truy xuất của ổ cứng rất chậm so với RAM.
    
- **Khi dùng Buffer (`BufferedInputStream` bọc ngoài):** Ở lần `read()` đầu tiên, nó sẽ phóng tay bốc luôn một mảng dữ liệu cực lớn (mặc định là $8192\text{ bytes} \approx 8\text{ KB}$) từ ổ cứng nạp thẳng vào bộ nhớ đệm trên RAM. Những lần lặp tiếp theo, Java chỉ việc lấy dữ liệu cực nhanh từ RAM ra thôi, không thèm đụng vào ổ cứng nữa cho tới khi xài hết sạch $8\text{ KB}$ đó.
    

---

### ⚠️ Một lưu ý sống còn: Lệnh `bos.flush()`

Do cơ chế gom dữ liệu vào bộ đệm rồi mới xả ra một thể, nên đôi khi dữ liệu ở cuối file (nhỏ hơn $8\text{ KB}$) vẫn bị "kẹt" lại trong RAM mà chưa kịp ghi xuống file đích.

- Lệnh **`bos.flush()`** chính là hành động "ấn nút xả bồn cầu", ép luồng đệm phải đổ nốt tất cả những gì còn sót lại trong RAM xuống file trên ổ cứng.
    
- _Mẹo nhỏ:_ Thực ra khi bạn dùng cấu trúc `try-with-resources`, lúc luồng tự đóng (`close()`), nó cũng sẽ tự động gọi `flush()` hộ bạn rồi. Nhưng chủ động viết thêm vào vẫn là một thói quen rất chuẩn Pro!
    

Bạn thấy cách bọc thêm "lớp đệm" này của Java có giống với cách người ta gom hàng đầy một xe tải rồi mới chở đi, thay vì chở từng món nhỏ lắt nhắt không?

Nếu bạn muốn đo đạc sự chênh lệch tốc độ khủng khiếp này, bạn có muốn mình hướng dẫn viết một bài test nhỏ so sánh trực tiếp thời gian chạy khi **có** và **không có** Buffer không?