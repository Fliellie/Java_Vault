Sau khi đã đi qua một loạt các lớp I/O khá "nặng đô" và phức tạp như `BufferedReader` hay `DataInputStream` trong các ảnh trước, chúng ta cùng đến với một lớp cực kỳ thân thiện, dễ dùng và là "người bạn quốc dân" của mọi lập trình viên Java khi mới bắt đầu: **Lớp `Scanner`** (nằm trong gói `java.util.Scanner`).

Nếu các lớp trước bắt bạn phải hiểu về mảng byte, bộ đệm (buffer) hay ép kiểu phức tạp, thì `Scanner` sinh ra để "đơn giản hóa" việc đọc dữ liệu từ bàn phím hoặc từ file.

---

## 1. Bản chất của lớp `Scanner`

`Scanner` là một lớp dùng để **phân tích cú pháp (parsing)** các kiểu dữ liệu nguyên thủy và chuỗi bằng cách sử dụng các biểu thức chính quy (regular expressions). Nó chia nhỏ dữ liệu đầu vào thành các phần tử gọi là **tokens** dựa trên các ký tự phân tách (mặc định là khoảng trắng, dấu tab hoặc dấu xuống dòng).

---

## 2. Các phương thức "Ăn liền" cực kỳ tiện lợi

Điểm ăn tiền nhất của `Scanner` là nó có sẵn các hàm đọc và tự động ép kiểu cho bạn, không cần bận tâm xử lý ngoại lệ rườm rà như `readLine()` của `BufferedReader`:

|**Phương thức**|**Chức năng**|
|---|---|
|`next()`|Đọc vào một **từ** (dừng lại khi gặp khoảng trắng).|
|`nextLine()`|Đọc vào **nguyên một dòng** (bao gồm cả khoảng trắng, dừng khi xuống dòng).|
|`nextInt()`|Đọc vào một số nguyên `int`.|
|`nextDouble()`|Đọc vào một số thực `double`.|
|`nextBoolean()`|Đọc vào giá trị `true`/`false`.|
|`hasNextInt()`|Kiểm tra xem từ tiếp theo có phải là số nguyên không (trả về boolean).|

---

## 3. Code thực chiến: Đọc dữ liệu từ Bàn phím

Đây là kịch bản phổ biến nhất khi bạn viết các ứng dụng console nhỏ để tương tác với người dùng:

Java

```java

import java.util.Scanner;

public class ScannerExample {
    public static void main(String[] args) {
        // Khởi tạo Scanner để đọc từ bàn phím (System.in)
        // Dùng try-with-resources để tự động đóng luồng đầu vào
        try (Scanner scanner = new Scanner(System.in)) {
            
            System.out.print("Nhập tên của bạn: ");
            String name = scanner.nextLine(); // Đọc nguyên dòng
            
            System.out.print("Nhập tuổi của bạn: ");
            int age = scanner.nextInt(); // Đọc số nguyên
            
            System.out.print("Nhập điểm GPA: ");
            double gpa = scanner.nextDouble(); // Đọc số thực
            
            System.out.println("--- Thông tin đã nhập ---");
            System.out.println("Xin chào " + name + ", " + age + " tuổi. GPA: " + gpa);
            
        } // Scanner tự động đóng tại đây
    }
}
```

---

## ⚠️ Lỗi "Trôi lệnh" kinh điển (Cần tuyệt đối lưu ý!)

Hầu như 100% người học Java đều từng gãi đầu bứt tai vì lỗi này. Đó là khi bạn dùng một hàm đọc số (như `nextInt()`) rồi ngay sau đó dùng hàm đọc chuỗi `nextLine()`.

**Hiện tượng:** Chương trình sẽ "bỏ qua" không cho bạn nhập chuỗi ở lệnh `nextLine()`.

**Nguyên nhân:** Khi bạn gõ số `20` và nhấn `Enter`, hàm `nextInt()` chỉ lấy đi số `20`, nhưng để lại ký tự xuống dòng `\n` trong bộ đệm. Lệnh `nextLine()` ngay phía sau lập tức vớ lấy ký tự `\n` đó và hiểu rằng bạn đã nhập xong một chuỗi rỗng!

**Cách khắc phục:** Luôn thêm một lệnh `scanner.nextLine();` "vô nghĩa" ở giữa để nuốt đi ký tự xuống dòng thừa đó.



```java
int age = scanner.nextInt(); 
scanner.nextLine(); // "Nuốt" ký tự xuống dòng thừa!
String address = scanner.nextLine(); // Bây giờ mới đọc địa chỉ bình thường
```

---

## ⚖️ So sánh: Scanner vs BufferedReader

|**Tiêu chí**|**Scanner**|**BufferedReader**|
|---|---|---|
|**Tốc độ**|Chậm hơn (vì phải phân tích cú pháp).|Rất nhanh (chỉ đọc mảng ký tự thô).|
|**Tính tiện dụng**|Rất cao (Tự ép kiểu dữ liệu).|Thấp (Chỉ đọc ra String, phải tự `Integer.parseInt`).|
|**Đọc file lớn**|Không khuyến khích.|Khuyên dùng để tối ưu hiệu năng.|

Tóm lại, nếu bạn làm bài tập nhỏ, đọc dữ liệu console nhẹ nhàng $\rightarrow$ Dùng **`Scanner`**. Nếu làm việc với file lớn hàng triệu dòng $\rightarrow$ Dùng **`BufferedReader`**.

Bạn có muốn mình hướng dẫn cách dùng `Scanner` để đọc dữ liệu từ một file text thay vì từ bàn phím không? Hay bạn muốn rẽ sang một chủ đề hoàn toàn mới?