Trong Java, việc in `hashCode()` rất đơn giản vì mọi đối tượng đều kế thừa phương thức này từ lớp `Object`. Tuy nhiên, kết quả in ra sẽ phụ thuộc vào việc lớp đó có **override** (ghi đè) phương thức `hashCode()` hay chưa.

Dưới đây là các cách thực hiện phổ biến:

### 1. In trực tiếp từ đối tượng

Đây là cách cơ bản nhất. Bạn chỉ cần gọi phương thức `.hashCode()` trên biến đối tượng.

Java

```java
Appconfig config = Appconfig.getInstance();
System.out.println("HashCode của đối tượng: " + config.hashCode());
```

### 2. In dưới dạng số Hệ thập lục phân (Hexadecimal)

Mã hash mặc định của Java thường được hiển thị dưới dạng Hex (giống như trong chuỗi `toString()` mặc định: `ClassName@hashcode`). Để in ra định dạng này, bạn dùng `Integer.toHexString()`:

Java

```java
int hash = config.hashCode();
System.out.println("HashCode (Hex): " + Integer.toHexString(hash));
```

---

### 3. Phân biệt HashCode "Thật" và HashCode "Ghi đè"

Đây là phần quan trọng nhất mà bạn cần lưu ý:

- **Override HashCode:** Nếu lớp `Appconfig` của bạn (hoặc các lớp như `String`, `Integer`) đã ghi đè `hashCode()`, thì kết quả trả về dựa trên **nội dung** dữ liệu bên trong.
    
- **Identity HashCode:** Nếu bạn muốn lấy mã hash "nguyên thủy" dựa trên địa chỉ bộ nhớ (ngay cả khi đã override), bạn phải dùng lớp `System`:
    

Java

```java
// Luôn trả về mã hash định danh duy nhất của JVM cho đối tượng đó
int nativeHash = System.identityHashCode(config);
System.out.println("Identity HashCode: " + nativeHash);
```

---

### 4. Áp dụng vào Code Singleton của bạn

Để kiểm chứng xem Singleton của bạn có hoạt động đúng hay không (tức là mọi nơi đều dùng chung một đối tượng), bạn hãy in `hashCode` ở nhiều chỗ khác nhau:

Java

```java
public class b1 {
    public static void main(String[] args) {
        Appconfig c1 = Appconfig.getInstance();
        Appconfig c2 = Appconfig.getInstance();

        System.out.println("Hash C1: " + c1.hashCode());
        System.out.println("Hash C2: " + c2.hashCode());

        if (c1.hashCode() == c2.hashCode()) {
            System.out.println("=> Cùng một đối tượng! Singleton chạy chuẩn.");
        }
    }
    
    // ... class Appconfig ở dưới ...
}
```

### Tại sao cần quan tâm đến HashCode?

Trong Java, có một nguyên tắc vàng: **"Nếu hai đối tượng bằng nhau (equals), chúng phải có cùng hashCode"**.

- Nếu bạn dùng Singleton, `hashCode` phải luôn giống nhau vì thực chất chỉ có một thực thể tồn tại.
    
- Nếu bạn dùng `HashMap` hoặc `HashSet`, `hashCode` sai sẽ khiến bạn không bao giờ tìm thấy dữ liệu đã lưu.