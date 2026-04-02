Nếu `VBox` là chuyên gia xếp gạch chồng lên nhau, thì **`HBox`** (viết tắt của Horizontal Box) lại là chuyên gia xếp hàng ngang.

`HBox` là một bộ chứa (container) sẽ tự động sắp xếp tất cả các phần tử con bên trong nó theo **chiều ngang**, từ trái sang phải trên một hàng duy nhất.

Bạn có thể thấy `HBox` xuất hiện rất nhiều trong các giao diện thực tế, ví dụ điển hình nhất là thanh công cụ (Toolbar), menu điều hướng, hoặc một thanh tìm kiếm gồm một ô nhập chữ và một nút bấm nằm cạnh nhau. Trong ví dụ làm màn hình đăng nhập lúc trước, mình cũng đã dùng `HBox` để xếp dòng chữ "Don't have an account?" và nút "Sign up" nằm ngang hàng với nhau đấy!

---

### 1. Các thuộc tính quan trọng của HBox

Các thuộc tính của HBox hoàn toàn tương tự như VBox, chỉ khác là nó tác động theo chiều ngang:

- **`spacing` (Khoảng cách):** Khoảng trống (pixel) giữa các phần tử theo chiều ngang.
    
- **`alignment` (Căn lề):** Vị trí của khối các phần tử (ví dụ: `Pos.CENTER_LEFT` - căn giữa theo chiều dọc nhưng ép sát lề trái, `Pos.BOTTOM_RIGHT` - góc dưới cùng bên phải...).
    
- **`padding` (Lề trong):** Khoảng cách từ đường viền ngoài của HBox đẩy các phần tử bên trong thụt vào.
    
- **`HGrow` (Độ co giãn ngang):** Đây là thuộc tính cực kỳ hữu ích của HBox. Nó cho phép bạn chỉ định một phần tử (như Textfield) tự động **giãn ra theo chiều ngang** để lấp đầy toàn bộ khoảng trống còn thừa của HBox.
    

---

### 2. Ví dụ cách sử dụng HBox làm "Thanh tìm kiếm"

Dưới đây là đoạn code tạo ra một thanh tìm kiếm cơ bản. Thanh này gồm một dòng chữ, một ô nhập liệu (tự giãn) và một nút bấm, tất cả nằm gọn trên một hàng ngang.

Java

```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.HBox;
import javafx.scene.layout.Priority;
import javafx.stage.Stage;

public class HBoxExample extends Application {

    @Override
    public void start(Stage primaryStage) {
        // 1. Khởi tạo HBox với khoảng cách giữa các phần tử là 10px
        HBox searchBar = new HBox(10); 
        
        // 2. Căn lề: Căn giữa theo chiều dọc, sát lề trái
        searchBar.setAlignment(Pos.CENTER_LEFT); 
        
        // 3. Padding: Tạo khoảng trống lề xung quanh 15px
        searchBar.setPadding(new Insets(15));
        searchBar.setStyle("-fx-background-color: #f0f0f0;"); // Nền xám nhạt

        // 4. Tạo các phần tử con
        Label lblSearch = new Label("Tìm kiếm:");
        TextField txtSearch = new TextField();
        txtSearch.setPromptText("Nhập từ khóa...");
        Button btnSearch = new Button("Tìm");

        // 5. Sử dụng HGrow: Ép ô nhập liệu (TextField) tự động giãn ngang 
        // để chiếm hết phần không gian trống còn lại
        HBox.setHgrow(txtSearch, Priority.ALWAYS);

        // 6. Thêm tất cả vào HBox (Thứ tự thêm vào sẽ là thứ tự từ trái qua phải)
        searchBar.getChildren().addAll(lblSearch, txtSearch, btnSearch);

        // Hiển thị lên màn hình
        Scene scene = new Scene(searchBar, 400, 100);
        primaryStage.setTitle("Ví dụ về HBox");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

Bạn đã nắm được cách hoạt động độc lập của VBox và HBox rồi, bạn có muốn mình hướng dẫn cách lồng ghép cả `VBox` và `HBox` lại với nhau để tạo ra những cấu trúc giao diện phức tạp và thực tế hơn không?