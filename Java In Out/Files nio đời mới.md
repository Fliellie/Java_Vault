Bước từ gói `java.io` cổ điển sang `java.nio` (New I/O) giống như việc bạn đang từ một chiếc xe số đời cũ bước lên một chiếc xe tay ga đời mới vậy.

Gói `java.nio.file` (đặc biệt là bộ đôi `Path` và `Files`) được Java ra mắt từ phiên bản 7 nhằm khắc phục những điểm cực kỳ "ức chế" mà lớp `File` truyền thống để lại.

Chúng ta hãy cùng mổ xẻ xem tại sao các "cao thủ" Java bây giờ lại chuộng nó đến thế nhé!

---

### 1. Bản chất: `File` (Cũ) vs `Path` (Mới)

- **`java.io.File` (Cổ điển):** Đại diện cho một tệp tin hoặc một thư mục. Nó vừa dùng để **định vị** đường dẫn, vừa ôm đồm luôn các **hành động** như tạo, xóa, kiểm tra file.
    
- **`java.nio.file.Path` (Hiện đại):** Chỉ làm đúng một nhiệm vụ duy nhất: **Định vị**. Nó là một cái la bàn đại diện cho đường dẫn trong hệ thống.
    
- **`java.nio.file.Files` (Hiện đại):** Là một lớp chứa toàn các hàm `static` để thực hiện **hành động** (đọc, ghi, copy, xóa...). Nó sẽ nhận đầu vào là các ông `Path` ở trên.
    

> 💡 **Sự tách biệt này cực kỳ thông minh:** `Path` lo việc tìm đường, còn `Files` lo việc hành động.

---

### 2. Tại sao `Path` và `Files` lại "ăn đứt" `File` truyền thống?

Hãy cùng đặt chúng lên bàn cân qua 4 "nỗi đau" lớn mà lớp `File` cũ gây ra:

#### Trận chiến 1: Cách xử lý lỗi (Error Handling)

- **`File` (Cũ):** Rất nhiều phương thức của nó trả về kiểu `boolean` (`true`/`false`) khi thất bại thay vì ném ra Exception.
    
    - _Ví dụ:_ Bạn gọi `file.delete()`, nếu file không xóa được (do bị khóa hoặc không có quyền), nó chỉ lẳng lặng trả về `false`. Bạn sẽ vò đầu bứt tai không biết tại sao không xóa được!
        
- **`Path/Files` (Mới):** Nếu thất bại, nó sẽ ném thẳng ra các Exception rất rõ ràng như `NoSuchFileException`, `AccessDeniedException`... Giúp bạn "bắt bệnh" chuẩn 100%.
    

#### Trận chiến 2: Tên gọi gây lú

- **`File` (Cũ):** Tên class là `File` nhưng nó đại diện cho cả... thư mục (Folder). Lệnh `file.isDirectory()` nghe rất mâu thuẫn.
    
- **`Path/Files` (Mới):** Tên gọi chuẩn xác. Đường dẫn tới file hay tới thư mục thì đều gọi chung là `Path`.
    

#### Trận chiến 3: Khả năng duyệt thư mục lớn

- **`File` (Cũ):** Khi bạn dùng `file.listFiles()` ở một thư mục có 100.000 file, Java sẽ đứng hình để nạp toàn bộ danh sách đó vào RAM rồi mới trả về mảng. Rất dễ bị tràn bộ nhớ (Out of Memory).
    
- **`Path/Files` (Mới):** Hỗ trợ `Files.walk()` và `Files.list()` trả về một `Stream`. Nó sẽ đọc đến đâu trả về đến đấy (Lazy loading), cực kỳ tiết kiệm RAM và mượt mà.
    

---

### 3. So sánh trực diện qua Code

Hãy xem sự gọn gàng của cách làm hiện đại nhé:

#### Kịch bản A: Đọc toàn bộ nội dung của một file văn bản

- **Cách cũ (`java.io`):** Bạn phải tạo `FileReader`, bọc vào `BufferedReader`, tạo vòng lặp `while` để đọc từng dòng, rồi đóng file trong `finally`. (Code dài cả chục dòng như Bài 5 của bạn).
    
- **Cách mới (`java.nio`):** Gọn gàng trong đúng 1 dòng nhạc!

    ```java
    List<String> lines = Files.readAllLines(Path.of("config.txt"));
    ```


#### Kịch bản B: Copy file

- **Cách cũ (`java.io`):** Phải mở `FileInputStream`, `FileOutputStream`, tạo mảng byte đệm, vừa đọc vừa ghi trong vòng lặp.
    
- **Cách mới (`java.nio`):** ```java
    
    Files.copy(Path.of("nguon.txt"), Path.of("dich.txt"));
    

---
### 🛠️ Code Bài 5 "Đập đi xây lại" bằng `Path` và `Files`

Java

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Map;
import java.util.Scanner;
import java.util.stream.Collectors;

public class DocCauHinhNIO {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Nhập đường dẫn file config: ");
        String filePath = scanner.nextLine();

        try {
            // 1. Dùng Path để định vị file
            Path path = Paths.get(filePath);

            // 2. Đọc file, lọc dòng và đổ thẳng vào Map chỉ với 1 luồng xử lý (Stream)
            Map<String, String> configMap = Files.lines(path)
                .map(String::trim) // Xóa khoảng trắng 2 đầu mỗi dòng
                .filter(line -> !line.isEmpty() && line.contains("=")) // Bỏ qua dòng trống và dòng thiếu dấu '='
                .map(line -> line.split("=", 2)) // Tách làm 2 phần: key và value
                .collect(Collectors.toMap(
                    parts -> parts[0].trim(), // Key
                    parts -> parts[1].trim(), // Value
                    (existing, replacement) -> replacement // Nếu trùng key thì lấy cái sau
                ));

            // 3. Kiểm tra dữ liệu (Tận dụng lại hàm cũ)
            kiemTraDuLieu(configMap);
            
            System.out.println("Config loaded successfully.");

        } catch (java.nio.file.NoSuchFileException e) {
            // NIO ném ra lỗi rất cụ thể khi không tìm thấy file thay vì FileNotFoundException
            System.out.println("Config file not found.");
        } catch (IOException e) {
            System.out.println("I/O error.");
            e.printStackTrace();
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format.");
        } catch (InvalidConfigException e) {
            System.out.println("Invalid config: " + e.getMessage());
        } finally {
            // 4. Không cần reader.close() nữa! Files.lines() tự đóng file cực kỳ an toàn.
            System.out.println("Program finished.");
            scanner.close();
        }
    }

    // Giữ nguyên hàm kiểm tra logic từ bài trước
    private static void kiemTraDuLieu(Map<String, String> map) throws InvalidConfigException {
        if (!map.containsKey("username")) throw new InvalidConfigException("Missing username");
        if (!map.containsKey("timeout")) throw new InvalidConfigException("Missing timeout");

        int timeout = Integer.parseInt(map.get("timeout"));
        if (timeout <= 0) throw new InvalidConfigException("timeout must be > 0");

        if (map.containsKey("maxConnections")) {
            int maxConn = Integer.parseInt(map.get("maxConnections"));
            if (maxConn < 1) throw new InvalidConfigException("maxConnections must be >= 1");
        }
    }
}

// Giữ nguyên Custom Exception
class InvalidConfigException extends Exception {
    public InvalidConfigException(String message) { super(message); }
}
```

---

### 😎 Những điểm "ăn tiền" của đoạn code mới này:

1. **Không còn khối `finally` rườm rà để đóng file:** `Files.lines()` mở file ra và tự động đóng lại ngay sau khi xử lý xong luồng. Bạn bớt được nỗi lo rò rỉ bộ nhớ.
    
2. **Không cần vòng lặp `while` thủ công:** Toàn bộ thao tác đọc từng dòng $\rightarrow$ cắt khoảng trắng $\rightarrow$ kiểm tra dấu `=` $\rightarrow$ nhét vào Map được gộp lại thành một chuỗi hành động liền mạch nhìn cực kỳ chuyên nghiệp.
    
3. **Bắt đúng bệnh:** Lỗi thiếu file bây giờ được chỉ đích danh là `NoSuchFileException` (một lớp con siêu cụ thể của `IOException`), nghe "xịn" hơn hẳn `FileNotFoundException` của thời kỳ đồ đá cũ.


### 1. Nhóm Đọc / Ghi file "Mì ăn liền" (Cực nhanh cho file nhỏ)

Nếu file của bạn có dung lượng vừa phải (vài MB đổ lại), bạn nên dùng thẳng các hàm này vì chúng tự mở và tự đóng file cho bạn luôn, chỉ bằng 1 dòng code.

- **`Files.readAllLines(Path path)`**
    
    - **Tác dụng:** Đọc toàn bộ file và trả về một danh sách các dòng `List<String>`.
        
    - **Khi nào dùng:** Cần lấy toàn bộ chữ trong file text để xử lý nhanh.
        
- **`Files.readString(Path path)`** (Từ Java 11)
    
    - **Tác dụng:** Đọc toàn bộ file và gom tất cả lại thành 1 chuỗi `String` duy nhất.
        
- **`Files.write(Path path, Iterable<? extends CharSequence> lines)`**
    
    - **Tác dụng:** Ghi một danh sách các chuỗi vào file. Mỗi phần tử trong list sẽ là một dòng.
        
- **`Files.writeString(Path path, CharSequence csq)`** (Từ Java 11)
    
    - **Tác dụng:** Ghi thẳng một chuỗi `String` dài vào file.
        

---

### 2. Nhóm Xử lý dữ liệu lớn (Stream API)

Khi file quá nặng hoặc thư mục có quá nhiều file, nạp hết vào RAM sẽ làm sập chương trình. Lúc này ta dùng luồng Stream (Lazy Loading - đọc tới đâu xử lý tới đó).

- **`Files.lines(Path path)`**
    
    - **Tác dụng:** Trả về một `Stream<String>`. Rất mạnh khi kết hợp với các hàm lọc `filter()`, `map()` của Java 8 như bài 5 chúng ta vừa làm.
        
- **`Files.list(Path dir)`**
    
    - **Tác dụng:** Trả về `Stream<Path>` chứa danh sách các file và thư mục con nằm trực tiếp bên trong thư mục `dir` (không đi sâu vào các thư mục cháu chắt).
        
- **`Files.walk(Path start)`**
    
    - **Tác dụng:** Duyệt "xuyên lục địa"! Nó sẽ đi sâu vào tất cả các thư mục con, cháu, chắt... từ thư mục gốc để tìm file. Cực kỳ bá đạo khi bạn muốn tìm kiếm file trong máy.
        

---

### 3. Nhóm Quản lý File và Thư mục (Tạo, Xóa, Copy...)

- **`Files.exists(Path path)`**
    
    - **Tác dụng:** Kiểm tra xem file hoặc thư mục đó có thực sự tồn tại trên ổ cứng hay chưa (Trả về `true`/`false`).
        
- **`Files.createFile(Path path)`**
    
    - **Tác dụng:** Tạo một file rỗng mới tinh. Sẽ báo lỗi nếu file đã có sẵn.
        
- **`Files.createDirectory(Path path)`**
    
    - **Tác dụng:** Tạo một thư mục mới.
        
- **`Files.createDirectories(Path path)`**
    
    - **Tác dụng:** Tạo thư mục và **tạo luôn cả các thư mục cha** nếu chúng chưa tồn tại (Ví dụ bạn muốn tạo đường dẫn `A/B/C` mà thư mục `A` chưa có, hàm này sẽ tự tạo cả `A`, `B` rồi tới `C`).
        
- **`Files.delete(Path path)`**
    
    - **Tác dụng:** Xóa file hoặc thư mục. Thư mục phải rỗng thì mới xóa được. Ném lỗi nếu file không tồn tại.
        
- **`Files.deleteIfExists(Path path)`**
    
    - **Tác dụng:** Xóa file nếu nó tồn tại. Nếu file không có sẵn thì lẳng lặng bỏ qua chứ không ném lỗi (Rất an toàn).
        
- **`Files.copy(Path source, Path target)`**
    
    - **Tác dụng:** Sao chép file từ nguồn sang đích.
        
- **`Files.move(Path source, Path target)`**
    
    - **Tác dụng:** Di chuyển file (hoặc dùng để đổi tên file).
        

---

### 4. Nhóm Kiểm tra thuộc tính (Metadata)

- **`Files.isDirectory(Path path)`**: Kiểm tra xem đường dẫn này có phải là một thư mục hay không.
    
- **`Files.isRegularFile(Path path)`**: Kiểm tra xem đây có phải là một file bình thường (không phải thư mục, không phải file ẩn/hệ thống) hay không.
    
- **`Files.size(Path path)`**: Trả về dung lượng của file tính bằng đơn vị **Byte**.
    

---

### 💡 Ví dụ "thực chiến" tổng hợp:

Hãy xem sự ngắn gọn khi kết hợp các hàm này để kiểm tra và tạo file:

Java

```java
Path path = Paths.get("data/config.txt");

try {
    // 1. Nếu chưa có thư mục 'data', tạo luôn
    if (!Files.exists(path.getParent())) {
        Files.createDirectories(path.getParent());
    }
    
    // 2. Nếu file chưa tồn tại, tạo file và ghi dữ liệu mẫu
    if (!Files.exists(path)) {
        Files.createFile(path);
        Files.writeString(path, "theme=dark\nfontsize=14");
        System.out.println("Đã khởi tạo file cấu hình mặc định!");
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

