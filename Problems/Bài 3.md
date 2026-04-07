Bài này áp dụng **Factory Method Pattern** (Mẫu phương thức nhà máy). Mục tiêu tối thượng của bài này là giúp code của bạn linh hoạt: Sau này lỡ sếp có yêu cầu thêm kênh gửi tin nhắn qua Zalo, Telegram... bạn chỉ cần viết thêm class mới chứ **không cần sửa lại** code cũ đang chạy ổn định.

---

### 📝 Tóm tắt đề bài "Xưởng sản xuất thông báo"

Bạn cần xây dựng một hệ thống phân cấp gồm 3 phần chính:

1. **Chuẩn sản phẩm (`Notification`):** Quy định mọi loại tin nhắn đều phải có hành động "Gửi".
    
2. **Sản phẩm cụ thể (`EmailNotification`, `SmsNotification`):** Hiện thực hóa việc gửi bằng cách in ra màn hình.
    
3. **Xưởng sản xuất (`NotificationApp` và các xưởng con):** Nơi chứa máy tự động bấm nút gửi và các khuôn đúc tương ứng.
    
4. **Hàm `main`:** Đóng vai trò người vận hành, chọn xưởng và bấm nút.
    

---

### 🛠️ "Mớ đồ nghề" bạn cần dùng để giải bài này:

Bạn hãy chuẩn bị sẵn các từ khóa và cấu trúc sau trong Java để ráp nối nhé:

#### 1. Đồ nghề tạo "Sản phẩm" (Dùng Interface)

- **`interface Notification`**: Tạo một giao diện chung. Trong này chỉ cần khai báo một hàm trống: `void send(String msg);`.
    
- **`implements Notification`**: Dùng từ khóa này cho 2 class cụ thể là `EmailNotification` và `SmsNotification`. Bạn sẽ viết đè (`@Override`) lại hàm `send` để in ra dòng chữ thông báo tương ứng.
    

#### 2. Đồ nghề tạo "Xưởng sản xuất" (Dùng Abstract Class)

Đây là "trái tim" của Factory Method.

- **`abstract class NotificationApp`**: Tạo một lớp trừu tượng.
    
- **`protected abstract Notification createNotification();`**: Đây chính là "Khuôn đúc trừu tượng" (Factory Method). Lớp này không biết tạo ra cái gì nên để `abstract`.
    
- **Hàm điều khiển `notifyUser(String msg)`**: Hàm này nằm trong lớp `NotificationApp` nhưng **không** được khai báo `abstract`. Nó sẽ chạy như sau:
    
    Java
    
    ```
    public void notifyUser(String msg) {
        // Gọi "khuôn đúc" để lấy sản phẩm (dù chưa biết cụ thể là Email hay Sms)
        Notification notification = createNotification(); 
        // Bấm nút gửi
        notification.send(msg);
    }
    ```
    
- **Lớp con `EmailApp` và `SmsApp`**: Kế thừa (`extends`) từ `NotificationApp`. Ở đây bạn bắt buộc phải viết đè lại hàm `createNotification()` và trả về (`return`) đúng đối tượng `new EmailNotification()` hoặc `new SmsNotification()`.
    

#### 3. Đồ nghề chạy thử trong hàm `main`

Bạn sẽ giả định người dùng muốn dùng Email chẳng hạn:

Java

```
public static void main(String[] args) {
    // Khai báo kiểu trừu tượng nhưng khởi tạo bằng xưởng cụ thể
    NotificationApp app = new EmailApp(); 
    
    // Bấm nút gửi, hệ thống tự biết tạo ra Email và gửi đi
    app.notifyUser("Chào mừng bạn đến với VS Code!");
}
```

---