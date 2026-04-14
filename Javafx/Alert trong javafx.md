Trong JavaFX, để tạo các hộp thoại thông báo (popup), bạn sử dụng lớp `Alert` thuộc package `javafx.scene.control`.

Dưới đây là cú pháp và các loại thông báo phổ biến nhất, được trình bày trực tiếp vào code để bạn dễ áp dụng:

### 1. 4 Loại Alert (AlertType) cơ bản

Khi khởi tạo một `Alert`, bạn phải truyền vào loại của nó để JavaFX tự động thiết lập icon và các nút bấm mặc định (như OK, Cancel):

- `Alert.AlertType.INFORMATION`: Thông báo thông tin.
    
- `Alert.AlertType.WARNING`: Cảnh báo.
    
- `Alert.AlertType.ERROR`: Thông báo lỗi.
    
- `Alert.AlertType.CONFIRMATION`: Hộp thoại xác nhận (Có nút OK và Cancel).
    

---

### 2. Code mẫu tạo thông báo đơn giản (Information / Warning / Error)

Mẫu code dưới đây áp dụng chung cho các loại thông báo chỉ cần người dùng đọc và bấm "OK" để đóng.

Java

```
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;

// 1. Khởi tạo đối tượng Alert với loại thông báo
Alert alert = new Alert(AlertType.INFORMATION);

// 2. Thiết lập nội dung
alert.setTitle("Thông báo hệ thống"); // Tiêu đề cửa sổ
alert.setHeaderText("Lưu dữ liệu thành công!"); // Dòng chữ in đậm ở trên cùng
alert.setContentText("Hồ sơ của bạn đã được cập nhật vào cơ sở dữ liệu."); // Nội dung chi tiết

// Mẹo: Nếu không muốn hiện phần Header, hãy set nó bằng null
// alert.setHeaderText(null); 

// 3. Hiển thị hộp thoại
alert.showAndWait(); 
```

---

### 3. Code mẫu tạo hộp thoại Xác nhận (Confirmation)

Đối với loại `CONFIRMATION`, bạn cần lấy được kết quả xem người dùng đã bấm nút `OK` hay `Cancel` để xử lý logic tiếp theo. Bạn sẽ dùng thêm đối tượng `Optional<ButtonType>`.

Java

```
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;
import javafx.scene.control.ButtonType;
import java.util.Optional;

Alert alert = new Alert(AlertType.CONFIRMATION);
alert.setTitle("Xác nhận xóa");
alert.setHeaderText("Bạn có chắc chắn muốn xóa mục này?");
alert.setContentText("Hành động này không thể hoàn tác.");

// Hiển thị và chờ người dùng chọn
Optional<ButtonType> result = alert.showAndWait();

// Kiểm tra kết quả
if (result.isPresent() && result.get() == ButtonType.OK) {
    // Thực thi code xóa dữ liệu ở đây
    System.out.println("Đã xóa dữ liệu!");
} else {
    // Hủy bỏ lệnh, không làm gì cả
    System.out.println("Đã hủy thao tác.");
}
```

### 4. Lưu ý quan trọng

- **`showAndWait()` vs `show()`**: Luôn ưu tiên dùng `showAndWait()`. Nó sẽ chặn (block) luồng ứng dụng lại, bắt người dùng phải tương tác với hộp thoại xong thì đoạn code bên dưới mới được chạy tiếp. Nếu dùng `show()`, hộp thoại sẽ hiện lên nhưng code bên dưới vẫn chạy tuồn tuột bỏ qua nó.

### 1. AlertType.WARNING (Cảnh báo)

Loại này dùng khi người dùng thực hiện một thao tác có rủi ro, hoặc hệ thống phát hiện vấn đề nhưng chưa nghiêm trọng đến mức gây lỗi dừng phần mềm (Ví dụ: Định thoát mà chưa lưu file, đặt mật khẩu quá yếu, dung lượng ổ đĩa sắp hết...).

Java

```
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;

Alert alert = new Alert(AlertType.WARNING);
alert.setTitle("Cảnh báo bảo mật");
alert.setHeaderText(null); // Đặt là null để ẩn dòng tiêu đề in đậm cho gọn
alert.setContentText("Mật khẩu của bạn quá ngắn. Nên dùng ít nhất 8 ký tự để đảm bảo an toàn.");
alert.showAndWait();
```

_(Giao diện sẽ hiển thị biểu tượng hình tam giác vàng có dấu chấm than)._

### 2. AlertType.ERROR (Báo lỗi)

Dùng khi có một sự cố thực sự xảy ra, ngăn cản thao tác tiếp theo của người dùng (Ví dụ: Đăng nhập sai tài khoản, mất kết nối cơ sở dữ liệu, file không tồn tại).

Java

```
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;

Alert alert = new Alert(AlertType.ERROR);
alert.setTitle("Lỗi thao tác");
alert.setHeaderText("Không thể lưu tài liệu!");
alert.setContentText("Ổ đĩa của bạn đã đầy hoặc phần mềm không có quyền ghi vào thư mục này.");
alert.showAndWait();
```

_(Giao diện sẽ hiển thị biểu tượng hình tròn màu đỏ có dấu X)._

---

### 💡 Bật mí thêm: 2 Loại Hộp thoại (Dialog) cực kỳ hữu ích khác

Trong thực tế làm JavaFX, ngoài việc "thông báo" cho người dùng biết (bằng `Alert`), bạn sẽ rất hay gặp trường hợp cần người dùng **nhập thêm dữ liệu nhanh** qua hộp thoại. Lúc này, bạn không dùng `Alert` nữa mà dùng 2 lớp họ hàng của nó:

**1. TextInputDialog (Hộp thoại yêu cầu nhập chữ)** Dùng khi bạn muốn người dùng gõ vào một đoạn text (như hỏi tên, số điện thoại, lý do hủy đơn...).

Java

```
import javafx.scene.control.TextInputDialog;
import java.util.Optional;

TextInputDialog dialog = new TextInputDialog("admin"); // Chữ "admin" sẽ được điền sẵn
dialog.setTitle("Nhập thông tin");
dialog.setHeaderText("Vui lòng nhập tên đăng nhập mới:");

// Hàm này trả về Optional<String> thay vì ButtonType
Optional<String> result = dialog.showAndWait();

// Nếu người dùng bấm OK và có nhập chữ
if (result.isPresent()){
    System.out.println("Tên mới là: " + result.get());
}
```

**2. ChoiceDialog (Hộp thoại chọn từ danh sách)** Hiển thị một menu dạng thả xuống (Dropdown) bắt người dùng chọn 1 trong các phương án có sẵn.

Java

```
import javafx.scene.control.ChoiceDialog;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

List<String> choices = Arrays.asList("Giao hàng tiêu chuẩn", "Giao hàng hỏa tốc", "Nhận tại cửa hàng");

// Truyền vào (Giá trị mặc định, Danh sách các giá trị)
ChoiceDialog<String> dialog = new ChoiceDialog<>("Giao hàng tiêu chuẩn", choices);
dialog.setTitle("Phương thức vận chuyển");
dialog.setHeaderText("Hãy chọn một phương thức bên dưới:");

Optional<String> result = dialog.showAndWait();
if (result.isPresent()){
    System.out.println("Bạn đã chọn: " + result.get());
}
```
