## 1. `FileWriter` (Dùng để GHI file)

`FileWriter` giúp bạn mở một file và đẩy các ký tự hoặc chuỗi (String) vào file đó.

### 📝 Code mẫu chuẩn hiện đại (Dùng try-with-resources để tự đóng file):

Java

```java
import java.io.FileWriter;
import java.io.IOException;

public class CachDungFileWriter {
    public static void main(String[] args) {
        String data = "Xin chào các bạn!\nMình đang học Java I/O.\nFileReader và FileWriter rất dễ dùng.";

        // Khai báo FileWriter trong try() để Java tự động đóng file
        try (FileWriter writer = new FileWriter("output.txt")) {
            
            // Cách 1: Ghi cả một chuỗi String
            writer.write(data);
            
            // Cách 2: Ghi từng ký tự
            writer.write('\n');
            writer.write('A');
            
            System.out.println("Đã ghi file thành công!");
            
        } catch (IOException e) {
            System.out.println("Đã xảy ra lỗi khi ghi file!");
            e.printStackTrace();
        }
    }
}
```

> 💡 **Mẹo cực hay:** Mặc định, `new FileWriter("file.txt")` sẽ **xóa sạch ruột cũ** của file để ghi đè từ đầu. Nếu bạn muốn **ghi nối tiếp** vào cuối file (append), hãy thêm tham số `true` vào phía sau như thế này:
> 
> `new FileWriter("file.txt", true);`

---

## 2. `FileReader` (Dùng để ĐỌC file)

`FileReader` giúp bạn đọc dữ liệu từ file lên. Vì nó đọc cấp thấp nên nó sẽ đọc theo **từng ký tự** (mỗi ký tự trả về một mã số nguyên ASCII/Unicode). Khi đọc hết file, nó sẽ trả về `-1`.

### 📝 Code mẫu đọc từng ký tự:

Java

```java
import java.io.FileReader;
import java.io.IOException;

public class CachDungFileReader {
    public static void main(String[] args) {
        
        try (FileReader reader = new FileReader("output.txt")) {
            int character;
            
            System.out.println("Nội dung file đọc được:");
            // Vòng lặp đọc từng ký tự cho đến khi bằng -1 (hết file)
            while ((character = reader.read()) != -1) {
                // Ép kiểu int về char để hiển thị ra chữ
                System.out.print((char) character);
            }
            
        } catch (IOException e) {
            System.out.println("Không tìm thấy file hoặc lỗi đọc file!");
            e.printStackTrace();
        }
    }
}
```

---

## 🚀 Nâng cấp: Tại sao người ta ít khi dùng `FileReader` đứng một mình?

Nếu bạn để ý kỹ code đọc file ở trên, việc `FileReader` bốc từng ký tự một từ ổ cứng rồi in ra màn hình sẽ **rất chậm** nếu file đó nặng vài MB (ổ cứng phải quay và đọc liên tục hàng triệu lần).

Vì vậy, các lập trình viên thường dùng giải pháp "Bọc đồ sứ trong xốp": Gắn thêm bộ nhớ đệm `BufferedReader` ra bên ngoài `FileReader` để nó bốc một mớ chữ đưa vào RAM xử lý cho nhanh, đồng thời giúp ta đọc được **nguyên cả một dòng chữ** bằng hàm `readLine()`.

### 📝 Code phối hợp đỉnh cao (Luôn khuyên dùng cho file text):

Java

```
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class DocFileChuanPro {
    public static void main(String[] args) {
        
        // Lồng FileReader vào trong BufferedReader
        try (BufferedReader br = new BufferedReader(new FileReader("output.txt"))) {
            String line;
            
            System.out.println("Đọc file bằng BufferedReader:");
            // Đọc nguyên dòng cho đến khi bằng null (hết file)
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```