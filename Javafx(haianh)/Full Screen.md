### 1. Full Screen ngay khi khởi động

Bạn sẽ viết code này trong file `Main.java`, ngay sau khi bạn đã có đối tượng `stage`.

Java

```java
@Override
public void start(Stage stage) throws IOException {
    FXMLLoader fxmlLoader = new FXMLLoader(Main.class.getResource("login.fxml"));
    Scene scene = new Scene(fxmlLoader.load());
    
    stage.setTitle("Ứng dụng của tôi");
    stage.setScene(scene);

    // Dòng quan trọng đây:
    stage.setFullScreen(true); 
    
    stage.show();
}
```

### 2. Cho phép nhấn nút để Full Screen

Nếu bạn muốn khi người dùng nhấn cái nút "HI" thì cửa sổ mới phóng to ra, bạn viết trong `Controller.java`:

Java

```java
@FXML
public void Button_HI(ActionEvent event) {
    // Lấy stage hiện tại
    Stage stage = (Stage) ((Node) event.getSource()).getScene().getWindow();
    
    // Đảo ngược trạng thái Full Screen (đang nhỏ thì to, đang to thì nhỏ)
    stage.setFullScreen(!stage.isFullScreen());
}
```

---

### 3. Các tùy chỉnh nâng cao (Rất hữu ích)

Khi bạn để chế độ Full Screen, JavaFX sẽ có một vài hành vi mặc định mà có thể bạn muốn thay đổi:

- **Tắt dòng chữ "Press ESC to exit full screen":** Nếu bạn không muốn dòng chữ gợi ý này hiện lên mỗi khi vào full screen:
    
    Java
    
    ```java
    stage.setFullScreenExitHint(""); 
    ```
    
- **Thay đổi phím thoát:** Mặc định là phím **ESC**. Bạn có thể đổi sang phím khác (ví dụ phím `X`):
    
    Java
    
    ```java
    stage.setFullScreenExitKeyCombination(KeyCombination.valueOf("X"));
    ```
    
- **Chế độ Maximize (Phóng to nhưng vẫn hiện thanh Taskbar):** Nếu bạn không muốn "nuốt" luôn cả thanh công cụ của Windows/Linux mà chỉ muốn nó to hết cỡ:
    
    Java
    
    ```java
    stage.setMaximized(true);
    ```
    

---

### 4. Lưu ý về Layout (Giao diện)

Khi bạn cho Full Screen, giao diện của bạn sẽ bị "lọt thỏm" ở giữa hoặc bị lệch sang trái nếu bạn dùng `AnchorPane` với tọa độ cố định.

> **Mẹo:** Để giao diện tự co giãn đẹp mắt khi Full Screen, bạn nên sử dụng các **Layout Panes** thông minh thay vì tọa độ tuyệt đối:
> 
> - **`VBox` / `HBox`**: Để các thành phần tự căn giữa.
>     
> - **`StackPane`**: Để mọi thứ luôn nằm chính giữa màn hình.
>     
> - **Anchor Constraints**: Trong Scene Builder, hãy "neo" (anchor) các nút bấm vào 4 cạnh của màn hình.
>