## Lớp `Stack` trong Java: Cấu Trúc Dữ Liệu "Vào Sau - Ra Trước" (LIFO)

Trong Java, **`java.util.Stack`** là một lớp đại diện cho cấu trúc dữ liệu **Ngăn xếp**. Nguyên lý hoạt động cốt lõi của Stack được gọi là **LIFO** (Last-In, First-Out), nghĩa là phần tử nào được đưa vào cuối cùng sẽ là phần tử được lấy ra đầu tiên.

Hãy tưởng tượng `Stack` giống như một chồng đĩa trong quán ăn: Bạn chỉ có thể đặt một chiếc đĩa mới lên trên cùng của chồng đĩa, và khi cần lấy đĩa ra để dùng, bạn cũng chỉ có thể lấy chiếc đĩa nằm ở trên cùng đó.

### 1. Đặc Điểm Quan Trọng

- **Kế thừa từ `Vector`:** Lớp `Stack` là một lớp con của `Vector`. Điều này có nghĩa là nó kế thừa mọi đặc tính của `Vector`, bao gồm khả năng tự động co giãn kích thước và **an toàn luồng (Thread-safe / Synchronized)**.
    
- **Hoạt động theo LIFO:** Mọi thao tác thêm và xóa đều diễn ra ở một đầu duy nhất (gọi là đỉnh - top của ngăn xếp).
    
- **Cho phép `null` và trùng lặp:** Giống như `ArrayList` hay `Vector`, `Stack` cho phép chứa các phần tử giống nhau và cả giá trị `null`.
    

---

### 2. 5 Phương Thức Độc Quyền Của Stack

Bên cạnh các phương thức kế thừa từ `Vector` (như `add()`, `get()`, `remove()`), lớp `Stack` cung cấp 5 phương thức đặc trưng để thao tác đúng chuẩn ngăn xếp:

|Phương thức|Chức năng|Hành động tương ứng|
|---|---|---|
|**`push(E item)`**|Thêm một phần tử lên **đỉnh** của ngăn xếp.|Đặt thêm một cái đĩa lên trên cùng.|
|**`pop()`**|Lấy phần tử ở **đỉnh** ngăn xếp ra và **xóa** nó khỏi ngăn xếp. Ném lỗi `EmptyStackException` nếu ngăn xếp rỗng.|Lấy cái đĩa trên cùng ra xài.|
|**`peek()`**|Nhìn thử xem phần tử ở **đỉnh** ngăn xếp là gì **mà không xóa** nó.|Ngó xem cái đĩa trên cùng màu gì.|
|**`empty()`**|Kiểm tra xem ngăn xếp có rỗng hay không (Trả về `true` hoặc `false`).|Xem thử còn cái đĩa nào không.|
|**`search(Object o)`**|Tìm kiếm phần tử và trả về **vị trí tính từ đỉnh (bắt đầu từ 1, không phải 0)**. Trả về `-1` nếu không tìm thấy.|Đếm từ trên xuống xem cái đĩa màu xanh nằm ở vị trí thứ mấy.|

---

### 3. Ví dụ Minh Họa

Dưới đây là cách sử dụng các phương thức cơ bản của `Stack`:

Java

```java
import java.util.Stack;

public class BaiTapStack {
    public static void main(String[] args) {
        // Khởi tạo một ngăn xếp chứa các chuỗi (Tên trình duyệt)
        Stack<String> lichSuDuyetWeb = new Stack<>();

        // 1. push() - Thêm vào ngăn xếp (Giống như bạn đang lướt web)
        lichSuDuyetWeb.push("Google.com");
        lichSuDuyetWeb.push("Facebook.com");
        lichSuDuyetWeb.push("Youtube.com");

        System.out.println("Lịch sử hiện tại: " + lichSuDuyetWeb); 
        // In ra: [Google.com, Facebook.com, Youtube.com]
        // Đỉnh (top) hiện tại đang là Youtube.com

        // 2. peek() - Xem trang hiện tại đang mở (đỉnh ngăn xếp)
        System.out.println("Đang xem: " + lichSuDuyetWeb.peek()); // In ra Youtube.com

        // 3. search() - Tìm vị trí (Tính từ đỉnh, đỉnh là vị trí 1)
        int viTri = lichSuDuyetWeb.search("Google.com"); 
        System.out.println("Vị trí của Google: " + viTri); // In ra 3 (Từ Youtube -> Facebook -> Google)

        // 4. pop() - Nút "Back" trên trình duyệt (Lấy ra và xóa khỏi ngăn xếp)
        String trangVuaDong = lichSuDuyetWeb.pop();
        System.out.println("Vừa đóng trang: " + trangVuaDong); // In ra Youtube.com
        System.out.println("Trang quay lại: " + lichSuDuyetWeb.peek()); // In ra Facebook.com

        // 5. empty() - Kiểm tra rỗng
        System.out.println("Lịch sử có trống không? " + lichSuDuyetWeb.empty()); // false
    }
}
```

---

### 4. 🚨 LƯU Ý CỰC KỲ QUAN TRỌNG (Best Practice)

Dù lớp `Stack` vẫn tồn tại và hoạt động tốt, nhưng trong thực tế lập trình Java hiện đại, **người ta rất hiếm khi sử dụng lớp `java.util.Stack`**.

**Tại sao lại như vậy?**

1. **Quá chậm:** Vì kế thừa từ `Vector`, mọi phương thức của `Stack` đều bị khóa đồng bộ (`synchronized`). Việc này gây lãng phí tài nguyên và làm giảm hiệu suất rất lớn khi chạy trong môi trường đơn luồng (Single-thread).
    
2. **Thiết kế lỗi (Phá vỡ tính đóng gói):** Bản chất của ngăn xếp (Stack) là chỉ được phép thao tác ở Đỉnh. Nhưng do kế thừa `Vector`, một đối tượng `Stack` vẫn có thể gọi hàm `get(3)` hoặc `insertElementAt(0)` để chọc ngoáy vào giữa ngăn xếp. Điều này phá vỡ hoàn toàn tính chất LIFO nghiêm ngặt của nó.
    

**Giải pháp thay thế (Cách người chuyên nghiệp làm):** Chính tài liệu chính thức của Java (JavaDocs) khuyên lập trình viên nên sử dụng giao diện **`Deque`** (được triển khai bởi lớp **`ArrayDeque`**) để làm ngăn xếp thay cho lớp `Stack` cũ kỹ:

Java

```
import java.util.ArrayDeque;
import java.util.Deque;

public class StackHienDai {
    public static void main(String[] args) {
        // Đây là cách khai báo một Stack chuẩn và nhanh nhất trong Java hiện đại
        Deque<Integer> nganXep = new ArrayDeque<>();
        
        nganXep.push(10);
        nganXep.push(20);
        
        System.out.println(nganXep.pop()); // Vẫn dùng push, pop, peek bình thường!
    }
}
```

Sử dụng `ArrayDeque` không bị gánh nặng đồng bộ hóa (synchronized), nên tốc độ xử lý nhanh hơn rất nhiều so với lớp `Stack` truyền thống.