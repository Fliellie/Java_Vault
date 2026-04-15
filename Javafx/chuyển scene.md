## 1. Phương pháp cơ bản: Thay đổi Root của Stage

```java

import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.scene.Node;
import javafx.stage.Stage;
import javafx.event.ActionEvent;
import java.io.IOException;

public class View1Controller {

    public void switchToView2(ActionEvent event) throws IOException {
        // 1. Tải file FXML mới
        Parent loader = FXMLLoader.load(getClass().getResource("View2.fxml"));
        
        // 2. Lấy Stage hiện tại từ sự kiện (event)
        Stage stage = (Stage)((Node)event.getSource()).getScene().getWindow();
        
        // 3. Tạo Scene mới với root vừa tải
        Scene scene = new Scene(root);
        
        // 4. Đặt Scene vào Stage và hiển thị
        stage.setScene(scene);
        stage.show();
    }
}
```

---

## 2. Cách truyền dữ liệu giữa các Controller

```java

public void handleLogin(ActionEvent event) throws IOException {
    FXMLLoader loader = new FXMLLoader(getClass().getResource("Profile.fxml"));
    Parent root = loader.load();

    // Lấy instance của Controller đích
    ProfileController profileController = loader.getController();
    
    // Gọi phương thức để truyền dữ liệu
    profileController.displayName("Chào mừng, Admin!");

    Stage stage = (Stage)((Node)event.getSource()).getScene().getWindow();
    stage.setScene(new Scene(root));
    stage.show();
}
```

---

## 3. Một số lưu ý quan trọng

- **Đường dẫn File:** Hãy đảm bảo đường dẫn `.getResource("filename.fxml")` là chính xác. Nếu file nằm trong package khác, bạn cần dùng đường dẫn tuyệt đối tính từ thư mục `resources` (ví dụ: `"/com/myapp/views/View2.fxml"`).
    
- **Quản lý Stage:** Nếu ứng dụng của bạn lớn, hãy cân nhắc tạo một lớp **NavigationUtils** hoặc **SceneManager** để quản lý việc chuyển cảnh tập trung, tránh lặp lại code.
    
- **Hiệu suất:** Việc load FXML tốn tài nguyên. Với các Scene chuyển đổi qua lại liên tục, bạn có thể load sẵn chúng vào bộ nhớ (cache) thay vì load lại từ ổ đĩa mỗi lần chuyển.
    

> **Mẹo nhỏ:** Để giao diện không bị giật khi chuyển cảnh, bạn có thể thêm các hiệu ứng Fade In/Fade Out bằng thư mục `javafx.animation`.