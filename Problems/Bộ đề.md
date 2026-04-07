## Bài 1: Mẫu Composite (Cây thư mục)

> 💡 **Bản chất:** Bạn muốn quản lý một cây thư mục. Trong thư mục thì có file, có shortcut và... có cả thư mục con khác. Mẫu Composite giúp bạn coi "thư mục" và "file" bình đẳng như nhau, đều là "vật thể trong hệ thống file".

### 📝 Tóm tắt đề bài "Cây gia phả thư mục"

Bạn cần tạo ra một cấu trúc cây sao cho một lệnh `print` từ thư mục gốc sẽ tự động gọi lệnh `print` của toàn bộ các file, shortcut và thư mục con nằm bên trong nó.

### 🛠️ "Mớ đồ nghề" cần dùng:

- **Interface `FileSystemItem`**: Khai báo hàm `void print(String indent);`.
    
- **Class `FileItem`**: Có `name`, `size`. Hàm `print` chỉ cần in ra tên và size.
    
- **Class `Shortcut`**: Có `name` và một biến `FileSystemItem target`. Hàm `print` sẽ in ra tên shortcut và gọi `target.getPath()` (bạn có thể viết thêm một hàm lấy đường dẫn logic).
    
- **Class `Folder`**: Có `name` và một danh sách `List<FileSystemItem> children = new ArrayList<>();`.
    
    - Hàm `print(String indent)` của `Folder` sẽ in tên nó ra trước, sau đó dùng vòng lặp `for` duyệt qua từng `item` trong danh sách `children` và gọi `item.print(indent + " ")` (cộng thêm khoảng trắng để thụt lề).
        

---

## Bài 2: Mẫu Decorator (Bọc bánh bông lan)

> 💡 **Bản chất:** Bạn có một chiếc bánh bông lan (Email). Bạn muốn rắc thêm phô mai (SMS) và thêm dâu tây (Facebook). Mỗi lần bọc thêm một lớp, chiếc bánh lại có thêm tính năng mà không làm hỏng lõi bánh bên trong.

### 📝 Tóm tắt đề bài "Gửi tin đa kênh"

Mặc định hệ thống chỉ gửi Email. Nhưng bạn muốn linh hoạt ghép thêm tính năng gửi SMS hoặc Facebook vào mà không cần sửa code cũ.

### 🛠️ "Mớ đồ nghề" cần dùng:

- **Interface `Notifier`**: Có hàm `void send(String msg);`.
    
- **Class `EmailNotifier` implements `Notifier`**: Đây là cái lõi. Hàm `send` in ra: `"Gửi Email: [msg]"`.
    
- **Abstract class `NotifierDecorator` implements `Notifier`**: Đây là cái vỏ bọc. Nó giữ một biến `protected Notifier wrappedNotifier;`. Trong hàm `send`, nó chỉ làm nhiệm vụ chuyển tiếp: `wrappedNotifier.send(msg);`.
    
- **Các lớp Decorator cụ thể (`SmsNotifier`, `FacebookNotifier`)**: Kế thừa từ `NotifierDecorator`. Trong hàm `send`, bạn vừa gọi `super.send(msg)` (để lớp bọc bên trong chạy), vừa in thêm dòng chữ gửi SMS hoặc Facebook của riêng nó.
    

---

## Bài 3: Nguyên lý OCP & SRP (Định dạng báo cáo)

> 💡 **Bản chất:** Code cũ dùng `if-else` để check loại file (JSON, XML). Sau này sếp đòi thêm file Excel, bạn lại phải vào sửa cái hàm đó $\rightarrow$ Vi phạm nguyên lý OCP (Đóng để sửa đổi, Mở để mở rộng).

### 📝 Tóm tắt đề bài "Thuê thợ chuyên môn"

Hàm `ReportService` không tự làm nhiệm vụ đổi định dạng nữa. Nó sẽ thuê một ông thợ (Formatter) làm việc đó.

### 🛠️ "Mớ đồ nghề" cần dùng:

- **Class `Report`**: Có `title`, `content`.
    
- **Interface `ReportFormatter`**: Khai báo hàm `String format(Report report);`.
    
- **Các class con**: `JsonFormatter` và `XmlFormatter` sẽ hiện thực hóa hàm `format` trả về chuỗi JSON hoặc XML tương ứng.
    
- **Class `ReportService`**: Không dùng `if-else` nữa. Khai báo `private ReportFormatter formatter;` nạp qua constructor. Hàm `export` chỉ cần đúng 1 dòng: `return formatter.format(data);`.
    

---

## Bài 4: Nguyên lý ISP & DIP (Trình phát đa phương tiện)

> 💡 **Bản chất:**
> 
> - **ISP (Tách interface):** Đừng bắt cá phải biết bay. Thiết bị chỉ phát nhạc thì đừng bắt nó implement cái interface có cả hàm phát video.
>     
> - **DIP (Đảo ngược phụ thuộc):** Thằng lớn (`MediaPlayer`) không được phụ thuộc trực tiếp vào thằng nhỏ (`AudioPlayer`). Cả hai phải nhìn nhau qua một "bản hợp đồng" (Interface).
>     

### 📝 Tóm tắt đề bài "Lắp ráp module"

Lớp `MediaPlayer` muốn phát gì thì phát, nhưng nó không được tự ý `new` các đầu phát. Nó phải đợi người ta đưa đầu phát vào từ bên ngoài (Constructor Injection).

### 🛠️ "Mớ đồ nghề" cần dùng:

- **Interface `AudioPlayable`**: Có hàm `playAudio`.
    
- **Interface `VideoPlayable`**: Có hàm `playVideo`.
    
- **Class `AudioPlayer`**: Chỉ `implements AudioPlayable`.
    
- **Class `VideoPlayer`**: Chỉ `implements VideoPlayable`.
    
- **Class `MediaPlayer`**: Khai báo 2 biến interface `AudioPlayable` và `VideoPlayable`. Nhận giá trị qua constructor và gọi hàm phát tương ứng.
    

---

## Bài 5: Mẫu Singleton (Hệ thống ghi Log)

> 💡 **Bản chất:** Giống hệt bài Singleton ở lượt trước nhưng đề bài này yêu cầu cụ thể hơn về cách hiển thị log `[INFO]` và `[ERROR]`.

### 🛠️ "Mớ đồ nghề" cần dùng:

- **Class `Logger`**: Constructor private, biến `instance` tĩnh.
    
- Hàm `getInstance()` dùng kỹ thuật Lazy Initialization (khởi tạo lười).
    
- Hai hàm `logInfo` và `logError` dùng `System.out.println` để in ra màn hình đúng định dạng đề bài yêu cầu.