Trong JavaFX, một cửa sổ được cấu tạo bởi 3 lớp chính lồng vào nhau: **Stage** (Khung cửa sổ) > **Scene** (Khung cảnh/Màn hình) > **Root Node** (Bố cục gốc).

Dưới đây là đoạn code tối thiểu và chuẩn nhất để khởi chạy một cửa sổ như vậy:


```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class Main extends Application {

    // Phương thức start() là điểm bắt đầu của mọi ứng dụng JavaFX
    @Override
    public void start(Stage primaryStage) {
        
        // 1. Tạo một Root Node (Vùng chứa gốc). Ở đây dùng StackPane trống.
        StackPane root = new StackPane();

        // 2. Tạo một Scene (Khung cảnh) chứa vùng gốc đó. 
        // Đặt kích thước cửa sổ là Rộng 400px, Cao 300px.
        Scene scene = new Scene(root, 400, 300);

        // 3. Thiết lập cho Stage (Cửa sổ chính)
        primaryStage.setTitle("Cửa sổ JavaFX Cơ Bản"); // Đặt tiêu đề
        primaryStage.setScene(scene);                  // Đưa Scene vào Stage
        primaryStage.show();                           // Hiển thị cửa sổ lên màn hình
    }

    // Phương thức main() để chạy chương trình
    public static void main(String[] args) {
        launch(args); // Gọi hàm launch() để khởi động luồng JavaFX
    }
}
```

### Giải thích nhanh 4 thành phần cốt lõi:

1. **`extends Application`**: Lớp của bạn bắt buộc phải kế thừa lớp `Application` của JavaFX. Nó cho phép chương trình quản lý vòng đời của giao diện (khởi tạo, chạy, đóng).
    
2. **`Stage`**: Chính là cái vỏ cửa sổ của hệ điều hành (chứa nút X tắt, thu nhỏ, phóng to và thanh tiêu đề). `primaryStage` được hệ thống tự động tạo và truyền vào hàm `start()`.
    
3. **`Scene`**: Nội dung bên trong cửa sổ. Một `Stage` có thể đổi nhiều `Scene` khác nhau (giống như chuyển cảnh trong game hoặc chuyển trang web).
    
4. **`StackPane (Root)`**: Một `Scene` bắt buộc phải có một "nền móng" để chứa các thành phần khác. Ở đây ta dùng `StackPane` trống. Về sau, nếu bạn muốn thêm nút bấm, chữ viết, bạn sẽ "nhét" chúng vào cái `root` này.