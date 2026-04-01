## Java Runtime Error (Lỗi khi chương trình đang chạy)

Trong Java, **Runtime Error** (Lỗi thực thi) là những lỗi xảy ra trong khi chương trình của bạn **đang chạy**, mặc dù trước đó nó đã vượt qua được quá trình biên dịch (Compile) một cách mượt mà không hề có lỗi cú pháp.

Nói một cách dễ hiểu: Trình biên dịch thấy code của bạn viết đúng ngữ pháp nên cho qua. Nhưng khi chạy thực tế, máy tính gặp phải một tình huống bất khả kháng hoặc phi lý dẫn đến việc chương trình bị "sập" (crash) giữa chừng.

---

### 1. Phân loại Runtime Error trong Java

Về mặt kỹ thuật, các lỗi xảy ra ở Runtime trong Java được chia thành 2 nhánh lớn (đều kế thừa từ lớp `Throwable`):

#### A. Unchecked Exception (Ngoại lệ không kiểm tra)

Đây là những lỗi thường xuất phát từ **sai sót trong logic lập trình** của con người. Trình biên dịch không ép bạn phải dùng `try-catch` cho những lỗi này, nhưng nếu chúng xảy ra, chương trình sẽ dừng hoạt động ngay lập tức.

- Tất cả các lớp này đều kế thừa từ lớp **`RuntimeException`**.
    

#### B. Error (Lỗi hệ thống nặng)

Đây là những lỗi nghiêm trọng liên quan đến **môi trường hệ thống hoặc phần cứng** (như hết bộ nhớ RAM, lỗi máy ảo JVM).

- Khác với Exception, khi một `Error` xảy ra, chương trình của bạn hầu như không thể tự cứu vãn hay xử lý bằng code được nữa.
    

---

### 2. Các "Gương Mặt" Runtime Error Kinh Điển Nhất

Dưới đây là danh sách những lỗi Runtime mà bất kỳ lập trình viên Java nào cũng từng "khóc thét" khi gặp phải:

#### 1. `NullPointerException` (Nỗi ám ảnh lớn nhất)

- **Nguyên nhân:** Xảy ra khi bạn cố gắng truy cập vào thuộc tính hoặc gọi phương thức của một đối tượng đang mang giá trị `null` (chưa được khởi tạo vùng nhớ).
    
- **Ví dụ:**
    
    Java
    
    ```
    String s = null;
    int doDai = s.length(); // BÙM! NullPointerException vì s là null
    ```
    

#### 2. `ArrayIndexOutOfBoundsException`

- **Nguyên nhân:** Bạn cố tình truy cập vào một vị trí (index) không tồn tại trong mảng (nhỏ hơn 0 hoặc lớn hơn/bằng độ dài của mảng).
    
- **Ví dụ:**
    
    Java
    
    ```
    int[] mang = {1, 2, 3}; // Mảng có 3 phần tử, index từ 0 đến 2
    int so = mang[5]; // BÙM! Index 5 không tồn tại
    ```
    

#### 3. `ArithmeticException`

- **Nguyên nhân:** Xảy ra khi có một phép toán lậu/vô lý trong toán học, phổ biến nhất là **chia một số nguyên cho 0**.
    
- **Ví dụ:**
    
    Java
    
    ```
    int ketQua = 10 / 0; // BÙM! Toán học không cho phép chia cho 0
    ```
    

#### 4. `ClassCastException`

- **Nguyên nhân:** Xảy ra khi bạn cố tình ép kiểu (cast) một đối tượng sang một kiểu dữ liệu mà nó không có quan hệ kế thừa.
    
- **Ví dụ:**
    
    Java
    
    ```
    Object x = "Đây là chuỗi";
    Integer y = (Integer) x; // BÙM! Chuỗi không thể biến thành Số nguyên
    ```
    

#### 5. `OutOfMemoryError` (Thuộc nhóm Error)

- **Nguyên nhân:** Xảy ra khi chương trình của bạn ngốn sạch bộ nhớ RAM được cấp phát cho máy ảo Java (JVM) và bộ dọn rác (Garbage Collector) không thể giải phóng thêm được nữa.
    

---

### 3. Cách "Sống Chung" và Khắc Phục Runtime Error

Khác với Compile Error (sửa xong mới chạy được), để đối phó với Runtime Error, bạn cần áp dụng các chiến thuật sau:

#### Cách 1: Phòng bệnh hơn chữa bệnh (Kiểm tra điều kiện)

Cách tốt nhất để xử lý `RuntimeException` là viết code cẩn thận để nó **không bao giờ xảy ra**.

Java

```
// Thay vì để mặc lỗi NullPointerException
if (s != null) {
    int doDai = s.length();
}

// Thay vì để mặc lỗi chia cho 0
if (mauSo != 0) {
    int thuong = tuSo / mauSo;
}
```

#### Cách 2: Bắt lỗi bằng khối `try-catch`

Nếu đó là những tình huống rủi ro không thể lường trước hết (hoặc khi thao tác với file, mạng), hãy bọc đoạn code đó lại để chương trình không bị sập đột ngột.

Java

```
try {
    int ketQua = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Đã xảy ra lỗi toán học nhưng chương trình vẫn chạy tiếp!");
}
```

---

### Bảng tóm tắt nhanh: Compile Error vs Runtime Error

|**Đặc điểm**|**Compile Error (Lỗi biên dịch)**|**Runtime Error (Lỗi thực thi)**|
|---|---|---|
|**Thời điểm**|Lúc bạn đang gõ code / bấm Build.|Lúc chương trình đang chạy dở.|
|**Nguyên nhân**|Sai cú pháp, thiếu dấu `;`, sai tên biến...|Lỗi logic, tràn bộ nhớ, chia cho 0...|
|**Hậu quả**|Không tạo ra được file `.class` để chạy.|Chương trình bị tắt/crash đột ngột.|

Bạn có đang gặp phải một chiếc `Exception` cụ thể nào trong lúc chạy code không? Cứ gửi đoạn code hoặc tên lỗi đó lên đây, mình sẽ "bắt bệnh" giúp cho nhé!