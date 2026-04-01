### 1. Tính Đóng Gói (Encapsulation) – "Cái Vỏ Máy"

Tính đóng gói giống như cái vỏ điện thoại bao bọc các linh kiện bên trong.

- **Ý nghĩa:** Bạn chỉ có thể tương tác với điện thoại qua màn hình và nút bấm (các phương thức `public`). Bạn không thể chạm trực tiếp vào pin hay chip (các thuộc tính `private`) để thay đổi điện áp tùy tiện.
    
- **Mục đích:** Giữ cho dữ liệu an toàn, tránh việc code bên ngoài can thiệp làm hỏng đối tượng. Bạn muốn đổi âm lượng? Phải dùng nút bấm (`setter`), không được tháo máy ra vặn biến trở.
    

### 2. Tính Trừu Tượng (Abstraction) – "Bảng Điều Khiển"

Tính trừu tượng là việc chỉ hiện ra những thứ người dùng cần, còn cách nó chạy thế nào thì giấu đi.

- **Ý nghĩa:** Khi bạn nhấn nút "Gọi điện", bạn chỉ cần biết nó sẽ thực hiện cuộc gọi. Bạn không cần biết sóng radio được mã hóa thế nào hay tháp viễn thông nào đang nhận tín hiệu.
    
- **Mục đích:** Giúp lập trình viên tập trung vào **"làm cái gì"** thay vì sa đà vào sự phức tạp của **"làm thế nào"**.
    

### 3. Tính Kế Thừa (Inheritance) – "Đời Máy Sau"

Tính kế thừa cho phép tạo ra lớp mới dựa trên lớp cũ để tận dụng những gì đã có.

- **Ý nghĩa:** Hãng sản xuất ra iPhone 15 (lớp cha). Khi làm iPhone 16 (lớp con), họ không cần thiết kế lại từ đầu cái loa, cái mic hay màn hình. Họ kế thừa toàn bộ từ đời 15 và chỉ việc nâng cấp chip hoặc thêm camera mới.
    
- **Mục đích:** Tái sử dụng code, giúp chương trình gọn nhẹ và dễ phát triển thêm tính năng mới mà không phải viết lại từ đầu.
    

### 4. Tính Đa Hình (Polymorphism) – "Một Nút Bấm, Nhiều Tác Dụng"

Đa hình là khả năng một hành động có thể thực hiện theo nhiều cách khác nhau tùy vào đối tượng cụ thể.

- **Ý nghĩa:** Hãy nhìn vào nút "Play" trên điện thoại.
    
    - Nếu bạn đang mở Spotify, nút đó sẽ phát **nhạc**.
        
    - Nếu bạn đang mở YouTube, nút đó sẽ phát **video**.
        
    - Nếu bạn đang mở Netflix, nút đó sẽ phát **phim**.
        
- **Mục đích:** Giúp hệ thống linh hoạt. Bạn chỉ cần ra lệnh "Phát đi!", còn mỗi ứng dụng (lớp con) sẽ tự biết cách "phát" phù hợp với nó.