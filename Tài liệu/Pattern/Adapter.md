Đây là một tình huống rất thực tế: **Yêu cầu của khách hàng thay đổi** (muốn dùng Paypal nhưng nhập số tiền đơn vị Cents thay vì Dollar).

Lúc này, bạn có hai lựa chọn: sửa lớp `PaypalProcessor` (vi phạm nguyên tắc Đóng/Mở) hoặc **tạo thêm một Adapter mới**. Cách chuyên nghiệp nhất chính là tạo thêm một Adapter để xử lý riêng đơn vị Cents cho Paypal.

Hãy xem cách chúng ta giải quyết mà không làm hỏng code cũ:

### 1. Giữ nguyên lớp Paypal cũ (Đơn vị Dollar)

Java

```java
// Lớp này đã chạy ổn định, không nên sửa trực tiếp
class PaypalProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Thanh toán qua Paypal: $" + amount);
    }
}
```

### 2. Tạo Adapter mới cho khách hàng dùng Cents

Chúng ta sẽ "bọc" cái `PaypalProcessor` cũ vào trong một cái Adapter mới để thực hiện phép chia:

$$Amount_{Dollar} = \frac{Amount_{Cents}}{100}$$

Java

```java
class PaypalCentsAdapter implements PaymentProcessor {
    private PaypalProcessor paypalProcessor;

    public PaypalCentsAdapter(PaypalProcessor paypalProcessor) {
        this.paypalProcessor = paypalProcessor;
    }

    @Override
    public void processPayment(double amountInCents) {
        // Khách hàng truyền vào 10000 (cents), chúng ta đổi thành 100.0 (dollars)
        double dollars = amountInCents / 100.0;
        
        System.out.println("[Adapter] Đang chuyển đổi " + amountInCents + " cents sang $" + dollars);
        
        // Tái sử dụng logic thanh toán của Paypal cũ
        paypalProcessor.processPayment(dollars);
    }
}
```

### 3. Cách sử dụng (Client)

Bây giờ, tùy vào việc khách hàng đưa vào đơn vị gì, bạn sẽ cấp cho họ "cục chuyển đổi" tương ứng.

Java

```java
public class Main {
    public static void main(String[] args) {
        // 1. Khách hàng bình thường (nhập $100)
        PaymentProcessor standardPaypal = new PaypalProcessor();
        standardPaypal.processPayment(100.0);

        System.out.println("---");

        // 2. Khách hàng dùng Cents (nhập 10000 cents)
        PaypalProcessor oldPaypal = new PaypalProcessor();
        PaymentProcessor centsAdapter = new PaypalCentsAdapter(oldPaypal);
        
        // Khách hàng vẫn gọi cùng một hàm processPayment
        centsAdapter.processPayment(10000.0); 
    }
}
```

---

### Tại sao làm cách này lại "xịn" hơn sửa thẳng vào lớp Paypal?

- **Tính kế thừa và bảo trì:** Nếu sếp bảo "Thôi khách hàng lại muốn dùng Dollar như cũ rồi", bạn chỉ cần vứt cái Adapter đi, code gốc `PaypalProcessor` vẫn nằm đó, sạch sẽ và không lỗi lầm.
    
- **Tránh Side-effects:** Nếu bạn sửa trực tiếp hàm `processPayment` trong `PaypalProcessor` để chia cho 100, tất cả những chỗ khác đang dùng Dollar sẽ bị trả về số tiền nhỏ đi 100 lần (thảm họa tài chính!).
    
- **Linh hoạt:** Bạn có thể tạo `StripeAdapter`, `PaypalCentsAdapter`, `VnPayVndAdapter`... Tất cả đều biến đổi về một chuẩn chung mà hệ thống của bạn yêu cầu.
    

**Tóm lại:** Adapter không chỉ dùng để kết nối các lớp khác nhau, mà còn dùng để **thay đổi/chuẩn hóa dữ liệu** trước khi đưa vào lớp cũ. Bạn thấy logic "chia 100" này đã giải quyết được vấn đề của khách hàng chưa?