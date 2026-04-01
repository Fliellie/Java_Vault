Khi bạn thực hiện **Ghi đè phương thức (Method Overriding)** trong Java, có những quy tắc cực kỳ nghiêm ngặt về việc khai báo ngoại lệ (`throws`). Nếu không nắm vững, bạn sẽ thấy trình biên dịch "mắng" mình liên tục bằng những đường kẻ đỏ dưới chân code.

Về cơ bản, Java tuân thủ nguyên tắc: **Lớp con không được "nguy hiểm" hơn lớp cha.**

---

### 1. Trường hợp Lớp cha KHÔNG khai báo Ngoại lệ

Nếu phương thức ở lớp cha không sử dụng từ khóa `throws` để khai báo bất kỳ **Checked Exception** nào, thì phương thức ghi đè ở lớp con:

- **KHÔNG ĐƯỢC PHÉP** khai báo thêm bất kỳ Checked Exception mới hoặc cao hơn nào (như `IOException`, `SQLException`).
    
- **ĐƯỢC PHÉP** khai báo các **Unchecked Exception** (Runtime Exceptions) như `ArithmeticException`, `NullPointerException`.
    

**Ví dụ:**

Java

```
class Cha {
    void lanhDao() { System.out.println("Cha đang làm việc..."); }
}

class Con extends Cha {
    @Override
    // void lanhDao() throws IOException { } // LỖI BIÊN DỊCH: Không được thêm Checked Exception
    void lanhDao() throws ArithmeticException { // OK: Unchecked Exception thì thoải mái
        System.out.println("Con làm việc nhưng có thể tính toán sai.");
    }
}
```

---

### 2. Trường hợp Lớp cha CÓ khai báo Ngoại lệ (Checked Exception)

Đây là nơi mọi thứ trở nên thú vị hơn. Nếu phương thức ở lớp cha có `throws` một Checked Exception, lớp con khi ghi đè có 3 lựa chọn:

1. **Khai báo cùng loại ngoại lệ** với lớp cha.
    
2. **Khai báo ngoại lệ con** của ngoại lệ ở lớp cha (Ví dụ: Cha ném `Exception`, con ném `IOException`).
    
3. **Không khai báo ngoại lệ nào cả** (Con "ngoan" hơn cha).
    

**⚠️ Điều cấm kỵ:** Lớp con **không được phép** khai báo một ngoại lệ "cha" (rộng hơn) hoặc một ngoại lệ hoàn toàn khác loại so với lớp cha.

**Ví dụ minh họa:**

Java

```
import java.io.*;

class Cha {
    void moFile() throws IOException { }
}

class ConNgoan extends Cha {
    @Override
    void moFile() throws FileNotFoundException { } // OK: FileNotFoundException là con của IOException
}

class ConSieuNgoan extends Cha {
    @Override
    void moFile() { } // OK: Không ném lỗi gì cả, càng tốt!
}

/* class ConHu extends Cha {
    @Override
    void moFile() throws Exception { } // LỖI BIÊN DỊCH: Exception lớn hơn IOException (Cha của cha)
}
*/
```

---

### 3. Tại sao Java lại khắt khe như vậy?

Hãy tưởng tượng bạn đang sử dụng **Tính đa hình (Polymorphism)**:

Java

```
Cha obj = new ConNgoan();
try {
    obj.moFile(); 
} catch (IOException e) {
    // Người dùng chỉ chuẩn bị tinh thần để bắt IOException
}
```

Nếu lớp `Con` được phép ném ra một cái `Exception` to đùng hoặc một cái lỗi lạ hoắc, thì khối `catch (IOException e)` mà bạn viết cho lớp `Cha` sẽ trở nên vô dụng, dẫn đến chương trình bị sập bất ngờ. Java ép lớp con phải "thu hẹp" phạm vi lỗi lại để đảm bảo tính an toàn cho code sử dụng đa hình.

---

### Bảng tóm tắt quy tắc vàng

|**Ngoại lệ ở Lớp Cha**|**Ngoại lệ ở Lớp Con (Khi ghi đè)**|**Trạng thái**|
|---|---|---|
|Không có|Unchecked Exception (`RuntimeException`)|**Hợp lệ**|
|Không có|Checked Exception (`IOException`...)|**LỖI**|
|`IOException`|Không có|**Hợp lệ**|
|`IOException`|`FileNotFoundException` (Lớp con)|**Hợp lệ**|
|`IOException`|`Exception` (Lớp cha của lỗi)|**LỖI**|
|`IOException`|`SQLException` (Khác loại)|**LỖI**|

---

### Một lưu ý nhỏ về Unchecked Exception

Với các lỗi như `NullPointerException` hay `ArrayIndexOutOfBoundsException`, Java cực kỳ thoải mái. Bạn có thể thêm, bớt hoặc thay đổi chúng ở phương thức ghi đè tùy ý mà không gặp bất cứ trở ngại nào từ trình biên dịch.

Bạn đã bao giờ gặp trường hợp muốn ghi đè một phương thức nhưng lại lúng túng vì nó bắt buộc phải xử lý một `IOException` mà bạn không muốn chưa? Đừng ngần ngại chia sẻ đoạn code đó nhé!