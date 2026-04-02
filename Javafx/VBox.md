### 1. VBox là gì?

**VBox** (viết tắt của Vertical Box) là một bộ chứa (container) sẽ tự động sắp xếp tất cả các phần tử con bên trong nó theo **chiều dọc**, từ trên xuống dưới thành một cột duy nhất.

Bạn có thể tưởng tượng nó giống như việc bạn xếp các khối lego chồng lên nhau vậy. Trong ví dụ về màn hình đăng nhập ở trên, toàn bộ form từ Logo -> Welcome -> Email -> Password -> Sign in đều được xếp dọc, đó chính là công việc của `VBox`.

---

### 2. Các thuộc tính quan trọng nhất của VBox

Để làm chủ VBox, bạn chỉ cần nắm vững 4 khái niệm cơ bản sau:

- **`spacing` (Khoảng cách):** Khoảng trống (tính bằng pixel) giữa các phần tử với nhau.
    
- **`alignment` (Căn lề):** Vị trí của toàn bộ khối các phần tử so với không gian của VBox (ví dụ: căn giữa `Pos.CENTER`, căn trên cùng bên trái `Pos.TOP_LEFT`, căn dưới cùng `Pos.BOTTOM_CENTER`...).
    
- **`padding` (Lề trong):** Khoảng cách từ đường viền của VBox đẩy các phần tử bên trong thụt vào (tương tự padding trong CSS).
    
- **`VGrow` (Độ co giãn):** Thuộc tính nâng cao giúp bạn quyết định xem một phần tử con có được phép kéo giãn ra để lấp đầy không gian thừa theo chiều dọc khi người dùng phóng to cửa sổ hay không (`VBox.setVgrow(node, Priority.ALWAYS)`).
    

---

### 3. Ví dụ cách sử dụng (Kèm Code)

Dưới đây là một đoạn code mẫu cực kỳ ngắn gọn, giải thích từng bước cách khởi tạo và tinh chỉnh `VBox`:

Java

```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class VBoxExample extends Application {

    @Override
    public void start(Stage primaryStage) {
        // 1. Khởi tạo VBox với 'spacing' (khoảng cách giữa các nút) là 15px
        VBox myVBox = new VBox(15); 

        // 2. Căn giữa tất cả các phần tử vào giữa màn hình
        myVBox.setAlignment(Pos.CENTER); 

        // 3. Thiết lập Padding (Lề Trên, Phải, Dưới, Trái) là 20px
        myVBox.setPadding(new Insets(20, 20, 20, 20)); 
        
        // Thêm màu nền nhạt để bạn dễ hình dung ranh giới của VBox
        myVBox.setStyle("-fx-background-color: #e8f4f8;"); 

        // 4. Tạo các phần tử con
        Button btn1 = new Button("Nút số 1");
        Button btn2 = new Button("Nút số 2 (Dài hơn một chút)");
        Button btn3 = new Button("Nút số 3");

        // 5. Thêm các phần tử con vào VBox (chúng sẽ tự động xếp dọc)
        myVBox.getChildren().addAll(btn1, btn2, btn3);

        // MẸO: Nếu bạn muốn tất cả các nút giãn chiều rộng bằng nhau để nhìn đẹp hơn
        // btn1.setMaxWidth(Double.MAX_VALUE);
        // btn2.setMaxWidth(Double.MAX_VALUE);
        // btn3.setMaxWidth(Double.MAX_VALUE);

        // Hiển thị lên màn hình
        Scene scene = new Scene(myVBox, 300, 250);
        primaryStage.setTitle("Ví dụ về VBox");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

> **Ghi nhớ nhanh:** Nếu bạn muốn xếp mọi thứ chồng lên nhau theo **chiều dọc**, hãy dùng **`VBox`**! Ngược lại, nếu muốn các phần tử nối đuôi nhau theo **chiều ngang** (từ trái sang phải), chúng ta sẽ dùng người anh em của nó là **`HBox`**.