
![[Pasted image 20260410160344.png]]
![[Pasted image 20260410160351.png]]
![[Pasted image 20260410160357.png]]















### 1. Stage là gì?

Stage đại diện cho một **Window** (cửa sổ) trong hệ điều hành (Windows, macOS, hoặc Ubuntu của bạn).

- Nó chứa các thành phần trang trí cửa sổ tiêu chuẩn: thanh tiêu đề, nút đóng (X), nút thu nhỏ (_), và nút phóng to.
    
- Một ứng dụng JavaFX có thể có nhiều Stage (nhiều cửa sổ), nhưng luôn có một **Primary Stage** được hệ thống tự động tạo ra và truyền vào phương thức `start()`.
    

---

### 2. Các phương thức quan trọng của Stage

Dựa trên đoạn code bạn đã viết, đây là những gì Stage có thể làm:

|Phương thức|Tác dụng|
|---|---|
|`setTitle("String")`|Đặt văn bản hiển thị trên thanh tiêu đề của cửa sổ.|
|`setScene(Scene)`|Đặt nội dung (Cảnh) vào trong cửa sổ.|
|`show()`|Hiển thị cửa sổ lên màn hình (nếu không gọi hàm này, chương trình vẫn chạy ngầm nhưng bạn sẽ không thấy gì).|
|`setResizable(false)`|Ngăn người dùng dùng chuột kéo giãn kích thước cửa sổ.|
|`close()`|Đóng cửa sổ và kết thúc vòng đời của Stage đó.|
|`initStyle(StageStyle)`|Thay đổi kiểu dáng cửa sổ (ví dụ: làm cửa sổ trong suốt hoặc mất thanh tiêu đề).|

Export to Sheets

---

### 3. Vòng đời của Stage

1. **Khởi tạo:** Khi bạn gọi `launch(args)`, JavaFX tạo ra Primary Stage.
    
2. **Cấu hình:** Bạn thiết lập tiêu đề, kích thước và gắn `Scene` vào Stage trong hàm `start()`.
    
3. **Hiển thị:** Gọi `stage.show()` để người dùng tương tác.
    
4. **Đóng:** Khi người dùng nhấn nút X, Stage sẽ bị đóng và tài nguyên sẽ được giải phóng.
    

---

### 4. Ví dụ: Tạo thêm một cửa sổ mới (Popup)

Bạn không nhất thiết chỉ dùng một Stage. Bạn có thể tạo thêm cửa sổ thứ hai như thế này:

Java

```java
// Tạo một cửa sổ mới
Stage secondaryStage = new Stage();
secondaryStage.setTitle("Cửa sổ phụ");

// Tạo nội dung cho cửa sổ phụ
VBox box = new VBox(new Label("Đây là thông báo!"));
Scene secondScene = new Scene(box, 200, 100);

secondaryStage.setScene(secondScene);
secondaryStage.show(); // Hiện cửa sổ mới lên
```

### 5. Lưu ý cho người dùng Linux (Ubuntu)

Vì bạn đang sử dụng **Ubuntu 24.04** với giao diện Gnome:

- Stage trong JavaFX sẽ tuân theo **Window Manager** của hệ thống.
    
- Nếu bạn đã cài đặt các theme như **WhiteSur** (giống macOS), các nút đóng/mở trên Stage của JavaFX sẽ tự động thay đổi giao diện theo theme đó để đồng bộ với toàn bộ hệ điều hành.