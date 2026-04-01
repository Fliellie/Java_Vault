# Tường minh
```
public class HelloWorld{

     public static void main(String []args){
        int a = 5;
        long b = a;
        System.out.print(b);
     }
}

```

# Ngầm định
```
public class HelloWorld{

     public static void main(String []args){
        char a = 'A';
        int b = a;
        char c = (char) b;
        System.out.println(b);
        System.out.println(c);     
    }
}

```
# Ép sang String
Trong Java, việc ép kiểu (ép từ số sang chuỗi) rất phổ biến và có vài cách từ "ngắn gọn" đến "chuyên nghiệp". Tùy vào phong cách code của bạn mà chọn nhé:

### 1. Cách "Lười" nhất (Nhanh nhất)

Bạn chỉ cần cộng số `int` đó với một chuỗi rỗng `""`. Java sẽ tự hiểu và biến tất cả thành String.

Java

```
int number = 100;
String s = number + ""; 
```

- **Ưu điểm:** Viết cực nhanh, dễ nhớ.
    
- **Nhược điểm:** Hiệu năng thấp hơn một chút xíu (không đáng kể với bài tập nhỏ) vì Java phải tạo ra một đối tượng trung gian.
    

### 2. Cách "Chính thống" (Khuyên dùng)

Sử dụng hàm `String.valueOf()`. Đây là cách rõ ràng và tường minh nhất.

Java

```
int number = 100;
String s = String.valueOf(number);
```

- **Ưu điểm:** Đọc vào là hiểu ngay ý đồ code, hiệu năng tốt.
    

### 3. Cách dùng lớp bao (Wrapper Class)

Sử dụng `Integer.toString()`. Cách này cũng tương đương với cách 2.

Java

```
int number = 100;
String s = Integer.toString(number);
```

---
## Một mẹo nhỏ cho bạn:

Nếu bạn đang làm bài tập mà cần định dạng số (ví dụ: muốn số 5 biến thành chuỗi "05" hoặc định dạng tiền tệ), hãy dùng `String.format()`:

Java

```
int number = 5;
String s = String.format("%02d", number); // Kết quả sẽ là "05"
```