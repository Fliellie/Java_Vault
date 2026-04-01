![[Pasted image 20260331203858.png|992]]

![[Pasted image 20260331203924.png|989]]




Nếu ví **Stage** là cái khung cửa sổ, thì **Scene** chính là tấm kính và những thứ được vẽ lên tấm kính đó.

---

### 1. Vị trí của Scene trong Hierarchy

JavaFX sử dụng cấu trúc **Scene Graph** (Cây phân cấp). Bạn có thể hình dung như sau:

1. **Stage (Sân khấu):** Cửa sổ ứng dụng (ngoài cùng).
    
2. **Scene (Cảnh):** Chứa một "Root Node" duy nhất.
    
3. **Nodes (Nút):** Các thành phần giao diện như Button, Label, Layout (VBox, HBox).
    

---

### 2. Các đặc điểm quan trọng của Scene

- **Một Stage - Một Scene tại một thời điểm:** Một cửa sổ (Stage) chỉ có thể hiển thị một Scene duy nhất tại một thời điểm. Tuy nhiên, bạn có thể dễ dàng "đổi cảnh" (ví dụ: bấm nút "Login" để chuyển từ Scene Đăng nhập sang Scene Trang chủ).
    
- **Root Node:** Khi tạo một Scene, bạn bắt buộc phải truyền vào một nút gốc (thường là một Layout như `StackPane`, `GridPane` hoặc `VBox`).
    
- **Kích thước:** Bạn có thể xác định chiều rộng và chiều cao của Scene ngay khi khởi tạo (ví dụ: `300x250` pixels).
    

---

### 3. Cách hoạt động trong code

Hãy nhìn lại đoạn code bạn đã chia sẻ, đây là cách `Scene` được thiết lập:

Java

```
// 1. Tạo Layout (Root Node)
StackPane layout = new StackPane(); 
layout.getChildren().add(button);

// 2. Tạo Scene và gắn Layout vào, đặt kích thước 300x250
Scene scene = new Scene(layout, 300, 250);

// 3. Đưa Scene lên Stage để hiển thị
primaryStage.setScene(scene);
primaryStage.show();
```

---

### 4. Quản lý sự kiện (Event Handling)

Scene không chỉ để hiển thị, nó còn là nơi quản lý các sự kiện từ chuột và bàn phím.

- Nếu bạn muốn bắt sự kiện khi người dùng nhấn một phím bất kỳ trên toàn bộ cửa sổ (không chỉ riêng một nút bấm), bạn sẽ gắn bộ lắng nghe vào `Scene`: `scene.setOnKeyPressed(event -> System.out.println("Bạn vừa nhấn phím: " + event.getCode()));`
    

### 5. Styling với CSS

Bạn có thể dùng CSS để làm đẹp cho toàn bộ ứng dụng thông qua Scene: `scene.getStylesheets().add("style.css");`

**Tóm lại:** Nếu bạn muốn thay đổi toàn bộ nội dung cửa sổ (ví dụ: chuyển từ màn hình Game sang màn hình Menu), cách tốt nhất là tạo một **Scene mới** và set lại cho `primaryStage`.

### 2. Cách áp dụng CSS vào ứng dụng JavaFX

Để JavaFX nhận diện và áp dụng file CSS này, bạn chỉ cần liên kết nó vào đối tượng `Scene` (khung cảnh) hoặc trực tiếp vào một `Parent/Node` cụ thể trong mã Java của bạn:
![[Pasted image 20260331084608.png]]
Tuy nhiên, CSS trong JavaFX có một điểm đặc trưng là các thuộc tính (properties) thường bắt đầu bằng tiền tố **`-fx-`** để phân biệt với CSS chuẩn của web.
![[Pasted image 20260331084645.png]]
