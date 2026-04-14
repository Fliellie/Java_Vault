Trong Java, `String` là một **đối tượng (Object)**, không phải là một mảng ký tự thuần túy. Vì vậy, bạn không thể dùng cú pháp `s[0]` như trong C++ hay Python. Nếu bạn viết `s[0]`, Java sẽ báo lỗi ngay lập tức.

Để truy cập ký tự tại một chỉ số (index), bạn phải dùng phương thức **`.charAt(index)`**.

## 1. Cách dùng đúng trong Java

Chỉ số (index) vẫn bắt đầu từ **0** đến **length() - 1**.

Java

```
String s = "Gemini";

// Cách SAI (Lỗi biên dịch)
// char c = s[0]; 

// Cách ĐÚNG
char c = s.charAt(0); // Kết quả: 'G'
char last = s.charAt(s.length() - 1); // Kết quả: 'i'
```

## 2. Tại sao Java lại làm khó thế?

Lý do chính là vì tính **Bất biến (Immutable)** của String trong Java.

- Java muốn bảo vệ nội dung chuỗi không bị thay đổi tùy tiện.
    
- Nếu cho phép `s[0] = 'X'`, bạn sẽ làm hỏng tính chất bất biến này. Việc bắt dùng hàm `.charAt()` giúp Java kiểm soát dữ liệu tốt hơn.
    

## 3. Nếu bạn vẫn muốn dùng chỉ số `[]` thì sao?

Bạn có thể "lách luật" bằng cách biến chuỗi thành một mảng ký tự thực thụ:

Java

```
String s = "Java";
char[] chars = s.toCharArray();

// Bây giờ thì dùng thoải mái như mảng
System.out.println(chars[0]); // Kết quả: 'J'
chars[0] = 'L'; // Có thể thay đổi được luôn (trên mảng chars)
```

---

## Cảnh báo "văng" lỗi (Exception)

Giống như khi làm việc với mảng, nếu bạn truyền vào một chỉ số âm hoặc lớn hơn hoặc bằng `length()`, Java sẽ ném ra lỗi kinh điển:

> `StringIndexOutOfBoundsException`

**Mẹo cho bạn:** Trước khi gọi `.charAt(i)`, hãy luôn đảm bảo `i < s.length()` để chương trình không bị "sập" giữa chừng nhé!