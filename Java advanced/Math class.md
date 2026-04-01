## Lớp `Math` trong Java: Hộp Công Cụ Toán Học Trực Tiếp

Lớp **`java.lang.Math`** là một trong những lớp được sử dụng thường xuyên nhất trong Java. Nó chứa một tập hợp các phương thức và hằng số tĩnh (static) để thực hiện các phép toán từ cơ bản (cộng, trừ, làm tròn) đến phức tạp (lượng giác, logarit, lũy thừa).

Vì nằm trong gói `java.lang`, bạn **không cần phải `import`** lớp này mà có thể lấy ra xài ngay lập tức.

---

### 1. Bốn Điều Tối Quan Trọng Cần Lưu Ý (Gotchas)

Trước khi đi vào các hàm cụ thể, đây là những đặc điểm "cốt lõi" của lớp `Math` mà bạn phải nắm rõ để tránh lỗi:

- **Không thể khởi tạo (No Instantiation):** Lớp `Math` được thiết kế với một hàm tạo (constructor) mang quyền truy cập `private`. Điều này có nghĩa là bạn **tuyệt đối không thể** viết `Math m = new Math();`.
    
- **Mọi thứ đều là `static`:** Vì không thể tạo đối tượng, tất cả các thuộc tính và phương thức của lớp này đều là `static`. Bạn gọi chúng trực tiếp thông qua tên lớp (ví dụ: `Math.max(10, 5)`).
    
- **Lượng giác tính bằng Radian, KHÔNG phải Độ (Degrees):** Đây là lỗi sai cực kỳ phổ biến. Nếu bạn muốn tính Sin của góc $90^\circ$, viết `Math.sin(90)` sẽ cho ra kết quả sai bét. Bạn bắt buộc phải đổi sang Radian trước bằng cách dùng `Math.toRadians(90)`.
    
- **Cẩn thận với Tràn Số (Integer Overflow):** Các phép toán bình thường của Java không báo lỗi khi số quá lớn bị tràn (ví dụ: `int` vượt quá 2.14 tỷ sẽ quay vòng về số âm). Từ Java 8, lớp `Math` bổ sung các hàm "Exact" như `Math.addExact()`, `Math.multiplyExact()`. Các hàm này sẽ ném ra lỗi `ArithmeticException` nếu xảy ra tràn số, giúp ứng dụng của bạn an toàn hơn.
    

---

### 2. Các Hằng Số Cố Định

Lớp `Math` cung cấp sẵn 2 hằng số toán học kinh điển với độ chính xác cao (kiểu `double`):

- **`Math.PI`**: Đại diện cho số $\pi$ (tỉ số giữa chu vi và đường kính hình tròn), giá trị xấp xỉ $3.14159...$
    
- **`Math.E`**: Đại diện cho cơ số logarit tự nhiên $e$, giá trị xấp xỉ $2.71828...$
    

---

### 3. Các Phương Thức Phổ Biến Nhất

Dưới đây là bảng phân loại các phương thức "nhẵn mặt" nhất mà bạn sẽ dùng thường xuyên:

#### Nhóm 1: Làm tròn và Giá trị tuyệt đối

|**Phương thức**|**Chức năng**|**Ví dụ**|**Kết quả**|
|---|---|---|---|
|`Math.abs(x)`|Trả về giá trị tuyệt đối $|x|$|
|`Math.ceil(x)`|Làm tròn LÊN số nguyên gần nhất|`Math.ceil(3.1)`|`4.0`|
|`Math.floor(x)`|Làm tròn XUỐNG số nguyên gần nhất|`Math.floor(3.9)`|`3.0`|
|`Math.round(x)`|Làm tròn theo quy tắc toán học thông thường (>= 0.5 thì lên)|`Math.round(3.5)`|`4`|

#### Nhóm 2: Lớn nhất, Nhỏ nhất

|**Phương thức**|**Chức năng**|**Ví dụ**|**Kết quả**|
|---|---|---|---|
|`Math.max(a, b)`|Trả về số lớn hơn giữa a và b|`Math.max(10, 20)`|`20`|
|`Math.min(a, b)`|Trả về số nhỏ hơn giữa a và b|`Math.min(10, 20)`|`10`|

#### Nhóm 3: Lũy thừa và Căn bậc

|**Phương thức**|**Chức năng**|**Ví dụ**|**Kết quả**|
|---|---|---|---|
|`Math.pow(a, b)`|Tính $a^b$ (a lũy thừa b). Luôn trả về `double`.|`Math.pow(2, 3)`|`8.0`|
|`Math.sqrt(x)`|Tính căn bậc hai $\sqrt{x}$.|`Math.sqrt(16)`|`4.0`|
|`Math.cbrt(x)`|Tính căn bậc ba $\sqrt[3]{x}$.|`Math.cbrt(27)`|`3.0`|

#### Nhóm 4: Tạo số ngẫu nhiên (Rất quan trọng)

- **`Math.random()`**: Phương thức này trả về một số thực `double` ngẫu nhiên trong khoảng: **$0.0 \le x < 1.0$** (có thể bằng 0.0 nhưng luôn nhỏ hơn 1.0).
    

---

### 4. Code Mẫu Ứng Dụng Thực Tế

Dưới đây là một đoạn code tổng hợp cách sử dụng lớp `Math`, đặc biệt là **công thức kinh điển để tạo một số nguyên ngẫu nhiên trong một khoảng (min, max)**:

Java

```
public class ToanHocVui {
    public static void main(String[] args) {
        
        // 1. Tính diện tích hình tròn (S = PI * r^2)
        double banKinh = 5.0;
        double dienTich = Math.PI * Math.pow(banKinh, 2);
        System.out.println("Diện tích hình tròn: " + Math.round(dienTich)); // Làm tròn cho đẹp

        // 2. Lượng giác (Phải đổi sang Radian)
        

[Image of unit circle radians and degrees]

        double gocDo = 30.0;
        double gocRadian = Math.toRadians(gocDo);
        System.out.println("Sin của 30 độ: " + Math.sin(gocRadian)); // Kết quả xấp xỉ 0.5

        // 3. CÔNG THỨC: Tạo số nguyên ngẫu nhiên từ min đến max (bao gồm cả max)
        int min = 10;
        int max = 50;
        
        // Giải thích: Math.random() ra số từ 0 -> 0.999
        // Nhân với (max - min + 1) để lấy khoảng cách (ví dụ 41) -> 0 -> 40.999
        // Ép kiểu (int) cắt bỏ phần thập phân -> 0 -> 40
        // Cộng thêm min -> 10 -> 50
        int soNgauNhien = (int)(Math.random() * (max - min + 1) + min);
        
        System.out.println("Số ngẫu nhiên từ 10 đến 50: " + soNgauNhien);
        
        // 4. Bắt lỗi tràn số với hàm Exact (Java 8+)
        try {
            int a = 2000000000;
            int b = 2000000000;
            // int c = a + b; // Sẽ bị quay vòng ra số âm mà KHÔNG báo lỗi
            int c = Math.addExact(a, b); // Sẽ ném ra ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Lỗi: Phép cộng vượt quá giới hạn của kiểu int!");
        }
    }
}
```

**Mẹo nhỏ:** Nếu ứng dụng của bạn cần tạo số ngẫu nhiên liên tục với tần suất cực cao hoặc có các yêu cầu khắt khe hơn về phân phối xác suất, hãy xem xét sử dụng lớp **`java.util.Random`** hoặc **`java.util.concurrent.ThreadLocalRandom`** thay vì `Math.random()`.