## 1. StringBuffer là gì?

Nếu `String` mang tính chất **bất biến (Immutable)** – tức là tạo ra rồi thì không sửa được, mỗi lần cộng chuỗi là sinh ra một vùng nhớ mới; thì `StringBuffer` lại mang tính chất **khả biến (Mutable)**.

Bạn có thể thêm, bớt, sửa, xóa nội dung thoải mái trên chính đối tượng `StringBuffer` đó mà **không hề tạo ra đối tượng rác** trong bộ nhớ.

**Đặc điểm "vàng" của StringBuffer:**

- **Có thể thay đổi (Mutable):** Thay đổi trực tiếp trên vùng nhớ của nó.
    
- **Đồng bộ hóa (Synchronized / Thread-safe):** Đây là điểm khác biệt lớn nhất của nó. Các phương thức của `StringBuffer` được đồng bộ hóa, nghĩa là an toàn khi sử dụng trong môi trường đa luồng (Multi-threading). Tại một thời điểm, chỉ có 1 luồng (thread) được phép thao tác với nó.
    

---

## 2. Các phương thức "chất lượng" nhất của StringBuffer

Vì sinh ra để "nhào nặn" chuỗi, `StringBuffer` cung cấp các phương thức thao tác rất mạnh mẽ mà `String` thông thường không có hoặc làm rất chậm:

|**Phương thức**|**Chức năng**|**Ví dụ & Kết quả**|
|---|---|---|
|`append(String s)`|Nối thêm chuỗi vào **cuối** chuỗi hiện tại.|`sb.append(" Java")` $\rightarrow$ `"Hello Java"`|
|`insert(int offset, String s)`|Chèn chuỗi `s` vào vị trí `offset` bất kỳ.|`sb.insert(5, " my")` $\rightarrow$ `"Hello my Java"`|
|`replace(int start, int end, String s)`|Thay thế đoạn chuỗi từ `start` đến `end-1` bằng chuỗi `s`.|`sb.replace(0, 5, "Hi")` $\rightarrow$ `"Hi my Java"`|
|`delete(int start, int end)`|Xóa đoạn ký tự từ `start` đến `end-1`.|`sb.delete(0, 3)` $\rightarrow$ `"my Java"`|
|`reverse()`|**Đảo ngược** toàn bộ chuỗi (rất hay gặp khi phỏng vấn).|`sb.reverse()` $\rightarrow$ `"avaJ ym"`|
|`capacity()`|Trả về sức chứa hiện tại của bộ đệm (mặc định khởi tạo là 16).|`sb.capacity()` $\rightarrow$ `16` (hoặc hơn nếu chuỗi dài)|

---

## 3. Code ví dụ thực tế

Dưới đây là cách bạn sử dụng `StringBuffer` trong thực tế:

Java

```java
public class StringBufferExample {
    public static void main(String[] args) {
        // Khởi tạo một StringBuffer
        StringBuffer sb = new StringBuffer("Java");
        
        // 1. Nối chuỗi
        sb.append(" is awesome");
        System.out.println(sb); // Kết quả: Java is awesome
        
        // 2. Chèn chuỗi
        sb.insert(4, " 21");
        System.out.println(sb); // Kết quả: Java 21 is awesome
        
        // 3. Xóa chuỗi (xóa chữ " 21")
        sb.delete(4, 7);
        System.out.println(sb); // Kết quả: Java is awesome
        
        // 4. Đảo ngược chuỗi
        StringBuffer sb2 = new StringBuffer("ABC");
        System.out.println(sb2.reverse()); // Kết quả: CBA
        
        // 5. Chuyển lại thành String thông thường khi đã xử lý xong
        String finalResult = sb.toString(); 
    }
}
```

---

## 4. Bảng so sánh "Sinh tử": String vs StringBuffer vs StringBuilder

Đi phỏng vấn Java kiểu gì bạn cũng sẽ gặp câu hỏi phân biệt 3 class này. Hãy nắm chắc bảng so sánh dưới đây:

|**Tiêu chí**|**String**|**StringBuffer**|**StringBuilder**|
|---|---|---|---|
|**Bản chất**|Bất biến (Immutable)|Khả biến (Mutable)|Khả biến (Mutable)|
|**An toàn luồng (Thread-safe)**|An toàn (vì không thể đổi)|**An toàn** (Synchronized)|**Không an toàn**|
|**Tốc độ (Hiệu năng)**|Rất chậm (nếu nối chuỗi nhiều)|Chậm hơn StringBuilder|**Nhanh nhất**|
|**Phiên bản Java**|Java 1.0|Java 1.0|Java 1.5|
|**Khi nào nên dùng?**|Khi nội dung chuỗi ít/không thay đổi.|Khi cần nối/sửa chuỗi liên tục trong môi trường **Đa luồng (Multi-thread)**.|Khi cần nối/sửa chuỗi liên tục trong **Đơn luồng (Single-thread)**.|

> **Tóm lại:** Nếu bạn đang viết một chương trình bình thường (đơn luồng) và cần nối chuỗi liên tục, hãy dùng **`StringBuilder`** vì nó nhanh nhất. Chỉ dùng **`StringBuffer`** khi bạn đang xử lý đa luồng (ví dụ nhiều user cùng truy cập và thay đổi một biến text cùng lúc) để tránh xung đột dữ liệu.