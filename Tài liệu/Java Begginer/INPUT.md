### 1. Xuất dữ liệu (Output)

Đây là phần đơn giản nhất. Bạn đã sử dụng nó rất nhiều rồi:

- **`System.out.println("Nội dung");`**: In ra màn hình và **xuống dòng**.
    
- **`System.out.print("Nội dung");`**: In ra màn hình và **không xuống dòng**.
    
- **`System.out.printf("Định dạng", biến);`**: In ra theo khuôn mẫu (rất hữu ích khi cần căn lề hoặc làm tròn số).
    

**Ví dụ:**

Java

```
double salary = 1250.55;
System.out.printf("Lương của bạn là: %.2f VNĐ", salary); 
// Kết quả: Lương của bạn là: 1250.55 VNĐ
```

---

### 2. Nhập dữ liệu (Input)

Để nhập liệu từ bàn phím, chúng ta sử dụng lớp `Scanner` nằm trong thư viện `java.util`.

#### Quy trình 3 bước:

1. **Import:** Thêm `import java.util.Scanner;` ở đầu file.
    
2. **Khởi tạo:** Tạo đối tượng `Scanner`.
    
3. **Sử dụng:** Dùng các hàm để đọc kiểu dữ liệu tương ứng.
    

Java

```
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        // Khởi tạo Scanner
        Scanner sc = new Scanner(System.in);

        System.out.print("Nhập tên: ");
        String name = sc.nextLine(); // Đọc chuỗi (cả dòng)

        System.out.print("Nhập tuổi: ");
        int age = sc.nextInt(); // Đọc số nguyên

        System.out.print("Nhập lương: ");
        double salary = sc.nextDouble(); // Đọc số thực

        System.out.println(name + " " + age + " tuổi, lương: " + salary);
        
        sc.close(); // Luôn nhớ đóng Scanner khi dùng xong để giải phóng bộ nhớ
    }
}
```

---

### 3. Lưu ý cực kỳ quan trọng: Lỗi "Trôi" lệnh Scanner

Đây là lỗi mà 90% người mới học Java đều gặp phải. Khi bạn dùng `nextInt()` hoặc `nextDouble()` rồi sau đó gọi `nextLine()`, chương trình sẽ **tự động bỏ qua** dòng nhập liệu tiếp theo.

**Tại sao?** Vì `nextInt()` chỉ đọc số và để lại ký tự "xuống dòng" (`\n`) trong bộ nhớ đệm. `nextLine()` sau đó đọc ngay cái "xuống dòng" đó và tưởng bạn đã nhập xong.

**Cách khắc phục:** Thêm một lệnh `sc.nextLine()` rỗng sau mỗi lệnh đọc số để "xả" bộ nhớ đệm.

Java

```
System.out.print("Nhập tuổi: ");
int age = sc.nextInt(); 

sc.nextLine(); // "Xả" ký tự xuống dòng còn thừa

System.out.print("Nhập tên: ");
String name = sc.nextLine(); // Bây giờ lệnh này mới hoạt động đúng
```

---

### 4. Bảng tổng hợp các hàm của `Scanner`

| **Kiểu dữ liệu** | **Hàm sử dụng**                                           |
| ---------------- | --------------------------------------------------------- |
| Chuỗi            | `sc.next()` (đọc 1 từ) hoặc `sc.nextLine()` (đọc cả dòng) |
| Số nguyên        | `sc.nextInt()`                                            |
| Số thực          | `sc.nextDouble()`                                         |
| Số thực (float)  | `sc.nextFloat()`                                          |
| Boolean          | `sc.nextBoolean()`                                        |