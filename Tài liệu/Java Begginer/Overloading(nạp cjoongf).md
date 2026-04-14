## 1. Overloading (Nạp chồng phương thức)

**Overloading là gì?** Overloading xảy ra khi bạn định nghĩa **nhiều phương thức có cùng tên** bên trong **cùng một lớp** (class), nhưng chúng phải **khác nhau về danh sách tham số** (khác nhau về số lượng tham số, kiểu dữ liệu của tham số, hoặc thứ tự các tham số).

- **Mục đích:** Giúp mã nguồn dễ đọc hơn, gom nhóm các hành động tương tự nhau dưới một cái tên duy nhất.
    
- **Bản chất:** Đây là thể hiện của tính đa hình lúc biên dịch (Compile-time Polymorphism).
    

**Cách sử dụng (Ví dụ trong Java):** Bạn sử dụng Overloading khi muốn thực hiện cùng một chức năng (như tính tổng, in dữ liệu) nhưng với các loại đầu vào khác nhau.

Java

```
class Calculator {
    // 1. Phương thức cộng 2 số nguyên
    public int add(int a, int b) {
        return a + b;
    }

    // 2. Overloading: Phương thức cộng 3 số nguyên (Khác số lượng tham số)
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // 3. Overloading: Phương thức cộng 2 số thực (Khác kiểu dữ liệu tham số)
    public double add(double a, double b) {
        return a + b;
    }
}
```