`LocalDateTime` là một class cực kỳ hữu ích thuộc gói `java.time` (được giới thiệu từ Java 8). Nó dùng để đại diện cho cả ngày và giờ nhưng **không chứa thông tin về múi giờ (time zone)**.

Class này khắc phục được hầu hết các nhược điểm của các class cũ như `java.util.Date` hay `Calendar`, đặc biệt là tính bất biến (immutable) và an toàn trong môi trường đa luồng (thread-safe).

Dưới đây là tổng hợp các hàm (phương thức) thường được sử dụng nhất và ví dụ code chi tiết dành cho bạn.

## 1. Các nhóm hàm hay sử dụng nhất

**Nhóm khởi tạo (Tạo đối tượng):**

- `LocalDateTime.now()`: Lấy ngày giờ hiện tại của hệ thống.
    
- `LocalDateTime.of(year, month, day, hour, minute, second)`: Tạo một ngày giờ cụ thể theo tham số truyền vào.
    
- `LocalDateTime.parse(text, formatter)`: Chuyển đổi một chuỗi (String) thành đối tượng `LocalDateTime`.
    

**Nhóm lấy thông tin (Getters):**

- `getYear()`, `getMonthValue()`, `getDayOfMonth()`: Lấy năm, tháng (dạng số), ngày trong tháng.
    
- `getHour()`, `getMinute()`, `getSecond()`: Lấy giờ, phút, giây.
    
- `getDayOfWeek()`: Lấy thứ trong tuần (Monday, Tuesday...).
    

**Nhóm tính toán thời gian (Cộng/Trừ):** Vì `LocalDateTime` là bất biến (immutable), các hàm này không làm thay đổi đối tượng gốc mà sẽ trả về một đối tượng `LocalDateTime` **mới**.

- `plusDays()`, `plusMonths()`, `plusYears()`, `plusHours()`...: Cộng thêm ngày, tháng, năm, giờ...
    
- `minusDays()`, `minusMonths()`, `minusHours()`...: Trừ đi ngày, tháng, giờ...
    

**Nhóm so sánh:**

- `isBefore(otherDate)`: Kiểm tra xem thời gian hiện tại có trước thời gian `otherDate` không.
    
- `isAfter(otherDate)`: Kiểm tra xem thời gian hiện tại có sau thời gian `otherDate` không.
    
- `isEqual(otherDate)`: Kiểm tra hai thời điểm có bằng nhau không.
    

**Nhóm định dạng (Formatting):**

- `format(DateTimeFormatter)`: Chuyển đổi đối tượng `LocalDateTime` thành chuỗi (String) theo định dạng mong muốn (ví dụ: dd/MM/yyyy HH:mm:ss).
    

---

## 2. Ví dụ sử dụng bằng Code Java

Dưới đây là một đoạn code tổng hợp cách sử dụng các hàm trên để bạn dễ hình dung:

Java

```java

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class LocalDateTimeExample {
    public static void main(String[] args) {
        
        // 1. Lấy ngày giờ hiện tại
        LocalDateTime now = LocalDateTime.now();
        System.out.println("Ngày giờ hiện tại: " + now);

        // 2. Tạo một ngày giờ cụ thể (VD: 20:30:00 ngày 15/08/2023)
        LocalDateTime specificDate = LocalDateTime.of(2023, 8, 15, 20, 30, 0);
        System.out.println("Ngày giờ cụ thể: " + specificDate);

        // 3. Lấy ra các thành phần (Năm, Tháng, Ngày, Giờ...)
        int year = now.getYear();
        int month = now.getMonthValue();
        int day = now.getDayOfMonth();
        int hour = now.getHour();
        System.out.println(String.format("Hôm nay là ngày %d, tháng %d, năm %d. Lúc %d giờ.", day, month, year, hour));

        // 4. Cộng/Trừ thời gian (VD: Thêm 1 tuần, trừ đi 2 tiếng)
        LocalDateTime nextWeek = now.plusWeeks(1);
        LocalDateTime twoHoursAgo = now.minusHours(2);
        System.out.println("Một tuần sau sẽ là: " + nextWeek);
        System.out.println("Hai tiếng trước là: " + twoHoursAgo);

        // 5. So sánh hai mốc thời gian
        if (now.isAfter(specificDate)) {
            System.out.println("Thời điểm hiện tại xảy ra SAU thời điểm cụ thể (15/08/2023).");
        }

        // 6. Định dạng (Format) LocalDateTime sang String theo ý muốn
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");
        String formattedNow = now.format(formatter);
        System.out.println("Ngày giờ đã định dạng: " + formattedNow);

        // 7. Chuyển đổi từ String (Parse) sang LocalDateTime
        String strDate = "27/03/2026 11:30:00";
        LocalDateTime parsedDate = LocalDateTime.parse(strDate, formatter);
        System.out.println("Ngày giờ sau khi parse từ String: " + parsedDate);
    }
}
```

> **Lưu ý nhỏ:** Nếu dự án của bạn cần xử lý thời gian có liên quan đến các quốc gia khác nhau hoặc cần lưu trữ thời gian toàn cầu thống nhất, bạn nên tìm hiểu thêm về `ZonedDateTime` hoặc `Instant` nhé, vì `LocalDateTime` không quan tâm bạn đang ở múi giờ nào.