Bất kỳ ai học Java cũng bắt đầu bằng lệnh `System.out.println()`, nhưng không phải ai cũng hiểu bản chất thực sự nằm bên dưới "nắp capo" của lớp `System`.

Hãy cùng "mổ xẻ" hai slide này nhé!

---

### 1. Bộ 3 "Quyền Lực" Của Lớp `System` (Slide 1)

Lớp `java.lang.System` cung cấp sẵn cho chúng ta 3 đối tượng tĩnh (static objects) để giao tiếp trực tiếp với hệ điều hành:

- **`System.out` (Luồng ra chuẩn):** Đại diện cho màn hình hiển thị. Bản chất nó là một đối tượng của lớp `PrintStream`. Khi bạn gọi `System.out.print()`, bạn đang đẩy dữ liệu ra màn hình.
    
- **`System.err` (Luồng lỗi chuẩn):** Cũng đại diện cho màn hình, cũng là `PrintStream`, nhưng nó được thiết kế **dành riêng để in các thông báo lỗi**.
    
    - _Mẹo nhỏ:_ Trong các IDE hiện đại (như Eclipse, IntelliJ), nội dung in từ `System.err` thường được hiển thị bằng **màu đỏ** để bạn dễ dàng phân biệt đâu là dữ liệu bình thường (`out`), đâu là lỗi (`err`).
        
- **`System.in` (Luồng vào chuẩn):** Đại diện cho bàn phím của bạn. Bản chất nó là một **`InputStream`** (Luồng byte). Bất cứ phím nào bạn gõ, nó sẽ chui vào hệ thống dưới dạng các con số nhị phân (byte).
    

---

### 2. Tại Sao Không Nên Dùng `System.in` Trực Tiếp? (Slide 2)

Như slide thứ 2 đã cảnh báo: **"`System.in` không nên sử dụng được trực tiếp"**. Tại sao vậy?

Vì `System.in` là một luồng **Byte** (chỉ hiểu các con số từ 0-255).

- Nếu bạn gõ chữ `"A"`, lệnh `System.in.read()` sẽ trả về con số `65`.
    
- Nếu bạn gõ một chữ tiếng Việt có dấu, nó sẽ trả về 2-3 con số rời rạc. Việc tự tay ghép các con số này lại thành một chữ hoàn chỉnh là một "cơn ác mộng" đối với lập trình viên.
    

Do đó, con người cần đọc **văn bản (String)**, chứ không cần đọc **byte thô**.

---

### 3. Dây Chuyền "Tiêu Chuẩn" Để Đọc Dữ Liệu Bàn Phím

Để chuyển đổi từ những cú gõ phím thô kệch thành một chuỗi (String) hoàn chỉnh, Java thiết kế một "dây chuyền lắp ráp" gồm 3 lớp bọc lấy nhau:

1. **Nguồn cấp (Byte):** `System.in` nhận các byte từ bàn phím.
    
2. **Máy phiên dịch (Char):** `InputStreamReader` sẽ bọc lấy `System.in`. Nhiệm vụ của nó là gom các byte lại và dịch thành các ký tự (Char) có ý nghĩa (đọc được tiếng Việt).
    
3. **Bộ gom dòng (String):** `BufferedReader` bọc lấy `InputStreamReader`. Nó cung cấp một cái "thùng chứa" (Buffer). Nó sẽ gom tất cả các ký tự bạn gõ cho đến khi bạn bấm phím `Enter`, sau đó đóng gói tất cả thành một chuỗi `String` hoàn chỉnh trả về cho bạn.
    

**Code minh họa thực tế:**


```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class NhapDuLieuConsole {
    public static void main(String[] args) {
        // Tạo "dây chuyền" 3 lớp bọc nhau như slide yêu cầu
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        System.out.print("Vui lòng nhập tên của bạn: ");
        
        try {
            // Lệnh readLine() sẽ đợi cho đến khi người dùng gõ xong và bấm Enter
            String ten = reader.readLine(); 
            System.out.println("Xin chào, " + ten + "!");
        } catch (IOException e) {
            System.err.println("Lỗi khi đọc dữ liệu: " + e.getMessage());
        }
    }
}
```

Ngày nay, để cho người mới học dễ thở hơn, Java đã ra mắt lớp **`java.util.Scanner`** với cú pháp ngắn gọn hơn rất nhiều (`Scanner sc = new Scanner(System.in);`).

Bạn có muốn mình làm một bảng so sánh nhanh xem giữa cách dùng `BufferedReader` (cổ điển, hiệu năng cao) và `Scanner` (hiện đại, tiện lợi) thì nên chọn cái nào trong từng tình huống cụ thể không?