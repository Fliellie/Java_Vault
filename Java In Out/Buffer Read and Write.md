Dưới đây là 2 đoạn code cực kỳ chuẩn và trực quan để bạn thấy được sự khác biệt khi dùng bộ đệm cho **File Văn Bản** (Luồng Ký tự) và **File Nhị Phân** (Luồng Byte).

Cả hai đều được viết theo phong cách hiện đại (`try-with-resources`) để tự động đóng file an toàn tuyệt đối.

---

### 1. File Văn Bản: Đọc và Ghi bằng `BufferedReader` & `BufferedWriter`

Lợi ích lớn nhất của việc dùng bộ đệm với file văn bản là bạn không phải đọc/ghi từng chữ cái mệt mỏi. Bạn có thể bốc nguyên một dòng để xử lý rất nhàn.

Java

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class BufferTextExample {
    public static void main(String[] args) {
        String filename = "sample.txt";

        // --- BƯỚC 1: GHI FILE CÓ BỘ ĐỆM ---
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(filename))) {
            bw.write("Dòng 1: Chào bạn đến với Java I/O!");
            bw.newLine(); // Tự động xuống dòng chuẩn theo hệ điều hành
            bw.write("Dòng 2: Đây là ví dụ về BufferedReader và BufferedWriter.");
            
            System.out.println("Đã ghi file thành công!");
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("----------------------------------------");

        // --- BƯỚC 2: ĐỌC FILE CÓ BỘ ĐỆM ---
        try (BufferedReader br = new BufferedReader(new FileReader(filename))) {
            String line;
            System.out.println("Nội dung file đọc được:");
            
            // Đọc nguyên cả dòng cho đến khi hết file (trả về null)
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

### 2. File Nhị Phân: Đọc và Ghi bằng `BufferedInputStream` & `BufferedOutputStream`

Lớp này thường dùng để xử lý các file không phải là văn bản thuần túy (như ảnh `.png`, `.jpg`, file nhạc `.mp3`, file nén `.zip`...). Nó xử lý mớ byte thô rất nhanh.


```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class BufferBinaryExample {
    public static void main(String[] args) {
        String sourceFile = "image.png"; // File ảnh gốc (bạn cần có sẵn file này)
        String targetFile = "image_copy.png";

        System.out.println("Đang tiến hành copy file ảnh...");

        // Gom cả luồng đọc và luồng ghi vào trong try()
        try (
            BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sourceFile));
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(targetFile))
        ) {
            int data;
            
            // Đọc từng byte từ bộ đệm của RAM và ghi sang bộ đệm đích
            while ((data = bis.read()) != -1) {
                bos.write(data);
            }
            
            // Ép xả hết những byte cuối cùng còn kẹt trong RAM xuống ổ cứng
            bos.flush(); 
            
            System.out.println("Copy file ảnh thành công rực rỡ!");
        } catch (IOException e) {
            System.out.println("Lỗi: Không tìm thấy file gốc hoặc file đang bị khóa!");
            e.printStackTrace();
        }
    }
}
```

---

### 🔑 Điểm mấu chốt để bạn không bị nhầm lẫn:

- Muốn đọc file text dòng bằng dòng? $\rightarrow$ Gọi ngay **`readLine()`** của `BufferedReader`.
    
- Muốn copy file dữ liệu thô nhanh như chớp? $\rightarrow$ Bọc ngay **`BufferedInputStream`** và **`BufferedOutputStream`**.