Trong Java, **`static`** là một từ khóa (modifier) được sử dụng để quản lý bộ nhớ và định nghĩa các thành phần thuộc về **lớp (class)** thay vì thuộc về **đối tượng (instance)** cụ thể.

Hiểu đơn giản: Nếu không có `static`, mỗi khi bạn tạo một đối tượng mới, nó sẽ có một bản sao riêng của thuộc tính đó. Nếu có `static`, tất cả các đối tượng đều dùng chung một bản sao duy nhất.

---

## 1. Các trường hợp sử dụng `static` phổ biến

### Biến tĩnh (Static Variables)

Biến được khai báo với `static` sẽ được khởi tạo một lần duy nhất khi lớp được nạp vào bộ nhớ. Nó thường được dùng để định nghĩa các hằng số hoặc các biến dùng chung cho toàn bộ ứng dụng (ví dụ: bộ đếm số lượng đối tượng).

Java

```
class Student {
    String name; // Biến instance (riêng cho từng SV)
    static String college = "Đại học Bách Khoa"; // Biến static (chung cho tất cả SV)
}
```

### Phương thức tĩnh (Static Methods)

Phương thức `static` có thể được gọi mà **không cần tạo đối tượng** của lớp đó.

- **Lưu ý:** Phương thức static chỉ có thể gọi trực tiếp các biến static và phương thức static khác. Nó không thể sử dụng từ khóa `this` hay gọi các biến instance thông thường.
    

Java

```
class MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}
// Cách dùng: MathUtils.add(5, 10); (Không cần new MathUtils())
```

### Khối tĩnh (Static Block)

Dùng để khởi tạo các biến tĩnh. Khối này chạy một lần duy nhất khi lớp được load, thậm chí trước cả hàm main.

### Lớp lồng tĩnh (Static Nested Class)

Trong Java, bạn không thể khai báo lớp ngoài là `static`, nhưng bạn có thể khai báo một lớp bên trong (inner class) là `static`. Lớp này không cần tham chiếu đến đối tượng của lớp cha.

## 2. Ví dụ về Biến Static (Bộ đếm đối tượng)

Giả sử bạn muốn theo dõi xem có bao nhiêu sinh viên đã được đăng ký vào hệ thống. Nếu dùng biến thông thường, mỗi khi tạo sinh viên mới, bộ đếm sẽ luôn reset về 1. Nhưng với `static`, bộ đếm sẽ tăng dần và dùng chung cho tất cả.

Java

```
class Student {
    String name;
    static int count = 0; // Biến dùng chung cho toàn bộ class

    Student(String name) {
        this.name = name;
        count++; // Mỗi lần tạo 1 đối tượng, count sẽ tăng lên
    }

    void display() {
        System.out.println("Tên: " + name + " | Tổng số SV: " + count);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("An");
        Student s2 = new Student("Bình");
        Student s3 = new Student("Chi");

        s3.display(); // Kết quả: Tên: Chi | Tổng số SV: 3
    }
}
```

---

## 3. Ví dụ về Phương thức Static (Tiện ích)

Các phương thức `static` cực kỳ hữu ích cho các hàm tính toán mà không cần lưu trữ dữ liệu cá nhân của đối tượng. Bạn gọi chúng trực tiếp thông qua tên lớp.

Java

```
class MyMath {
    // Phương thức static tính diện tích hình tròn
    public static double tinhDienTichHinhTron(double banKinh) {
        return 3.14159 * banKinh * banKinh;
    }
}

public class TestStatic {
    public static void main(String[] args) {
        // Không cần "new MyMath()", gọi trực tiếp luôn:
        double dt = MyMath.tinhDienTichHinhTron(5.0);
        System.out.println("Diện tích: " + dt);
    }
}
```

---

## 4. Ví dụ về Hằng số Static (Global Constants)

Thông thường, `static` hay đi kèm với `final` để tạo ra các hằng số dùng chung trong toàn bộ dự án (ví dụ: cấu hình hệ thống, URL API, các giá trị toán học).

Java

```
class Config {
    public static final String API_URL = "https://api.example.com";
    public static final int TIMEOUT = 5000;
}

public class DatabaseService {
    void connect() {
        // Truy cập hằng số ở bất cứ đâu trong code
        System.out.println("Connecting to: " + Config.API_URL);
    }
}
```

---

## ⚠️ Sai lầm hay gặp khi mới học Static

Một lỗi phổ biến là cố gắng gọi một biến **không phải static** từ trong một hàm **static** (như hàm `main`).

Java

```
public class Test {
    int x = 10; // Biến instance (non-static)

    public static void main(String[] args) {
        // System.out.println(x); // LỖI! Hàm static không biết x của đối tượng nào.
        
        // Cách sửa: Phải tạo đối tượng mới gọi được x
        Test obj = new Test();
        System.out.println(obj.x); 
    }
}
```

**Tại sao lại lỗi?** Vì khi hàm `main` (static) chạy, đối tượng `obj` chưa chắc đã tồn tại trong bộ nhớ, nên nó không thể "nhìn thấy" biến `x`.
