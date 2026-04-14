`java.time.LocalDate` là một lớp trong Java (được giới thiệu từ bản **Java 8**) dùng để đại diện cho một **ngày** (năm, tháng, ngày) nhưng **không bao gồm thông tin về thời gian** (giờ, phút, giây) và **không có múi giờ** (time zone).

Hãy coi nó giống như một tờ lịch trên tường: bạn biết hôm nay là ngày nào, nhưng không biết bây giờ là mấy giờ.

---

### Tại sao nên dùng `LocalDate`?

Trước Java 8, chúng ta thường dùng `java.util.Date`, nhưng nó có nhiều hạn chế: không trực quan, lỗi thời (mutable) và gây khó khăn khi xử lý múi giờ. `LocalDate` giải quyết được các vấn đề này vì nó **bất biến (immutable)** và an toàn hơn cho các ứng dụng hiện đại.

---

### Cách sử dụng cơ bản

Dưới đây là một số thao tác phổ biến nhất với `LocalDate`:

#### 1. Khởi tạo ngày tháng

Java

```
import java.time.LocalDate;
import java.time.Month;

// Lấy ngày hiện tại
LocalDate today = LocalDate.now();

// Tạo một ngày cụ thể (Ví dụ: 13/03/2026)
LocalDate myDate = LocalDate.of(2026, 3, 13);
// Hoặc dùng Enum Month
LocalDate anotherDate = LocalDate.of(2026, Month.MARCH, 13);

// Tạo từ chuỗi (định dạng mặc định ISO: YYYY-MM-DD)
LocalDate parsedDate = LocalDate.parse("2026-03-13");
```

#### 2. Thao tác với ngày tháng

Bạn có thể dễ dàng cộng trừ ngày, tháng, năm:

Java

```
LocalDate nextWeek = today.plusWeeks(1);
LocalDate lastMonth = today.minusMonths(1);
LocalDate nextYear = today.plusYears(1);
```

#### 3. Trích xuất thông tin

Java

```
int year = today.getYear();
Month month = today.getMonth();
int day = today.getDayOfMonth();
```

---

### Sơ đồ so sánh các thành phần thời gian trong Java 8+

Việc hiểu cách `LocalDate` nằm trong hệ thống `java.time` sẽ giúp bạn chọn đúng lớp khi cần:

- **`LocalDate`**: Chỉ Ngày (YYYY-MM-DD).
    
- **`LocalTime`**: Chỉ Giờ (HH:mm:ss).
    
- **`LocalDateTime`**: Ngày và Giờ (YYYY-MM-DD HH:mm:ss).
    
- **`ZonedDateTime`**: Ngày, Giờ và Múi giờ.
    

---

### Một vài lưu ý nhỏ:

- **Không dùng cho tính toán thời gian thực:** Nếu bạn cần đo khoảng thời gian (ví dụ: code này chạy mất bao lâu), hãy dùng `Instant` hoặc `Duration` thay vì `LocalDate`.
    
- **Immutable:** Mọi phương thức như `plusDays()` hay `minusMonths()` đều không thay đổi đối tượng cũ, mà nó trả về một **đối tượng `LocalDate` mới**. Hãy nhớ gán giá trị trả về vào một biến nhé!
# Other Function
Để tính khoảng cách giữa hai ngày trong Java (số ngày chênh lệch), cách chuẩn nhất và hiện đại nhất là sử dụng lớp **`ChronoUnit`** kết hợp với **`LocalDate`**.

Dưới đây là cách thực hiện:

### Sử dụng `ChronoUnit.DAYS.between`

Đây là cách đơn giản và dễ đọc nhất:

Java

```
import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class Main {
    public static void main(String[] args) {
        LocalDate date1 = LocalDate.of(2026, 3, 1);
        LocalDate date2 = LocalDate.of(2026, 3, 13);

        // Tính khoảng cách giữa date1 và date2
        long daysBetween = ChronoUnit.DAYS.between(date1, date2);

        System.out.println("Số ngày chênh lệch là: " + daysBetween);
    }
}
```

### Giải thích các lưu ý quan trọng:

1. **Thứ tự tham số:** `ChronoUnit.DAYS.between(start, end)` sẽ lấy ngày kết thúc trừ đi ngày bắt đầu.
    
    - Nếu `end` lớn hơn `start`, kết quả là **số dương**.
        
    - Nếu `end` nhỏ hơn `start`, kết quả là **số âm**.
        
2. **Kết quả trả về:** Phương thức này trả về kiểu dữ liệu `long`, rất an toàn nếu bạn tính khoảng cách giữa hai ngày cách nhau rất xa (nhiều năm).
    
3. **Tại sao không dùng phép trừ trực tiếp?** Vì `LocalDate` là đối tượng, bạn không thể dùng toán tử `-` như các số nguyên (`int`, `long`). Bạn phải dùng các phương thức tiện ích mà Java đã cung cấp sẵn trong gói `java.time.temporal`.
    

---

### Một cách khác: Sử dụng `Period`

Nếu bạn muốn biết khoảng cách chi tiết hơn (bao nhiêu năm, bao nhiêu tháng, bao nhiêu ngày), bạn có thể dùng lớp `Period`:

Java

```
import java.time.LocalDate;
import java.time.Period;

LocalDate date1 = LocalDate.of(2026, 1, 1);
LocalDate date2 = LocalDate.of(2026, 3, 13);

Period period = Period.between(date1, date2);

System.out.println("Khoảng cách: " + period.getMonths() + " tháng và " + period.getDays() + " ngày.");
```

_Lưu ý: `Period` chỉ tính theo đơn vị năm/tháng/ngày. Nếu bạn chỉ quan tâm đến tổng số ngày (không quan tâm tháng/năm), thì `ChronoUnit.DAYS.between` vẫn là lựa chọn tốt nhất._