## 1. Bản chất của String trong Java

Để làm chủ được `String`, bạn cần nắm vững 3 đặc điểm "vàng" sau:

- **Bất biến (Immutable):** Một khi đối tượng `String` đã được tạo ra, giá trị của nó **không thể bị thay đổi**. Khi bạn nối chuỗi hoặc thao tác trên chuỗi, Java thực chất đang tạo ra một chuỗi mới hoàn toàn chứ không sửa trên chuỗi cũ.
    
- **String Pool (Bộ nhớ đệm):** Để tiết kiệm bộ nhớ, Java duy trì một vùng nhớ đặc biệt gọi là String Pool. Khi bạn khai báo chuỗi theo kiểu literal (dùng dấu `""`), Java sẽ kiểm tra xem chuỗi đó đã có trong Pool chưa. Nếu có rồi, nó sẽ dùng lại chứ không tạo mới.
    
- **Không phải kiểu dữ liệu nguyên thủy:** `String` là một **lớp (Class)**, thuộc gói `java.lang`, không phải kiểu dữ liệu nguyên thủy như `int` hay `char`.
    

---

## 2. Cách khởi tạo String

Có hai cách chính để bạn tạo một chuỗi:

1. **Sử dụng String Literal (Khuyên dùng):**
    
    Java
    
    ```
    String s1 = "Hello"; // Lưu trong String Pool
    String s2 = "Hello"; // Trỏ cùng đến vùng nhớ với s1
    ```
    
2. **Sử dụng từ khóa `new`:**
    
    Java
    
    ```
    String s3 = new String("Hello"); // Tạo một đối tượng hoàn toàn mới trong bộ nhớ Heap
    ```
    

> **Lưu ý quan trọng:** Luôn dùng phương thức `.equals()` để so sánh nội dung 2 chuỗi, đừng dùng `==`. Toán tử `==` chỉ so sánh xem 2 biến có trỏ cùng vào một ô nhớ hay không thôi!

---

## 3. Các phương thức phổ biến trong lớp String

Dưới đây là bảng gom nhóm các phương thức cực kỳ hay dùng giúp bạn dễ tra cứu:

### 🛠️ Kiểm tra và Tìm kiếm

|**Phương thức**|**Ý nghĩa**|**Ví dụ**|
|---|---|---|
|`length()`|Trả về độ dài chuỗi|`"Hi".length()` $\rightarrow$ `2`|
|`charAt(int index)`|Lấy ký tự tại vị trí `index`|`"Java".charAt(1)` $\rightarrow$ `'a'`|
|`isEmpty()`|Kiểm tra chuỗi có rỗng không|`"".isEmpty()` $\rightarrow$ `true`|
|`contains(CharSequence s)`|Kiểm tra chuỗi có chứa chuỗi con không|`"Hello".contains("el")` $\rightarrow$ `true`|
|`startsWith(String prefix)`|Kiểm tra chuỗi bắt đầu bằng...|`"Java".startsWith("J")` $\rightarrow$ `true`|
|`indexOf(String str)`|Trả về vị trí đầu tiên tìm thấy chuỗi con|`"Hello".indexOf("l")` $\rightarrow$ `2`|

### ✂️ Cắt, Ghép và Biến đổi

|**Phương thức**|**Ý nghĩa**|**Ví dụ**|
|---|---|---|
|`substring(int begin)`|Cắt chuỗi từ vị trí `begin` đến hết|`"Hello".substring(2)` $\rightarrow$ `"llo"`|
|`toLowerCase()`|Chuyển thành chữ thường|`"JAVA".toLowerCase()` $\rightarrow$ `"java"`|
|`toUpperCase()`|Chuyển thành chữ hoa|`"java".toUpperCase()` $\rightarrow$ `"JAVA"`|
|`trim()`|Loại bỏ khoảng trắng thừa ở 2 đầu|`" hi ".trim()` $\rightarrow$ `"hi"`|
|`replace(char old, char new)`|Thay thế ký tự/chuỗi|`"cat".replace('c', 'b')` $\rightarrow$ `"bat"`|

### 🤝 So sánh và Tách chuỗi

|**Phương thức**|**Ý nghĩa**|**Ví dụ**|
|---|---|---|
|`equals(Object anObject)`|So sánh nội dung (có phân biệt hoa/thường)|`"A".equals("a")` $\rightarrow$ `false`|
|`equalsIgnoreCase(String s)`|So sánh nội dung (KHÔNG phân biệt hoa/thường)|`"A".equalsIgnoreCase("a")` $\rightarrow$ `true`|
|`split(String regex)`|Tách chuỗi thành mảng dựa vào ký tự phân cách|`"a,b".split(",")` $\rightarrow$ `["a", "b"]`|

---

## 💡 Mẹo nhỏ cho bạn: Khi nào nên dùng `StringBuilder`?

Vì `String` là bất biến, nên nếu bạn dùng vòng lặp để cộng chuỗi hàng nghìn lần (ví dụ: `s += i;`), Java sẽ phải tạo ra hàng nghìn đối tượng rác trong bộ nhớ, làm app chạy rất chậm.

Trong trường hợp cần thay đổi, cộng dồn chuỗi liên tục, bạn hãy chuyển sang dùng **`StringBuilder`** hoặc **`StringBuffer`** nhé! Chúng sinh ra để tối ưu việc này đấy.