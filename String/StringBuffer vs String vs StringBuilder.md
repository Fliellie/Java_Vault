## Phân biệt String, StringBuffer và StringBuilder trong Java

Trong Java, cả ba lớp **`String`**, **`StringBuffer`**, và **`StringBuilder`** đều được sử dụng để lưu trữ và xử lý chuỗi ký tự (text). Tuy nhiên, chúng có những khác biệt cốt lõi về **tính bất biến (mutability)** và **tính an toàn luồng (thread-safety)**, dẫn đến hiệu năng sử dụng hoàn toàn khác nhau.

Dưới đây là phân tích chi tiết từng lớp:

### 1. Lớp `String` (Kẻ Bất Biến)

- **Tính bất biến (Immutable):** Đây là đặc điểm quan trọng nhất của `String`. Một khi đối tượng `String` được tạo ra trong bộ nhớ (đặc biệt là trong vùng String Pool), giá trị của nó **không thể bị thay đổi**.
    
- **Chuyện gì xảy ra khi bạn nối chuỗi?** Khi bạn dùng toán tử `+` hoặc hàm `concat()` để nối hoặc sửa một `String`, Java thực chất KHÔNG sửa đối tượng cũ. Thay vào đó, nó tạo ra một **đối tượng `String` hoàn toàn mới** trong bộ nhớ và bỏ lại đối tượng cũ (trở thành rác để Garbage Collector đi dọn).
    
- **Hiệu suất:** Rất chậm và tốn bộ nhớ nếu bạn phải nối chuỗi liên tục (ví dụ: nối chuỗi trong một vòng lặp hàng ngàn lần).
    
- **Khi nào nên dùng:** Khi chuỗi của bạn mang tính cố định, ít bị thay đổi (ví dụ: lưu URL, tên cấu hình, mật khẩu, text in ra màn hình).
    

### 2. Lớp `StringBuffer` (Kẻ Thay Đổi & An Toàn Luồng)

- **Có thể thay đổi (Mutable):** Khác với `String`, đối tượng `StringBuffer` giống như một cái túi có thể co giãn. Khi bạn dùng hàm `append()` để nối chuỗi, nó sẽ nhét thêm dữ liệu trực tiếp vào đối tượng hiện tại thay vì tạo ra đối tượng mới. Điều này giúp tiết kiệm bộ nhớ cực kỳ hiệu quả.
    
- **An toàn luồng (Thread-safe / Synchronized):** Tất cả các phương thức quan trọng của `StringBuffer` đều được đánh dấu bằng từ khóa `synchronized`. Điều này có nghĩa là nếu có nhiều luồng (threads) cùng lúc muốn sửa cái túi `StringBuffer` này, chúng phải xếp hàng chờ nhau.
    
- **Hiệu suất:** Nhanh hơn `String` rất nhiều khi thao tác nối/sửa chuỗi, nhưng chậm hơn `StringBuilder` vì hệ thống phải tốn chi phí quản lý việc "xếp hàng" của các luồng (khóa đồng bộ).
    
- **Khi nào nên dùng:** Khi bạn cần thao tác nối chuỗi liên tục trong môi trường **Đa luồng (Multi-threading)**.
    

### 3. Lớp `StringBuilder` (Kẻ Tốc Độ Bàn Thờ)

Được ra mắt từ Java 1.5 để khắc phục nhược điểm tốc độ của `StringBuffer`.

- **Có thể thay đổi (Mutable):** Giống hệt `StringBuffer`. Bạn có thể thoải mái thêm, xóa, sửa ký tự bên trong đối tượng hiện tại.
    
- **KHÔNG An toàn luồng (Non-synchronized):** `StringBuilder` gỡ bỏ hoàn toàn cơ chế `synchronized` (xếp hàng). Các luồng có thể nhảy vào sửa dữ liệu cùng lúc.
    
- **Hiệu suất:** **Nhanh nhất** trong cả 3 lớp. Do không phải bận tâm đến việc khóa/mở khóa đồng bộ cho các luồng, `StringBuilder` xử lý các thao tác nối chuỗi với tốc độ tối đa.
    
- **Khi nào nên dùng:** Đây là lựa chọn **mặc định và phổ biến nhất** cho 99% các trường hợp cần nối/tạo chuỗi phức tạp, thao tác trong vòng lặp ở môi trường **Đơn luồng (Single-thread)**.
    

---

### Bảng So Sánh Nhanh

|**Đặc điểm**|**String**|**StringBuffer**|**StringBuilder**|
|---|---|---|---|
|**Khả năng thay đổi**|Bất biến (Immutable)|Có thể thay đổi (Mutable)|Có thể thay đổi (Mutable)|
|**An toàn luồng**|Có (Do bất biến)|Có (Synchronized)|**Không**|
|**Hiệu suất (Nối chuỗi)**|Rất Chậm|Nhanh|**Cực Kỳ Nhanh**|
|**Sử dụng bộ nhớ**|Tốn kém (Tạo nhiều rác)|Tiết kiệm|Tiết kiệm|
|**Phiên bản Java**|Java 1.0|Java 1.0|Java 1.5|

---

### Ví dụ minh họa hiệu năng

Hãy xem chuyện gì xảy ra nếu chúng ta nối chuỗi 10,000 lần:

Java

```java

public class SoSanhChuoi {
    public static void main(String[] args) {
        int soLanLap = 10000;

        // 1. Dùng String (Tạo ra 10,000 đối tượng rác trong bộ nhớ)
        long thoiGianBatDau = System.currentTimeMillis();
        String s = "";
        for (int i = 0; i < soLanLap; i++) {
            s += "Java"; 
        }
        System.out.println("String tốn: " + (System.currentTimeMillis() - thoiGianBatDau) + " ms");

        // 2. Dùng StringBuffer (Chỉ dùng 1 đối tượng, nhưng có khóa đồng bộ)
        thoiGianBatDau = System.currentTimeMillis();
        StringBuffer sBuffer = new StringBuffer();
        for (int i = 0; i < soLanLap; i++) {
            sBuffer.append("Java");
        }
        System.out.println("StringBuffer tốn: " + (System.currentTimeMillis() - thoiGianBatDau) + " ms");

        // 3. Dùng StringBuilder (Chỉ dùng 1 đối tượng, không khóa)
        thoiGianBatDau = System.currentTimeMillis();
        StringBuilder sBuilder = new StringBuilder();
        for (int i = 0; i < soLanLap; i++) {
            sBuilder.append("Java");
        }
        System.out.println("StringBuilder tốn: " + (System.currentTimeMillis() - thoiGianBatDau) + " ms");
    }
}
```

**Kết quả thực tế (tương đối):**

- `String` tốn khoảng: **150 ms** (chậm nhất)
    
- `StringBuffer` tốn khoảng: **2 ms**
    
- `StringBuilder` tốn khoảng: **0 ms** (nhanh đến mức mili-giây không đo được)
    

**Lời khuyên:** Trong công việc hàng ngày, hãy luôn ưu tiên sử dụng `StringBuilder` khi bạn cần xây dựng hoặc nối chuỗi từ nhiều mảnh nhỏ (đặc biệt là trong vòng lặp). Chỉ dùng `StringBuffer` khi bạn thực sự code ứng dụng đa luồng (Multi-threading). Dùng `String` khi bạn chỉ cần lưu một chuỗi text ngắn gọn không bị thay đổi.