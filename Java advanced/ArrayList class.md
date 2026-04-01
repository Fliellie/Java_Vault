## Lớp `ArrayList` trong Java: Mảng Động Tiện Lợi

Trong Java, **`java.util.ArrayList`** là một trong những lớp được sử dụng phổ biến nhất. Nó là một phần của Java Collections Framework và triển khai (implements) giao diện `List`.

Bạn có thể hình dung `ArrayList` giống như một mảng (`array`) thông thường, nhưng được nâng cấp với khả năng **tự động co giãn kích thước**. Khi bạn thêm hoặc xóa phần tử, `ArrayList` sẽ tự động điều chỉnh bộ nhớ ở hậu trường để chứa vừa vặn dữ liệu của bạn.

### 1. Các Đặc Điểm Cốt Lõi

- **Kích thước động (Dynamic Sizing):** Khác với mảng truyền thống có kích thước cố định ngay từ lúc khai báo, `ArrayList` có thể lớn lên hoặc thu nhỏ lại tùy ý trong quá trình chương trình chạy.
    
- **Truy cập qua chỉ số (Index-based):** Giống như mảng, các phần tử trong `ArrayList` được lưu trữ theo thứ tự và bạn có thể truy cập cực nhanh vào bất kỳ phần tử nào thông qua chỉ số của nó (bắt đầu từ `0`).
    
- **Cho phép trùng lặp và `null`:** Bạn có thể thêm bao nhiêu phần tử giống nhau tùy thích, và hoàn toàn có thể thêm giá trị `null` vào danh sách.
    
- **Chỉ chứa Đối tượng (Objects):** Như chúng ta đã bàn ở phần Generics và Wrapper Class, `ArrayList` không thể chứa các kiểu nguyên thủy (như `int`, `double`). Bạn bắt buộc phải dùng các Lớp Bao Bọc (như `Integer`, `Double`).
    
- **Không an toàn luồng (Non-synchronized):** `ArrayList` không được đồng bộ hóa. Nếu nhiều luồng (threads) cùng lúc sửa đổi một `ArrayList`, dữ liệu có thể bị sai lệch. (Nếu cần đa luồng, bạn nên dùng `Vector` hoặc `CopyOnWriteArrayList`).
    

---

### 2. Cơ Chế Hoạt Động Ngầm (Under the Hood)

Làm thế nào `ArrayList` có thể "co giãn"? Bản chất bên trong `ArrayList` vẫn là một **mảng `Object[]` bình thường**.

1. **Khởi tạo:** Khi bạn tạo một `ArrayList` rỗng (`new ArrayList<>()`), nó thường tạo ngầm một mảng có sức chứa (capacity) mặc định là 10.
    
2. **Khi mảng đầy:** Nếu bạn cố thêm phần tử thứ 11, `ArrayList` sẽ tự động làm 3 việc:
    
    - Tạo ra một mảng mới lớn hơn (thường là gấp rưỡi, tức tăng thêm 50% sức chứa).
        
    - Sao chép toàn bộ dữ liệu từ mảng cũ sang mảng mới.
        
    - Vứt bỏ mảng cũ (để Garbage Collector dọn dẹp) và thêm phần tử thứ 11 vào mảng mới.
        

Quá trình này diễn ra hoàn toàn tự động, giúp bạn rảnh tay không phải lo lắng về việc quản lý bộ nhớ thủ công.

---

### 3. So Sánh Nhanh: Mảng Thường (Array) vs `ArrayList`

Đây là kiến thức cực kỳ quan trọng để bạn quyết định lúc nào nên dùng cái nào:

|**Tiêu chí**|**Mảng Thường ([])**|**ArrayList**|
|---|---|---|
|**Kích thước**|Cố định (Không thể thay đổi sau khi tạo)|Động (Tự động co giãn)|
|**Kiểu dữ liệu**|Chứa được cả Kiểu Nguyên Thủy và Đối tượng|**Chỉ chứa được Đối tượng** (Phải dùng Wrapper classes)|
|**Tốc độ**|Rất nhanh (Đặc biệt cho kiểu nguyên thủy)|Chậm hơn một chút (Do chi phí co giãn mảng và boxing/unboxing)|
|**Lấy kích thước**|Thuộc tính `length` (Ví dụ: `mang.length`)|Phương thức `size()` (Ví dụ: `danhSach.size()`)|
|**Thêm/Xóa phần tử**|Phức tạp (Phải tự viết code dịch chuyển dữ liệu)|Rất dễ (Có sẵn các hàm `add()`, `remove()`)|

---

### 4. Các Phương Thức Phổ Biến Nhất

|**Phương thức**|**Chức năng**|**Ví dụ**|
|---|---|---|
|`add(E e)`|Thêm phần tử vào cuối danh sách|`list.add("Java")`|
|`add(int index, E e)`|Chèn phần tử vào một vị trí cụ thể|`list.add(0, "Python")`|
|`get(int index)`|Lấy phần tử tại vị trí chỉ định|`String s = list.get(0)`|
|`set(int index, E e)`|Thay thế/Sửa phần tử tại vị trí chỉ định|`list.set(1, "C++")`|
|`remove(int index)`|Xóa phần tử tại vị trí chỉ định|`list.remove(0)`|
|`remove(Object o)`|Xóa phần tử đầu tiên khớp với giá trị truyền vào|`list.remove("Java")`|
|`size()`|Trả về số lượng phần tử hiện có|`int count = list.size()`|
|`clear()`|Xóa sạch toàn bộ phần tử trong danh sách|`list.clear()`|
|`contains(Object o)`|Kiểm tra xem danh sách có chứa phần tử này không|`boolean coKhong = list.contains("C++")`|

---

### 5. Code Mẫu Ứng Dụng Thực Tế

Dưới đây là cách khởi tạo và thao tác với `ArrayList` trong thực tế:

Java

```java
import java.util.ArrayList;

public class BaiTapArrayList {
    public static void main(String[] args) {
        
        // 1. Khởi tạo ArrayList chứa các số nguyên (Phải dùng Integer)
        ArrayList<Integer> diemSo = new ArrayList<>();

        // 2. Thêm phần tử (Auto-boxing sẽ tự động biến int thành Integer)
        diemSo.add(8);
        diemSo.add(9);
        diemSo.add(10);
        diemSo.add(1, 7); // Chèn số 7 vào vị trí index 1

        System.out.println("Danh sách ban đầu: " + diemSo); // In ra: [8, 7, 9, 10]

        // 3. Sửa phần tử
        diemSo.set(0, 5); // Đổi phần tử ở index 0 thành 5
        
        // 4. Lấy phần tử và Xóa phần tử
        int diemDauTien = diemSo.get(0);
        diemSo.remove(3); // Xóa phần tử ở index 3 (số 10)

        // 5. Duyệt qua ArrayList bằng vòng lặp for-each (Cách khuyên dùng)
        System.out.print("Duyệt danh sách: ");
        for (Integer diem : diemSo) {
            System.out.print(diem + " ");
        }
        System.out.println(); // Xuống dòng

        // 6. Một số hàm tiện ích khác
        System.out.println("Số lượng điểm: " + diemSo.size());
        System.out.println("Có điểm 9 không? " + diemSo.contains(9));
        
        // Làm sạch danh sách
        diemSo.clear();
        System.out.println("Danh sách sau khi clear có rỗng không? " + diemSo.isEmpty());
    }
}
```