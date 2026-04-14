## Lớp Bao Bọc (Wrapper Class) trong Java

Như chúng ta vừa nhắc đến ở phần Generics, bạn không thể sử dụng các kiểu dữ liệu nguyên thủy (primitive types) như `int`, `double`, `char` trực tiếp trong các cấu trúc dữ liệu tổng quát như `ArrayList<T>`.

Đó chính là lúc **Lớp Bao Bọc (Wrapper Class)** xuất hiện để giải cứu. Nói một cách đơn giản, Wrapper Class là một lớp đối tượng dùng để "gói" (wrap) một giá trị nguyên thủy vào bên trong nó, biến một giá trị đơn thuần thành một **đối tượng (Object)** thực thụ.

### Danh sách các Lớp Wrapper

Mỗi kiểu dữ liệu nguyên thủy trong Java đều có một Lớp Wrapper tương ứng nằm trong gói `java.lang`:

|**Kiểu nguyên thủy (Primitive)**|**Lớp Wrapper tương ứng (Object)**|
|---|---|
|`byte`|`Byte`|
|`short`|`Short`|
|`int`|**`Integer`** (Lưu ý viết đầy đủ)|
|`long`|`Long`|
|`float`|`Float`|
|`double`|`Double`|
|`char`|**`Character`** (Lưu ý viết đầy đủ)|
|`boolean`|`Boolean`|

---

### Tại sao chúng ta cần Lớp Wrapper?

1. **Sử dụng trong Collection Framework (Generics):** Các cấu trúc dữ liệu như `ArrayList`, `HashMap`, `HashSet` chỉ làm việc với đối tượng (Object), không làm việc với kiểu nguyên thủy. Bạn buộc phải dùng `ArrayList<Integer>` chứ không thể dùng `ArrayList<int>`.
    
2. **Khả năng chứa giá trị `null`:** Kiểu nguyên thủy luôn có giá trị mặc định (ví dụ `int` mặc định là `0`). Nhưng đôi khi trong cơ sở dữ liệu hoặc API, một con số có thể không có giá trị (null). Lớp Wrapper là Object nên nó có thể nhận giá trị `null`.
    
3. **Cung cấp các phương thức tiện ích:** Các lớp Wrapper chứa rất nhiều phương thức tĩnh (static methods) hữu ích để chuyển đổi dữ liệu. Ví dụ nổi tiếng nhất là `Integer.parseInt("123")` để biến một chuỗi thành số nguyên.
    

---

### Tự động đóng gói (Autoboxing) và Mở gói (Unboxing)

Trước Java 5, việc chuyển đổi qua lại giữa kiểu nguyên thủy và Lớp Wrapper rất thủ công và rườm rà:


```java
// Trước Java 5
int soNguyenThuy = 5;
Integer doiTuongSo = Integer.valueOf(soNguyenThuy); // Đóng gói thủ công
int laiLaSoNguyenThuy = doiTuongSo.intValue();      // Mở gói thủ công
```

Tuy nhiên, từ Java 5 trở đi, trình biên dịch đã đủ thông minh để tự động làm việc này cho bạn thông qua cơ chế **Autoboxing** và **Unboxing**.

- **Autoboxing (Tự động đóng gói):** Quá trình Java tự động chuyển đổi kiểu nguyên thủy thành đối tượng Wrapper tương ứng.
    
- **Unboxing (Tự động mở gói):** Quá trình Java tự động trích xuất giá trị nguyên thủy từ đối tượng Wrapper.
    

#### Ví dụ minh họa:



```java
public class Main {
    public static void main(String[] args) {
        // 1. AUTOBOXING (int -> Integer)
        // Java tự động gọi Integer.valueOf(10) ở hậu trường
        Integer soA = 10; 
        
        // Hoạt động trơn tru với Collections
        ArrayList<Integer> danhSachSo = new ArrayList<>();
        danhSachSo.add(25); // Autoboxing: 25 (int) biến thành Integer(25)

        // 2. UNBOXING (Integer -> int)
        // Java tự động gọi soA.intValue() ở hậu trường
        int soB = soA; 
        
        // Khi tính toán, Java tự động Unboxing để cộng trừ nhân chia
        int tong = soA + 15; // soA được mở gói thành 10, rồi cộng với 15
        System.out.println("Tổng là: " + tong); // In ra 25

        // 3. SỬ DỤNG PHƯƠNG THỨC TIỆN ÍCH
        String chuoiSo = "2024";
        int nam = Integer.parseInt(chuoiSo); // Chuyển chuỗi thành int
        System.out.println("Năm nay là: " + nam);
        
        // Tìm giá trị lớn nhất/nhỏ nhất
        System.out.println("Giá trị int lớn nhất là: " + Integer.MAX_VALUE);
    }
}
```

### Một lưu ý nhỏ nhưng "chết người" khi so sánh

Vì Lớp Wrapper là **Object**, nên khi bạn so sánh hai đối tượng Wrapper bằng toán tử `==`, nó sẽ **so sánh địa chỉ vùng nhớ**, chứ không so sánh giá trị thực tế bên trong (trừ một số trường hợp được Java lưu vào bộ nhớ đệm (cache) như các số từ -128 đến 127).

Do đó, **luôn luôn sử dụng `.equals()`** khi muốn so sánh giá trị của hai biến Wrapper, giống như cách bạn làm với chuỗi `String`!