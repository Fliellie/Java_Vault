Trong Java, để lấy độ dài (số lượng ký tự) của một chuỗi, bạn dùng phương thức `.length()`.

Cực kỳ đơn giản như sau:

Java

```
String name = "Gemini";
int size = name.length(); 

System.out.println("Độ dài chuỗi là: " + size); // Kết quả sẽ là 6
```

## ⚠️ Lưu ý quan trọng để không bị "bug":

1. **Phân biệt với Mảng (Array):**
    
    - Với **String**, bạn phải có cặp ngoặc đơn: `s.length()` (đây là một phương thức).
        
    - Với **Mảng**, bạn KHÔNG dùng ngoặc đơn: `arr.length` (đây là một thuộc tính).
        
    - _Rất nhiều người mới học hay nhầm lẫn chỗ này, bạn cẩn thận nhé!_
        
2. **Chuỗi có khoảng trắng:**
    
    - Khoảng trắng cũng được tính là một ký tự. Ví dụ: `"Hoc Java"` có độ dài là 8.
        
3. **Lỗi NullPointerException:**
    
    - Nếu chuỗi của bạn chưa được khởi tạo (`String s = null;`), việc gọi `.length()` sẽ làm chương trình "văng" lỗi ngay lập tức. Hãy đảm bảo chuỗi không bị null trước khi check size.
        

---

## Một vài hàm liên quan nếu bạn đang xử lý chuỗi:

- **Kiểm tra chuỗi rỗng:** `s.isEmpty()` (Trả về `true` nếu length bằng 0).
    
- **Loại bỏ khoảng trắng thừa ở 2 đầu:** `s.trim().length()` (Hữu ích khi nhận dữ liệu từ người dùng).