## Lớp `Object` trong Java: "Ông Tổ" Của Mọi Lớp

Trong thế giới Java, lớp **`java.lang.Object`** đóng vai trò cực kỳ đặc biệt: Nó là gốc rễ (root) của toàn bộ cây phả hệ class.

Dù bạn có khai báo hay không, **mọi lớp trong Java (kể cả các mảng) đều mặc định kế thừa trực tiếp hoặc gián tiếp từ lớp `Object`**.

### 1. Các con kế thừa của lớp `Object`

Thực chất, **tất cả các lớp** đều là con của `Object`.

Khi bạn viết một lớp mới mà không dùng từ khóa `extends`:

Java

```java
class SinhVien {
    // ...
}
```

Trình biên dịch Java sẽ âm thầm tự động thêm `extends Object` vào cho bạn:

Java

```java
class SinhVien extends Object {
    // ...
}
```

Ngay cả các lớp có sẵn của Java như `String`, `Integer`, `ArrayList`, hay các ngoại lệ (Exceptions) đều là con cháu của `Object`. Nhờ có tính chất này, bạn có thể tạo ra một mảng hoặc danh sách có thể chứa BẤT KỲ kiểu dữ liệu đối tượng nào bằng cách khai báo mảng/danh sách kiểu `Object` (như cách `ArrayList` làm mà chúng ta vừa bàn ở trên).

---

### 2. Các phương thức phổ biến của lớp `Object`

Vì mọi lớp đều kế thừa từ `Object`, nên mọi đối tượng trong Java đều sở hữu sẵn các phương thức dưới đây. Tuy nhiên, trong thực tế lập trình, chúng ta thường xuyên phải **ghi đè (override)** một số phương thức này để chúng hoạt động đúng với logic của lớp con.

Dưới đây là các phương thức quan trọng nhất bạn cần nắm vững:

#### Nhóm 1: Nhóm thông tin và so sánh (Thường xuyên phải ghi đè)

- **`toString()`**
    
    - **Tác dụng:** Trả về một chuỗi đại diện cho đối tượng.
        
    - **Mặc định:** Trả về TênLớp + `@` + Mã Hash (ví dụ: `SinhVien@15db9742`).
        
    - **Thực tế:** Bạn gần như luôn luôn nên ghi đè hàm này để in ra thông tin có ý nghĩa hơn (ví dụ: in ra tên, tuổi, mã sinh viên thay vì một chuỗi loằng ngoằng vô nghĩa). Hệ thống sẽ tự động gọi hàm này khi bạn dùng `System.out.println(doiTuong)`.
        
- **`equals(Object obj)`**
    
    - **Tác dụng:** Kiểm tra xem đối tượng hiện tại có "bằng" với đối tượng `obj` truyền vào hay không.
        
    - **Mặc định:** Chỉ so sánh địa chỉ vùng nhớ (giống hệt toán tử `==`). Nghĩa là 2 đối tượng `SinhVien` có cùng tên, cùng mã sinh viên nhưng được `new` ở 2 nơi khác nhau thì `equals` mặc định vẫn trả về `false`.
        
    - **Thực tế:** Bắt buộc phải ghi đè nếu bạn muốn so sánh giá trị bên trong (ví dụ: 2 sinh viên được coi là `equals` nếu có chung Mã Sinh Viên).
        
- **`hashCode()`**
    
    - **Tác dụng:** Trả về một số nguyên (mã băm) đại diện cho địa chỉ vùng nhớ của đối tượng.
        
    - **Quy tắc vàng:** Nếu bạn đã ghi đè `equals()`, bạn **bắt buộc** phải ghi đè `hashCode()`. Nếu hai đối tượng `equals()` nhau trả về `true`, thì `hashCode()` của chúng phải giống hệt nhau. Điều này cực kỳ quan trọng khi bạn lưu đối tượng vào các cấu trúc dữ liệu dựa trên mã băm như `HashSet` hay `HashMap`.
        

#### Nhóm 2: Nhóm làm việc với Đa luồng (Multithreading)

Các phương thức này được dùng để các luồng (threads) giao tiếp với nhau khi tranh chấp tài nguyên. Bạn không thể ghi đè chúng (chúng được khai báo là `final`).

- **`wait()` / `wait(long timeout)`**: Bắt luồng hiện tại tạm dừng (chờ đợi) và nhả khóa (lock) cho đến khi một luồng khác đánh thức nó.
    
- **`notify()`**: Đánh thức một luồng ngẫu nhiên đang `wait()` trên đối tượng này.
    
- **`notifyAll()`**: Đánh thức tất cả các luồng đang `wait()` trên đối tượng này.
    

#### Nhóm 3: Nhóm tiện ích hệ thống

- **`getClass()`**
    
    - **Tác dụng:** Trả về đối tượng `Class` đại diện cho kiểu dữ liệu thực sự của đối tượng lúc runtime (rất hữu ích khi dùng Reflection).
        
    - _Không thể ghi đè (final)._
        
- **`clone()`**
    
    - **Tác dụng:** Tạo và trả về một bản sao (copy) của đối tượng hiện tại.
        
    - **Lưu ý:** Để dùng được hàm này, lớp của bạn phải `implements Cloneable`, nếu không sẽ bị ném lỗi `CloneNotSupportedException`. Mặc định nó chỉ copy nông (Shallow copy).
        
- **`finalize()`**
    
    - **Tác dụng:** Được Bộ thu gom rác (Garbage Collector - GC) gọi ngay trước khi đối tượng bị dọn dẹp khỏi bộ nhớ. Dùng để giải phóng các tài nguyên (như đóng file, đóng kết nối mạng).
        
    - **Lưu ý:** Kể từ Java 9, phương thức này đã bị đánh dấu là **Deprecated** (không khuyến khích dùng nữa) vì nó không đáng tin cậy và làm giảm hiệu năng. Thay vào đó, Java khuyên dùng khối `try-with-resources` hoặc `Cleaner`.