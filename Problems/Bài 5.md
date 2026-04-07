Bài 4 này chia làm hai phần riêng biệt. Mình sẽ tách nhỏ từng phần, "dịch" sang ngôn ngữ đời thực và bày sẵn đồ nghề cho bạn trên VS Code như các bài trước nhé!

---

## 4(a). Mẫu Adapter (Bộ chuyển đổi)

> 💡 **Bản chất:** Bạn có một chiếc điện thoại iPhone chân sạc Lightning, nhưng ổ cắm ở quán cafe lại là Type-C. Bạn không thể đập cái iPhone ra sửa, cũng không thể đập ổ cắm của quán. Bạn cần một **đầu chuyển đổi (Adapter)** cắm vào giữa.

### 📝 Tóm tắt đề bài "Đầu chuyển sạc"

Hệ thống mới của bạn quy định mọi thứ sắp xếp phải đi qua cái cổng tên là `Sorter` với hàm `sort()`. Nhưng ngặt nỗi bạn lụm được một thư viện cũ cực xịn (`LegacySorter`) dùng thuật toán QuickSort nhưng cái hàm của nó lại tên là `quickSort()`.

- **Nhiệm vụ:** Viết một class trung gian `SorterAdapter`. Class này bên ngoài thì mang vỏ bọc của chuẩn mới (`Sorter`), nhưng bên trong ruột lại âm thầm gọi cái hàm của thư viện cũ (`LegacySorter`) để xử lý.
    

### 🛠️ "Mớ đồ nghề" cho phần Adapter:

#### 1. Đồ nghề chuẩn bị (Đề bài cho sẵn)

- **`interface Sorter`**: Có hàm `int[] sort(int[] arr);`.
    
- **`class LegacySorter`**: Có hàm `public int[] quickSort(int[] arr) { ... }`. (Trong thân hàm này bạn cứ viết đại một vòng lặp hoặc dùng `Arrays.sort(arr)` để giả lập sắp xếp nhé).
    

#### 2. Đồ nghề làm "Đầu chuyển" (Adapter)

- Tạo class **`SorterAdapter implements Sorter`**.
    
- Bên trong class này, bạn khai báo một biến để giữ cái máy cũ: `private LegacySorter legacySorter;`.
    
- Viết hàm khởi tạo để nạp máy cũ vào:
    
    Java
    
    ```
    public SorterAdapter(LegacySorter legacySorter) {
        this.legacySorter = legacySorter;
    }
    ```
    
- Viết đè lại hàm của chuẩn mới và lén lút gọi hàm cũ:
    
    Java
    
    ```
    @Override
    public int[] sort(int[] arr) {
        // Nhờ máy cũ xử lý hộ
        return legacySorter.quickSort(arr); 
    }
    ```
    

---

## 4(b). Mẫu Prototype (Nhân bản vô tính)

> 💡 **Bản chất:** Bạn cần làm $100$ cái báo cáo có khung xương giống hệt nhau, chỉ khác mỗi cái tên tiêu đề. Thay vì ngồi gõ `new ReportTemplate()` $100$ lần rồi set lại từng thuộc tính (rất tốn tài nguyên), bạn tạo ra $1$ cái chuẩn rồi bấm nút **"Copy - Paste" (Clone)** ra $99$ cái còn lại.

### 📝 Tóm tắt đề bài "Bản photocopy"

Bạn cần tạo ra một class mẫu báo cáo `ReportTemplate`. Sau đó viết cơ chế sao cho từ một file gốc, bạn "đẻ" ra được các file con giống hệt. Khi bạn sửa file con, file gốc vẫn nguyên vẹn không bị ảnh hưởng.

### 🛠️ "Mớ đồ nghề" cho phần Prototype:

#### 1. Đồ nghề tạo khuôn mẫu

- Tạo class `ReportTemplate` chứa 3 thuộc tính đề bài yêu cầu.
    
- Để class này có thể "nhân bản", trong Java cách chuẩn nhất là bạn cho nó **`implements Cloneable`**.
    
- Viết đè lại hàm `clone()` của Java:
    
    Java
    
    ```
    @Override
    public ReportTemplate clone() {
        try {
            ReportTemplate cloned = (ReportTemplate) super.clone();
            // CỰC KỲ QUAN TRỌNG: Vì 'sections' là một List (đối tượng), 
            // bạn cần tạo một List mới cho bản sao để tránh việc 
            // sửa list ở bản sao làm hỏng list ở bản gốc! (Deep Clone)
            cloned.sections = new ArrayList<>(this.sections);
            return cloned;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // Không bao giờ xảy ra vì đã implements Cloneable
        }
    }
    ```
    

#### 2. Đồ nghề chạy thử trong hàm `main`

Java

```
// 1. Tạo file gốc
ReportTemplate goc = new ReportTemplate("Báo cáo gốc", "Hết", List.of("Phần 1", "Phần 2"));

// 2. Nhân bản
ReportTemplate banSao1 = goc.clone();
ReportTemplate banSao2 = goc.clone();

// 3. Chỉnh sửa bản sao
banSao1.setTitle("Báo cáo số 1");
banSao2.setTitle("Báo cáo số 2");

// 4. In ra cả 3 để kiểm tra xem "goc" có còn giữ nguyên Title là "Báo cáo gốc" không.
```

---