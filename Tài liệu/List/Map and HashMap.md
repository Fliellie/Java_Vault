## Giao Diện `Map` và Lớp `HashMap` trong Java: Từ Điển Tra Cứu Siêu Tốc

Khác với `List` (danh sách) hay `Set` (tập hợp) chỉ lưu trữ từng phần tử đơn lẻ, **`Map`** trong Java mang đến một cách lưu trữ dữ liệu hoàn toàn khác: **Lưu trữ theo cặp Khóa - Giá trị (Key - Value)**.

Bạn có thể hình dung `Map` giống hệt như một cuốn từ điển hay danh bạ điện thoại. Trong danh bạ, Tên của bạn là "Khóa" (Key), còn Số điện thoại là "Giá trị" (Value). Khi cần tìm số điện thoại, bạn không cần duyệt qua từng trang, bạn chỉ cần tìm đúng Tên là sẽ ra ngay Số điện thoại.

### 1. Đặc Điểm Cốt Lõi Của `Map`

Mặc dù thuộc Java Collections Framework, nhưng giao diện `java.util.Map` **không** kế thừa từ giao diện `Collection`. Nó tự xây dựng một đế chế riêng với các quy tắc sau:

- **Khóa (Key) là duy nhất:** Bạn không thể có 2 Khóa giống hệt nhau trong cùng một `Map`. Nếu bạn cố tình thêm một Khóa đã tồn tại, Giá trị cũ sẽ bị đè (thay thế) bằng Giá trị mới.
    
- **Giá trị (Value) có thể trùng lặp:** Nhiều Khóa khác nhau hoàn toàn có thể trỏ đến cùng một Giá trị (Giống như việc hai người khác nhau có thể dùng chung một số điện thoại bàn).
    
- **Chỉ lưu Đối tượng:** Tương tự `ArrayList`, `Map` yêu cầu Key và Value phải là các Đối tượng (Object), ví dụ: `Map<String, Integer>`. Không được dùng kiểu nguyên thủy như `int`, `double`.
    

---

### 2. Lớp `HashMap` - "Ngôi Sao" Của Giao Diện Map

Có nhiều lớp triển khai giao diện `Map` (như `TreeMap`, `LinkedHashMap`), nhưng **`java.util.HashMap`** là lớp phổ biến nhất và được sử dụng rộng rãi nhất.

**Đặc điểm nổi bật của `HashMap`:**

- **Tốc độ Bàn Thờ (O(1)):** `HashMap` sử dụng một kỹ thuật gọi là **Hashing (Băm)** ở hậu trường. Nhờ vậy, thao tác thêm dữ liệu (`put`) và lấy dữ liệu (`get`) diễn ra cực kỳ nhanh, gần như ngay lập tức, bất kể `HashMap` của bạn đang chứa 10 phần tử hay 1 triệu phần tử.
    
- **Không đảm bảo thứ tự:** `HashMap` không quan tâm bạn thêm các phần tử vào theo thứ tự nào. Khi bạn in nó ra, thứ tự các cặp Key-Value có thể bị xáo trộn hoàn toàn so với lúc bạn nhập vào.
    
- **Chấp nhận `null`:** `HashMap` cho phép bạn sử dụng **một** Khóa có giá trị `null` và **nhiều** Giá trị là `null`.
    
- **Không an toàn luồng (Non-synchronized):** Tương tự `ArrayList`, `HashMap` không phù hợp cho môi trường đa luồng nếu không có cơ chế khóa (synchronized) đi kèm.
    

---

### 3. Bảng Các Phương Thức "Nhẵn Mặt" Của `Map`

Thay vì dùng `add()` và `get(index)` như `List`, `Map` có bộ hàm riêng biệt:

|**Phương thức**|**Chức năng**|**Ví dụ**|
|---|---|---|
|`put(K key, V value)`|Thêm một cặp Khóa-Giá trị vào Map. Nếu Khóa đã tồn tại, nó sẽ đè Giá trị mới lên.|`map.put("SV01", "Nguyễn Văn A")`|
|`get(Object key)`|Lấy ra Giá trị tương ứng với Khóa. Trả về `null` nếu không tìm thấy Khóa.|`String ten = map.get("SV01")`|
|`remove(Object key)`|Xóa cả cặp Khóa-Giá trị khỏi Map dựa vào Khóa.|`map.remove("SV01")`|
|`containsKey(Object key)`|Kiểm tra xem Map có chứa Khóa này không (Trả về `true`/`false`). Rất hay dùng!|`map.containsKey("SV02")`|
|`containsValue(Object val)`|Kiểm tra xem Map có chứa Giá trị này không.|`map.containsValue("Nguyễn Văn A")`|
|`keySet()`|Trả về một **Tập hợp (`Set`)** chứa toàn bộ các **Khóa**. Dùng để duyệt qua Map.|`Set<String> keys = map.keySet()`|
|`values()`|Trả về một **Danh sách (`Collection`)** chứa toàn bộ các **Giá trị**.|`Collection<String> vals = map.values()`|

---

### 4. Code Mẫu: Quản Lý Điểm Thi Bằng HashMap

Dưới đây là một ví dụ thực tế chỉ ra cách thêm, lấy và đặc biệt là **cách duyệt (iterate)** qua một `HashMap` - thứ thường làm khó người mới học:

Java

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class BaiTapHashMap {
    public static void main(String[] args) {
        
        // 1. Khởi tạo HashMap với Key là String (Mã SV) và Value là Double (Điểm)
        Map<String, Double> bangDiem = new HashMap<>();

        // 2. Thêm dữ liệu bằng put()
        bangDiem.put("SV001", 8.5);
        bangDiem.put("SV002", 9.0);
        bangDiem.put("SV003", 7.5);
        
        // Cố tình put trùng Key "SV001" -> Điểm 8.5 sẽ bị đè thành 10.0
        bangDiem.put("SV001", 10.0); 

        System.out.println("Bảng điểm hiện tại: " + bangDiem);
        // Sẽ in ra: {SV003=7.5, SV001=10.0, SV002=9.0} (Thứ tự bị xáo trộn!)

        // 3. Truy xuất dữ liệu bằng get()
        System.out.println("Điểm của SV002 là: " + bangDiem.get("SV002"));

        // 4. Kiểm tra tồn tại
        if (bangDiem.containsKey("SV099")) {
            System.out.println("Có sinh viên này!");
        } else {
            System.out.println("Không tìm thấy mã SV099.");
        }

        System.out.println("\n--- CÁCH DUYỆT QUA HASHMAP ---");
        
        // CÁCH 1: Duyệt qua từng Khóa (Key) bằng keySet()
        Set<String> danhSachMaSV = bangDiem.keySet();
        for (String maSV : danhSachMaSV) {
            Double diem = bangDiem.get(maSV); // Có Khóa rồi thì lấy Giá trị
            System.out.println("Mã SV: " + maSV + " - Điểm: " + diem);
        }

        // CÁCH 2: Duyệt trực tiếp cả Cặp (Entry) - Nhanh và Khuyên dùng hơn
        System.out.println("\n--- DUYỆT BẰNG ENTRYSET ---");
        for (Map.Entry<String, Double> capDuLieu : bangDiem.entrySet()) {
            System.out.println("Khóa: " + capDuLieu.getKey() + " -> Giá trị: " + capDuLieu.getValue());
        }
    }
}
```

Bạn có muốn tìm hiểu sâu hơn về cơ chế **"Hashing" (Băm)** cực kỳ thông minh mà `HashMap` sử dụng ở hậu trường để đạt được tốc độ truy xuất $O(1)$ không?