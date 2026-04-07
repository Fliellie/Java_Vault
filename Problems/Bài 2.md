### 📝 Tóm tắt đề bài "Tủ hồ sơ dùng chung" (Singleton)

Bạn cần tạo ra một Class tên là `AppConfig` sao cho:

1. **Quy tắc 1:** Không cho phép ai dùng lệnh `new AppConfig()` từ bên ngoài.
    
2. **Quy tắc 2:** Phải có một hàm `getInstance()` để lấy đối tượng. Hàm này chỉ tạo đối tượng khi thực sự có người gọi đến đầu tiên (Khởi tạo lười - Lazy Initialization).
    
3. **Quy tắc 3:** Dù 2 người (2 luồng) có cùng lúc xông vào đòi tạo, hệ thống vẫn tỉnh táo để không đẻ ra 2 cái tủ khác nhau.
    
4. **Kiểm tra:** Tạo 2 luồng chạy song song, cùng lấy `AppConfig` và in `hashCode()` ra. Nếu 2 mã số băm (hashCode) giống nhau $\rightarrow$ Bạn thắng!
    

---

### 🛠️ "Mớ đồ nghề" bạn cần dùng để giải bài này:

Để giải quyết bài này chuẩn chỉnh, bạn cần chuẩn bị sẵn trong đầu các kiến thức và từ khóa sau trong Java:

#### 1. Đồ nghề để tạo cấu trúc Singleton (Lazy)

- **`private static AppConfig instance;`** $\rightarrow$ Biến tĩnh dùng để lưu giữ đối tượng duy nhất.
    
- **`private AppConfig() { ... }`** $\rightarrow$ Đặt hàm khởi tạo (Constructor) là `private`. Đây là "tuyệt chiêu" để khóa chặt không cho class khác dùng lệnh `new`.
    
- **`public static AppConfig getInstance()`** $\rightarrow$ Hàm "cửa ngõ" để người ngoài thò tay vào lấy đối tượng.
    

#### 2. Đồ nghề để "Chống dẫm chân nhau" (An toàn đa luồng)

Nếu chỉ viết `if (instance == null) instance = new AppConfig();` thì khi 2 luồng cùng chạy vào một lúc, cả hai đều thấy `null` và thế là 2 đối tượng được tạo ra (Toang!). Để khắc phục, bạn cần dùng:

- Từ khóa **`synchronized`**: Giúp khóa hàm hoặc khóa một khối code lại, chỉ cho phép đúng 1 luồng được đi vào xử lý tại một thời điểm.
    
- Kỹ thuật đỉnh cao **`Double-Checked Locking`** kết hợp với từ khóa **`volatile`** cho biến `instance`: Đây là cách tối ưu nhất giúp app chạy vừa an toàn vừa mượt mà không bị chậm.
    

#### 3. Đồ nghề để tạo Đa luồng (Multi-threading) trong hàm `main`

Để mô phỏng 2 người cùng vào lấy cấu hình, bạn cần dùng:

- Biểu thức Lambda kết hợp với `Thread`:
    
    Java
    
    ```
    Runnable task = () -> {
        AppConfig config = AppConfig.getInstance();
        System.out.println("Mã hashCode: " + config.hashCode());
    };
    
    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);
    t1.start();
    t2.start();
    ```
    

---