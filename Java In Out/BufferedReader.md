## 1. Tại sao lại cần "Buffer" (Bộ đệm)?

Từ khóa ăn tiền nhất ở đây chính là chữ **"Buffered"**.

Hãy tưởng tượng bạn đang chuyển gạch xây nhà (đọc dữ liệu từ ổ cứng).

- Nếu dùng `FileReader` thông thường, bạn sẽ nhặt **từng viên gạch một** chạy từ đống gạch vào nhà. Việc chạy đi chạy lại ổ cứng liên tục như vậy tốn cực kỳ nhiều thời gian và tài nguyên.
    
- Nếu dùng `BufferedReader`, bạn được trang bị một **chiếc xe rùa (bộ đệm - buffer)**. Bạn sẽ xúc đầy một xe gạch (thường là 8KB dữ liệu cùng lúc) mang vào nhà, rồi thong thả lấy từng viên ra dùng (đọc từ RAM). Khi nào dùng hết xe đó mới ra xúc xe khác.
    

Chính cơ chế đọc gom nhóm này giúp `BufferedReader` có tốc độ xử lý **nhanh hơn hàng nghìn lần** so với việc đọc trực tiếp!

---

## 2. "Vũ khí tối thượng": Phương thức `readLine()`

Lý do thứ hai khiến mọi người yêu thích `BufferedReader` chính là phương thức **`readLine()`**.

Thay vì bắt bạn phải tự ghép từng ký tự lại với nhau rồi đau đầu tự dò xem đâu là ký tự xuống dòng (`\n` hay `\r`), `readLine()` làm sẵn tất cả cho bạn:

- Nó quét qua bộ đệm và gom toàn bộ ký tự lại thành một chuỗi `String` hoàn chỉnh cho đến khi gặp dấu xuống dòng.
    
- Khi file đã được đọc cạn kiệt, nó sẽ trả về giá trị **`null`** (đây là dấu hiệu hoàn hảo để kết thúc vòng lặp).
    

---

## 3. Code thực chiến: Đọc file văn bản siêu tốc

Dưới đây là form code chuẩn mực và hiện đại nhất (sử dụng `try-with-resources` của Java 7+) để bạn đọc mọi file `.txt`, `.csv`, hay `.log` một cách an toàn:

Java

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderExample {
    public static void main(String[] args) {
        // Đường dẫn tới file cần đọc
        String filePath = "data.txt";

        // Khởi tạo BufferedReader bọc lấy FileReader bên trong khối try
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            
            String line;
            int lineNumber = 1;
            
            // Vòng lặp đọc từng dòng cho đến khi hết file (trả về null)
            while ((line = br.readLine()) != null) {
                System.out.println("Dòng " + lineNumber + ": " + line);
                lineNumber++;
            }
            
            System.out.println("--- Đã đọc xong toàn bộ file! ---");
            
        } catch (IOException e) {
            System.out.println("Lỗi cmnr! Không thể đọc file: " + e.getMessage());
        }
    }
}
```

---

## 4. Tóm lại: Khi nào nên dùng?

- **DÙNG `BufferedReader`:** Khi bạn cần đọc file text dung lượng lớn (từ vài MB đến vài GB), đọc file log của server, hoặc đọc dữ liệu dạng bảng (CSV) theo từng dòng để xử lý cực nhanh.
    
- **KHÔNG DÙNG:** Khi bạn cần đọc file hình ảnh, video (hãy dùng `BufferedInputStream`), hoặc khi bạn cần phân tích chuỗi nhập vào từ bàn phím có ép kiểu nhanh (lúc này `Scanner` lại phù hợp hơn).
    

Bạn đã nắm được bí kíp đọc file siêu tốc rồi, bạn có muốn chúng ta tìm hiểu luôn "người anh em song sinh" của nó là **`BufferedWriter`** để hoàn thiện bộ kỹ năng Đọc - Ghi văn bản ở tốc độ bàn thờ không?