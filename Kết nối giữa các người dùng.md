### Cách 1: Giải pháp "Mỳ ăn liền" dùng Ngrok (Đề xuất cho đồ án)

Đây là cách các nhóm sinh viên làm đồ án hay dùng nhất vì nó miễn phí và mất đúng 1 phút để cài.

- **Ngrok** là một phần mềm tạo ra một "đường hầm" xuyên thủng Tường lửa nhà bạn.
    
- Bạn chỉ cần chạy Server trên máy tính (cổng 9090). Sau đó bật Ngrok lên gõ: `ngrok tcp 9090`.
    
- Ngrok sẽ cấp cho bạn một cái link cực ngầu trên mạng (kiểu như: `tcp://0.tcp.ap.ngrok.io:12345`). Bạn chỉ cần copy cái link đó cho bạn bè nhập vào file Client. Bùm! Họ ở Mỹ cũng kết nối được vào máy bạn.
### 1. File Server: Trạm trung chuyển (`SimpleServer.java`)

Thằng này không cần giao diện. Nhiệm vụ của nó là: Ai kết nối tới thì ghi sổ lại. Ai gửi tin nhắn lên thì nó copy tin nhắn đó gửi cho tất cả những người còn lại trong sổ.

Java

```java
import java.io.*;
import java.net.*;
import java.util.*;

public class SimpleServer {
    // Cuốn sổ lưu lại tất cả các 'ống bơ' gửi tin nhắn về cho Client
    private static List<PrintWriter> danhSachClient = new ArrayList<>();

    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(9090); // Mở cửa số 9090
        System.out.println("== SERVER ĐANG CHẠY TẠI CỔNG 9090 ==");

        while (true) {
            Socket socket = server.accept(); // Đứng đợi, có người vào là tóm lại
            System.out.println("-> Có một người chơi vừa kết nối!");

            // Lấy cái ống để bơm dữ liệu về cho người chơi này, lưu vào sổ
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            danhSachClient.add(out);

            // Mở 1 luồng riêng để ngồi nghe ông khách này lảm nhảm
            new Thread(() -> {
                try {
                    BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                    String tinNhan;
                    // Hễ ông này gửi gì lên...
                    while ((tinNhan = in.readLine()) != null) {
                        System.out.println("Server nhận được: " + tinNhan);
                        // ...thì Server cầm loa hét lại cho TẤT CẢ mọi người cùng nghe
                        for (PrintWriter client : danhSachClient) {
                            client.println(tinNhan);
                        }
                    }
                } catch (Exception e) {}
            }).start();
        }
    }
}
```

---

### 2. File Client: Giao diện người chơi (`SimpleClient.java`)

Thằng này có một cái bảng JavaFX trống không. Nó mở 2 luồng: Một luồng đọc chữ bạn gõ từ Terminal để ném lên Server. Một luồng vểnh tai nghe Server, hễ Server gửi gì về thì in lên màn hình JavaFX.

Java

```java
import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.control.TextArea;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;
import java.io.*;
import java.net.Socket;
import java.util.Scanner;

public class SimpleClient extends Application {
    private TextArea manHinhHienThi;

    @Override
    public void start(Stage stage) {
        // 1. Dựng một cái UI trống không, chỉ có 1 cái bảng Text
        manHinhHienThi = new TextArea();
        manHinhHienThi.setEditable(false); // Không cho gõ vào bảng này
        
        stage.setScene(new Scene(new VBox(manHinhHienThi), 400, 300));
        stage.setTitle("App Chat Vui Vẻ");
        stage.show();

        // 2. Kết nối tới Server ở chế độ chạy ngầm để không làm đơ giao diện
        new Thread(this::ketNoiVoiServer).start();
    }

    private void ketNoiVoiServer() {
        try {
            // Gõ cửa nhà Server ở địa chỉ local (máy tính của bạn) và cổng 9090
            Socket socket = new Socket("localhost", 9090);
            PrintWriter guiLenServer = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader ngheTuServer = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            // LUỒNG A: Nhận chữ bạn gõ từ Terminal rồi ném cho Server
            new Thread(() -> {
                Scanner scanner = new Scanner(System.in);
                System.out.println("Nhập tin nhắn vào đây rồi ấn Enter nhé:");
                while (scanner.hasNextLine()) {
                    String chuoiVuaNhap = scanner.nextLine();
                    guiLenServer.println(chuoiVuaNhap); // Ném lên Server
                }
            }).start();

            // LUỒNG B: Nghe ngóng Server, có tin gì là cập nhật lên giao diện
            String tinNhanĐen;
            while ((tinNhanĐen = ngheTuServer.readLine()) != null) {
                String finalTinNhan = tinNhanĐen;
                // Nhớ quy tắc: Đụng vào UI là phải dùng Platform.runLater
                Platform.runLater(() -> manHinhHienThi.appendText(finalTinNhan + "\n"));
            }

        } catch (Exception e) {
            System.out.println("Không kết nối được Server!");
        }
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

### 🎮 Cách chạy để thấy "Phép màu"

1. **Bước 1:** Bật file `SimpleServer.java` lên chạy trước. Cứ để nó chạy ngầm đó.
    
2. **Bước 2:** Bật file `SimpleClient.java` lên. Bạn sẽ thấy 1 cái cửa sổ trắng bóc hiện ra. Đồng thời ở Terminal (console) hiện chữ _"Nhập tin nhắn vào đây..."_.
    
3. **Bước 3:** (Quan trọng để test online) - Bạn hãy bật **tiếp một cái `SimpleClient.java` nữa** (tức là chạy file đó lần 2). Lúc này bạn sẽ có 2 cửa sổ trắng.
    
4. **Trải nghiệm:** Bấm vào cái Terminal của Client 1, gõ "Hello anh em" rồi Enter. Nhìn lên 2 cái màn hình trắng xem chữ có nhảy ra đồng loạt không nhé!
