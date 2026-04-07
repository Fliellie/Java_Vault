Bài số 5 này chính là bài "Chốt hạ" để kiểm tra xem bạn đã thực sự làm chủ được các Design Patterns vừa học hay chưa. Thay vì dọn sẵn món như các bài trước, đề bài lần này đưa ra các **tình huống thực tế** và bắt bạn phải tự "bốc thuốc" xem tình huống nào thì dùng Pattern nào.

Mình sẽ giúp bạn phân tích đề bài, gọi tên chính xác từng Pattern cần dùng và bày sẵn "đồ nghề" để bạn tự tin gõ code trên VS Code nhé!

---

### 📝 Phân tích và "Bắt bệnh" cho 4 yêu cầu

#### 🩺 Tình huống 1:

> _"Hệ thống cần một lớp Logger chỉ có một đối tượng duy nhất trong chương trình."_

- **Pattern phù hợp:** **`Singleton`**
    
- **Tại sao:** Từ khóa _"chỉ có một đối tượng duy nhất"_ chính là định nghĩa nguyên bản của Singleton.
    

#### 🩺 Tình huống 2:

> _"Hệ thống cần tạo các đối tượng Export: PdfExport, ExcelExport. Việc tạo đối tượng không được viết trực tiếp bằng new trong main."_

- **Pattern phù hợp:** **`Factory Method`**
    
- **Tại sao:** Đề bài yêu cầu tạo ra các sản phẩm cùng loại (Export) nhưng cấm dùng lệnh `new` trực tiếp trong `main` $\rightarrow$ Phải đẩy việc `new` này cho một "nhà máy" (Factory) lo.
    

#### 🩺 Tình huống 3:

> _"Hệ thống có lớp cũ: `OldPlayer` (có hàm `playFile()`). Hệ thống mới yêu cầu: `interface Player` (có hàm `play()`). Không được sửa lớp cũ."_

- **Pattern phù hợp:** **`Adapter`**
    
- **Tại sao:** Đây là bài toán "lệch chân cắm". Bạn có cổng mới (`play()`), có máy cũ (`playFile()`) và không được đập cái nào ra sửa $\rightarrow$ Cần một đầu chuyển (Adapter).
    

#### 🩺 Tình huống 4:

> _"Hệ thống cần tạo bản sao của một đối tượng cấu hình để chỉnh sửa mà không làm thay đổi bản gốc."_

- **Pattern phù hợp:** **`Prototype`**
    
- **Tại sao:** Từ khóa _"tạo bản sao"_ và _"không làm thay đổi bản gốc"_ chính là công việc của việc nhân bản (Clone) trong Prototype.
    

---

### 🛠️ "Mớ đồ nghề" tổng hợp cho Bài 5:

Vì bài này gộp cả 4 pattern, file code sẽ khá dài. Bạn hãy chia nhỏ ra viết từng phần một:

#### 1. Đồ nghề cho Logger (Singleton)

- Tạo class `Logger`.
    
- Khóa constructor: `private Logger() {}`
    
- Tạo biến tĩnh: `private static Logger instance;`
    
- Viết hàm `public static synchronized Logger getInstance()` để cấp phát đối tượng.
    

#### 2. Đồ nghề cho Export (Factory Method)

- Tạo `interface Export` có hàm `void exportData();`.
    
- Tạo `PdfExport` và `ExcelExport` cùng `implements Export`.
    
- Tạo `abstract class ExportFactory` có hàm `abstract Export createExport();`.
    
- Tạo 2 xưởng con: `PdfExportFactory` và `ExcelExportFactory` để `return` về đúng đối tượng.
    

#### 3. Đồ nghề cho Player (Adapter)

- Giữ nguyên `class OldPlayer` và `interface Player` như đề bài cho.
    
- Tạo class `PlayerAdapter implements Player`.
    
- Trong Adapter, khai báo `private OldPlayer oldPlayer;`.
    
- Hàm `play(String name)` mới sẽ gọi `oldPlayer.playFile(name)`.
    

#### 4. Đồ nghề cho Cấu hình (Prototype)

- Tạo class `AppConfiguration implements Cloneable`.
    
- Khai báo vài thuộc tính mẫu (ví dụ: `String theme`).
    
- Viết đè hàm `public AppConfiguration clone()` và gọi `super.clone()`.
    

---

### 🚀 Gợi ý viết hàm `main` để kiểm tra toàn bộ:

Trong hàm `main`, bạn sẽ duyệt qua từng bài toán như thế này:



```java
public static void main(String[] args) {
    System.out.println("--- 1. TEST SINGLETON ---");
    Logger log1 = Logger.getInstance();
    Logger log2 = Logger.getInstance();
    System.out.println("Cùng 1 đối tượng? " + (log1 == log2));

    System.out.println("\n--- 2. TEST FACTORY METHOD ---");
    ExportFactory factory = new PdfExportFactory(); // Hoặc ExcelExportFactory
    Export exporter = factory.createExport();
    exporter.exportData();

    System.out.println("\n--- 3. TEST ADAPTER ---");
    Player player = new PlayerAdapter(new OldPlayer());
    player.play("song.mp3");

    System.out.println("\n--- 4. TEST PROTOTYPE ---");
    AppConfiguration goc = new AppConfiguration("Dark Mode");
    AppConfiguration saoChep = goc.clone();
    saoChep.setTheme("Light Mode");
    System.out.println("Gốc vẫn là: " + goc.getTheme());
}
```

---

Thế là chúng ta đã "giải mã" xong toàn bộ 5 bài tập mẫu thiết kế cực kỳ chất lượng này rồi! Bạn thấy đấy, khi nắm được bản chất và "mớ đồ nghề" cần dùng, việc viết code Design Patterns không hề đáng sợ như người ta đồn thổi đúng không?