Ngoại lệ (Exception) là những sự kiện bất thường xảy ra trong khi chương trình đang chạy (Runtime) làm gián đoạn luồng xử lý bình thường (ví dụ: chia cho 0, đọc một file không tồn tại, hoặc gọi một đối tượng bị `null`). Nếu không xử lý, chương trình của bạn sẽ bị "sập" (crash) ngay lập tức.

Để đối phó với ngoại lệ, Java cung cấp một bộ khung cấu trúc cực kỳ chặt chẽ gồm 5 từ khóa "vàng": **`try`**, **`catch`**, **`finally`**, **`throw`**, và **`throws`**.

---

## 1. Cấu trúc cốt lõi: `try - catch - finally`

Đây là "chiếc khiên" bảo vệ code của bạn phổ biến nhất. Hãy hình dung nó như việc bạn đi xe máy: `try` là đoạn đường bạn đi, `catch` là chiếc mũ bảo hiểm bảo vệ bạn khi ngã, và `finally` là việc dù có ngã hay không thì bạn vẫn phải về đến nhà.



```java
try {
    // 1. Khối Code có nguy cơ xảy ra lỗi
    int result = 10 / 0; // Sẽ gây ra ArithmeticException (chia cho 0)
    System.out.println("Dòng này sẽ không bao giờ được chạy!");
} 
catch (ArithmeticException e) {
    // 2. Khối Code xử lý khi lỗi xảy ra
    System.out.println("Lỗi rồi bạn ơi! Không thể chia cho số 0.");
    System.out.println("Chi tiết lỗi: " + e.getMessage());
} 
finally {
    // 3. Khối Code LUÔN LUÔN được thực thi (Dù có lỗi hay không)
    System.out.println("Khối finally luôn chạy để dọn dẹp tài nguyên (đóng file, ngắt kết nối DB...)");
}
```

### 🎯 Quy tắc hoạt động:

- **`try`**: Bắt buộc phải có. Chứa những dòng code "mạo hiểm".
    
- **`catch`**: Có thể có một hoặc nhiều khối `catch` để bắt các loại lỗi khác nhau. Nếu code trong `try` bình yên vô sự, khối `catch` bị bỏ qua.
    
- **`finally`**: Không bắt buộc, nhưng nếu có thì **kiểu gì nó cũng chạy** (kể cả khi trong `try` hoặc `catch` có lệnh `return`). Thường dùng để giải phóng bộ nhớ, đóng file, đóng cổng mạng.
    

---

## 2. Kỹ thuật "Bắt nhiều cá một lúc" (Multi-catch)

Trong thực tế, một đoạn code trong `try` có thể sinh ra nhiều loại lỗi khác nhau. Bạn có 2 cách để xử lý:

**Cách 1: Dùng nhiều khối `catch` (Từ cụ thể đến tổng quát)**

> **⚠️ Lưu ý tối quan trọng:** Bạn phải xếp các Exception con (cụ thể) lên trước, Exception cha (tổng quát như `Exception` hay `RuntimeException`) xuống dưới cùng. Nếu đảo ngược lại, Java sẽ báo lỗi vì "cá lớn" đã nuốt hết "cá bé" rồi!



```java
try {
    String s = null;
    System.out.println(s.length()); // Gây ra NullPointerException
} catch (NullPointerException e) {
    System.out.println("Chuỗi bị null rồi!");
} catch (Exception e) { // Exception cha bọc lót cuối cùng
    System.out.println("Có lỗi gì đó khác xảy ra!");
}
```

**Cách 2: Gộp chung các lỗi cùng cấp (Java 7 trở lên)** Nếu các lỗi có cách xử lý giống nhau, bạn có thể dùng dấu gạch đứng `|` để gộp chúng lại cho gọn sạch:

Java

```java
try {
    // Code mạo hiểm
} catch (NullPointerException | ArithmeticException e) {
    System.out.println("Gặp lỗi null hoặc lỗi toán học rồi!");
}
```

---

## 3. Cặp bài trùng: `throw` và `throws`

Hai từ khóa này nhìn rất giống nhau nhưng nhiệm vụ lại hoàn toàn khác nhau. Đây là điểm cực kỳ dễ gây lú lẫn cho người mới.

### 🔴 `throw` (Chủ động ném ra một lỗi)

Dùng để tự tay bạn tạo ra và "ném" một ngoại lệ ra ngoài khi dữ liệu không thỏa mãn logic do bạn tự quy định.

Java

```java
public void checkTuoi(int tuoi) {
    if (tuoi < 18) {
        // Chủ động ném lỗi nếu chưa đủ tuổi
        throw new ArithmeticException("Chưa đủ 18 tuổi, không cho vào!");
    }
    System.out.println("Chào mừng bạn!");
}
```

### 🟡 `throws` (Khai báo/Cảnh báo lỗi ở tên hàm)

Dùng ở phần khai báo tên phương thức (method) để **"đá quả bóng trách nhiệm"** lên cấp cao hơn. Nó báo hiệu cho ai gọi hàm này biết rằng: _"Hàm này có thể gây ra lỗi đấy nhé, nhớ dùng try-catch bọc lấy nó khi gọi!"_.

Java

```java
// Throws cảnh báo hàm này có thể văng ra IOException
public void docFile(String duongDan) throws IOException {
    FileReader file = new FileReader(duongDan); // Có thể gây lỗi nếu ko tìm thấy file
}
```

---

## 💎 Bonus: `try-with-resources` (Cực kỳ hiện đại)

Từ Java 7 trở đi, nếu bạn làm việc với các đối tượng cần đóng sau khi dùng (như đọc file, kết nối Database), thay vì hì hục viết `close()` trong khối `finally`, bạn hãy dùng `try-with-resources`. Java sẽ **tự động đóng** tài nguyên cho bạn cho dù có lỗi hay không!

Java

```java
// Khai báo tài nguyên ngay trong ngoặc tròn của try
try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    System.out.println("Lỗi đọc file!");
} 
// Không cần khối finally để đóng br nữa, Java tự lo!
```

Bạn muốn chúng ta thử thực hành viết một class "Ngoại lệ tự định nghĩa" (Custom Exception) để ứng dụng vào một bài toán thực tế (như kiểm tra số dư tài khoản ngân hàng không được âm), hay bạn muốn chuyển sang chủ đề tiếp theo?