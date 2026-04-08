### 1. Lỗi Re-ordering (Tái sắp xếp lệnh) là gì?

Khi bạn viết code, bạn kỳ vọng máy tính sẽ thực thi lần lượt từ trên xuống dưới. Tuy nhiên, trong thực tế, **Trình biên dịch (Compiler)** và **Bộ vi xử lý (CPU)** lại cực kỳ "tối ưu hóa".

Để tăng hiệu suất, chúng có quyền thay đổi thứ tự thực thi các câu lệnh của bạn, miễn là kết quả cuối cùng (trong một luồng duy nhất) không bị thay đổi.

**Vấn đề xảy ra khi chạy Đa luồng (Multi-threading):**

Hãy xem xét ví dụ giả mã (pseudo-code) sau với 2 luồng chạy song song:

Java 

```java
int a = 0;
boolean flag = false;

// Luồng 1 (Thread 1)
a = 1;           // Lệnh (1)
flag = true;     // Lệnh (2)

// Luồng 2 (Thread 2)
if (flag == true) {   // Lệnh (3)
    System.out.println(a); // Lệnh (4)
}
```

- **Theo logic thông thường:** Luồng 1 gán `a = 1`, sau đó bật cờ `flag = true`. Luồng 2 thấy cờ đã bật, liền in ra giá trị của `a`. Bạn chắc màn hình sẽ in ra `1`.
    
- **Thực tế với Re-ordering:** Compiler hoặc CPU thấy rằng Lệnh (1) và Lệnh (2) không phụ thuộc vào nhau (thay đổi `a` không ảnh hưởng đến `flag`). Do đó, để tối ưu, nó có thể chạy Lệnh (2) _trước_ Lệnh (1).
    
    - Kịch bản lỗi: Luồng 1 vừa chạy xong `flag = true` (nhưng chưa gán `a = 1`), thì CPU chuyển ngữ cảnh sang Luồng 2.
        
    - Luồng 2 kiểm tra thấy `flag` đã là `true`, nó chạy Lệnh (4) và in ra giá trị của `a`. Lúc này `a` vẫn bằng `0`.
        
    - **Hậu quả:** Chương trình in ra `0` thay vì `1`, dẫn đến lỗi logic nghiêm trọng và rất khó debug (vì nó thỉnh thoảng mới xảy ra).
        

### 2. Từ khóa `volatile` giải quyết vấn đề như thế nào?

Trong các ngôn ngữ như Java, khi bạn đánh dấu một biến là `volatile`, bạn đang ra lệnh cho JVM và CPU thực hiện hai quy tắc nghiêm ngặt:

**Quy tắc 1: Đảm bảo tính hiển thị (Visibility)** Mỗi CPU core thường có một bộ nhớ Cache riêng (L1, L2) để chạy cho nhanh. Đôi khi biến thay đổi ở core này nhưng core khác không biết. Biến `volatile` buộc CPU không được dùng Cache mà phải đọc/ghi trực tiếp vào Main Memory (RAM). Nếu một luồng thay đổi giá trị, tất cả các luồng khác sẽ ngay lập tức nhìn thấy giá trị mới đó.

**Quy tắc 2: Ngăn chặn Re-ordering (Memory Barrier - Hàng rào bộ nhớ)** Đây chính là "khắc tinh" của lỗi bạn thấy trong ảnh. Khi một biến được khai báo là `volatile`, hệ thống sẽ thiết lập một rào cản bộ nhớ.

- Bất kỳ lệnh đọc/ghi nào nằm **trước** lệnh ghi vào biến `volatile` đều không được phép sắp xếp lại để nằm **sau** nó.
    
- Bất kỳ lệnh đọc/ghi nào nằm **sau** lệnh đọc biến `volatile` đều không được phép đưa lên **trước** nó.
    

**Quay lại ví dụ trên, nếu ta sửa đổi:**

Java

```java
int a = 0;
volatile boolean flag = false; // Đã thêm volatile
```

Lúc này, Lệnh (1) `a = 1` bắt buộc phải được thực thi xong xuôi và ghi vào Main Memory thì mới đến lượt Lệnh (2) `flag = true` được thực thi. Luồng 2 sẽ luôn in ra kết quả là `1`, loại bỏ hoàn toàn lỗi re-ordering.

**Tóm lại:** Lỗi re-ordering là "tác dụng phụ" của việc máy tính cố gắng tối ưu hóa tốc độ chạy code, gây sai lệch logic khi nhiều luồng cùng thao tác trên các biến chung. Từ khóa `volatile` chính là chiếc phanh hãm sự tối ưu hóa "quá đà" đó lại, giúp đảm bảo thứ tự thực thi và tính đồng nhất dữ liệu trên bộ nhớ.