![[Pasted image 20260401221711.png]]
Tạo giao diện như trên với những gì đã học :))
```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.shape.SVGPath;
import javafx.scene.text.Font;
import javafx.scene.text.FontWeight;
import javafx.stage.Stage;

public class LoginApp extends Application {

    @Override
    public void start(Stage primaryStage) {
        // 1. Tạo Layout chính (VBox sắp xếp các phần tử từ trên xuống dưới)
        VBox root = new VBox(15); // Khoảng cách giữa các phần tử là 15px
        root.setAlignment(Pos.CENTER); // Căn giữa tất cả các phần tử
        root.setPadding(new Insets(40, 50, 40, 50)); // Căn lề (Trên, Phải, Dưới, Trái)
        root.setStyle("-fx-background-color: white;"); // Nền trắng

        // Chiều rộng tiêu chuẩn cho các ô nhập liệu và nút bấm
        double fieldWidth = 300;

        // 2. Tạo Logo tia chớp màu cam (Sử dụng SVG Path để vẽ)
        SVGPath lightningLogo = new SVGPath();
        // Đường dẫn vẽ hình tia chớp đơn giản
        lightningLogo.setContent("M 15 0 L 0 18 L 12 18 L 8 32 L 26 12 L 14 12 Z");
        lightningLogo.setFill(Color.web("#ff8200")); // Tô màu cam giống ảnh
        
        // Bọc logo vào VBox để thêm khoảng trống phía dưới
        VBox logoBox = new VBox(lightningLogo);
        logoBox.setAlignment(Pos.CENTER);
        logoBox.setPadding(new Insets(0, 0, 10, 0));

        // 3. Tiêu đề "Welcome back"
        Label titleLabel = new Label("Welcome back");
        titleLabel.setFont(Font.font("Segoe UI", FontWeight.BOLD, 22));
        titleLabel.setTextFill(Color.web("#1a2b3c")); // Màu chữ xanh đen đậm

        // 4. Phụ đề "Sign in to continue to UltraAV"
        Label subtitleLabel = new Label("Sign in to continue to UltraAV");
        subtitleLabel.setFont(Font.font("Segoe UI", 14));
        subtitleLabel.setTextFill(Color.web("#667788")); // Màu xám nhạt
        
        VBox titleBox = new VBox(5, titleLabel, subtitleLabel);
        titleBox.setAlignment(Pos.CENTER);
        titleBox.setPadding(new Insets(0, 0, 15, 0));

        // 5. Ô nhập Email (Có viền màu cam đang được focus)
        TextField emailField = new TextField("support@ultraantivirus.com");
        emailField.setMaxWidth(fieldWidth);
        emailField.setPrefHeight(40);
        // Thiết lập CSS: Xóa viền mặc định, thêm viền cam giống ảnh
        emailField.setStyle(
                "-fx-background-color: white; " +
                "-fx-border-color: #ff8200; " +
                "-fx-border-width: 1.5px; " +
                "-fx-padding: 8px 12px;"
        );

        // 6. Ô nhập Mật khẩu (Có nền xám và icon con mắt)
        PasswordField passwordField = new PasswordField();
        passwordField.setPromptText("Enter password");
        passwordField.setStyle("-fx-background-color: transparent; -fx-padding: 0;"); // Trong suốt để ăn theo nền HBox
        HBox.setHgrow(passwordField, Priority.ALWAYS); // Chiếm phần không gian còn lại

        // Icon con mắt (dùng kí tự Unicode cho đơn giản)
        Label eyeIcon = new Label("👁"); 
        eyeIcon.setTextFill(Color.GRAY);
        eyeIcon.setStyle("-fx-font-size: 16px; -fx-cursor: hand;");

        // Bọc ô mật khẩu và icon con mắt vào một HBox nền xám
        HBox passwordBox = new HBox(10, passwordField, eyeIcon);
        passwordBox.setAlignment(Pos.CENTER_LEFT);
        passwordBox.setMaxWidth(fieldWidth);
        passwordBox.setPrefHeight(40);
        passwordBox.setStyle(
                "-fx-background-color: #f2f4f6; " + // Nền xám nhạt
                "-fx-padding: 8px 12px;"
        );

        // 7. Nút đăng nhập "Sign in"
        Button signInButton = new Button("Sign in");
        signInButton.setMaxWidth(fieldWidth);
        signInButton.setPrefHeight(40);
        // Style cho nút: nền cam nhạt, chữ màu cam đậm (giống trạng thái trong ảnh)
        signInButton.setStyle(
                "-fx-background-color: #fcd5b5; " + 
                "-fx-text-fill: #d28c55; " +
                "-fx-font-weight: bold; " +
                "-fx-cursor: hand;"
        );

        // 8. Liên kết "Forgot password?"
        Hyperlink forgotPasswordLink = new Hyperlink("Forgot password?");
        forgotPasswordLink.setStyle(
                "-fx-text-fill: #d35400; " + // Màu cam sẫm
                "-fx-underline: true; " + // Gạch chân
                "-fx-border-color: transparent;" // Bỏ viền gạch đứt mặc định khi click
        );

        // 9. Dòng chữ "Don't have an account? Sign up"
        Label noAccountLabel = new Label("Don't have an account? ");
        noAccountLabel.setTextFill(Color.web("#667788"));

        Hyperlink signUpLink = new Hyperlink("Sign up");
        signUpLink.setStyle(
                "-fx-text-fill: #d35400; " +
                "-fx-underline: true; " +
                "-fx-border-color: transparent; " +
                "-fx-padding: 0;"
        );

        // Đặt chữ và link vào HBox để nằm ngang hàng
        HBox signUpBox = new HBox(noAccountLabel, signUpLink);
        signUpBox.setAlignment(Pos.CENTER);
        signUpBox.setPadding(new Insets(30, 0, 0, 0)); // Tạo khoảng cách xa hơn phía trên

        // 10. Gắn tất cả các phần tử vào Layout chính (root)
        root.getChildren().addAll(
                logoBox,
                titleBox,
                emailField,
                passwordBox,
                signInButton,
                forgotPasswordLink,
                signUpBox
        );

        // 11. Khởi tạo Scene và hiển thị Stage (Cửa sổ ứng dụng)
        Scene scene = new Scene(root, 450, 550); // Chiều rộng 450, Chiều cao 550
        primaryStage.setTitle("UltraAV Login");
        primaryStage.setScene(scene);
        primaryStage.setResizable(false); // Khóa kích thước cửa sổ
        primaryStage.show();
        
        // Ép focus ra khỏi textfield ban đầu để thấy rõ placeholder
        root.requestFocus(); 
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```



# Bản tách code
```css
/* style.css */

/* Nền chính của ứng dụng */
.root-pane {
    -fx-background-color: white;
}

/* Ô nhập email đang được focus (viền cam) */
.email-field {
    -fx-background-color: white;
    -fx-border-color: #ff8200;
    -fx-border-width: 1.5px;
    -fx-padding: 8px 12px;
}

/* Hộp chứa mật khẩu (nền xám) */
.password-box {
    -fx-background-color: #f2f4f6;
    -fx-padding: 8px 12px;
}

/* Ô nhập mật khẩu (trong suốt để ăn theo nền của password-box) */
.password-field {
    -fx-background-color: transparent;
    -fx-padding: 0;
}

/* Icon con mắt */
.eye-icon {
    -fx-font-size: 16px;
    -fx-cursor: hand;
}

/* Nút Sign in */
.sign-in-button {
    -fx-background-color: #fcd5b5;
    -fx-text-fill: #d28c55;
    -fx-font-weight: bold;
    -fx-cursor: hand;
}

/* Liên kết (Forgot password, Sign up) */
.custom-link {
    -fx-text-fill: #d35400;
    -fx-underline: true;
    -fx-border-color: transparent;
}

/* Xóa padding mặc định cho link Sign up để nó sát vào chữ bên cạnh */
.sign-up-link {
    -fx-padding: 0;
}
```



```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.shape.SVGPath;
import javafx.scene.text.Font;
import javafx.scene.text.FontWeight;
import javafx.stage.Stage;

public class LoginApp extends Application {

    @Override
    public void start(Stage primaryStage) {
        // 1. Tạo Layout chính
        VBox root = new VBox(15);
        root.setAlignment(Pos.CENTER);
        root.setPadding(new Insets(40, 50, 40, 50));
        root.getStyleClass().add("root-pane"); // Thêm class CSS

        double fieldWidth = 300;

        // 2. Tạo Logo tia chớp
        SVGPath lightningLogo = new SVGPath();
        lightningLogo.setContent("M 15 0 L 0 18 L 12 18 L 8 32 L 26 12 L 14 12 Z");
        lightningLogo.setFill(Color.web("#ff8200")); 
        
        VBox logoBox = new VBox(lightningLogo);
        logoBox.setAlignment(Pos.CENTER);
        logoBox.setPadding(new Insets(0, 0, 10, 0));

        // 3. Tiêu đề
        Label titleLabel = new Label("Welcome back");
        titleLabel.setFont(Font.font("Segoe UI", FontWeight.BOLD, 22));
        titleLabel.setTextFill(Color.web("#1a2b3c"));

        // 4. Phụ đề
        Label subtitleLabel = new Label("Sign in to continue to UltraAV");
        subtitleLabel.setFont(Font.font("Segoe UI", 14));
        subtitleLabel.setTextFill(Color.web("#667788"));
        
        VBox titleBox = new VBox(5, titleLabel, subtitleLabel);
        titleBox.setAlignment(Pos.CENTER);
        titleBox.setPadding(new Insets(0, 0, 15, 0));

        // 5. Ô nhập Email
        TextField emailField = new TextField("support@ultraantivirus.com");
        emailField.setMaxWidth(fieldWidth);
        emailField.setPrefHeight(40);
        emailField.getStyleClass().add("email-field"); // Thêm class CSS

        // 6. Ô nhập Mật khẩu & Icon
        PasswordField passwordField = new PasswordField();
        passwordField.setPromptText("Enter password");
        passwordField.getStyleClass().add("password-field"); // Thêm class CSS
        HBox.setHgrow(passwordField, Priority.ALWAYS);

        Label eyeIcon = new Label("👁"); 
        eyeIcon.setTextFill(Color.GRAY);
        eyeIcon.getStyleClass().add("eye-icon"); // Thêm class CSS

        HBox passwordBox = new HBox(10, passwordField, eyeIcon);
        passwordBox.setAlignment(Pos.CENTER_LEFT);
        passwordBox.setMaxWidth(fieldWidth);
        passwordBox.setPrefHeight(40);
        passwordBox.getStyleClass().add("password-box"); // Thêm class CSS

        // 7. Nút đăng nhập
        Button signInButton = new Button("Sign in");
        signInButton.setMaxWidth(fieldWidth);
        signInButton.setPrefHeight(40);
        signInButton.getStyleClass().add("sign-in-button"); // Thêm class CSS

        // 8. Liên kết Forgot password
        Hyperlink forgotPasswordLink = new Hyperlink("Forgot password?");
        forgotPasswordLink.getStyleClass().add("custom-link"); // Thêm class CSS chung cho link

        // 9. Dòng chữ Sign up
        Label noAccountLabel = new Label("Don't have an account? ");
        noAccountLabel.setTextFill(Color.web("#667788"));

        Hyperlink signUpLink = new Hyperlink("Sign up");
        signUpLink.getStyleClass().addAll("custom-link", "sign-up-link"); // Thêm 2 class CSS

        HBox signUpBox = new HBox(noAccountLabel, signUpLink);
        signUpBox.setAlignment(Pos.CENTER);
        signUpBox.setPadding(new Insets(30, 0, 0, 0));

        // 10. Gắn tất cả vào root
        root.getChildren().addAll(
                logoBox, titleBox, emailField, passwordBox, 
                signInButton, forgotPasswordLink, signUpBox
        );

        // 11. Khởi tạo Scene
        Scene scene = new Scene(root, 450, 550);
        
        // --- QUAN TRỌNG: Nạp file CSS vào Scene ---
        // Đảm bảo file style.css nằm cùng vị trí thư mục với file LoginApp.class sau khi build
        try {
            scene.getStylesheets().add(getClass().getResource("style.css").toExternalForm());
        } catch (NullPointerException e) {
            System.out.println("Không tìm thấy file style.css. Vui lòng kiểm tra lại đường dẫn!");
        }

        primaryStage.setTitle("UltraAV Login");
        primaryStage.setScene(scene);
        primaryStage.setResizable(false);
        primaryStage.show();
        
        root.requestFocus(); 
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```