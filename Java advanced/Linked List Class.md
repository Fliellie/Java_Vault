## Lớp `LinkedList` trong Java: Danh Sách Liên Kết Kép

Nếu `ArrayList` là một mảng có thể tự động co giãn, thì **`java.util.LinkedList`** lại mang một triết lý thiết kế hoàn toàn khác. Mặc dù cả hai đều dùng để lưu trữ danh sách và triển khai giao diện `List`, cấu trúc bên trong của `LinkedList` lại dựa trên **Danh sách liên kết kép (Doubly-Linked List)**.

### 1. Cơ Chế Hoạt Động (Cách các mắt xích nối với nhau)

`LinkedList` **không** sử dụng một mảng liên tục trong bộ nhớ để chứa dữ liệu. Thay vào đó, nó lưu trữ dữ liệu trong các đối tượng riêng biệt được gọi là **Node (Mắt xích / Nút)**. Các Node này nằm rải rác trong bộ nhớ.

Mỗi Node trong `LinkedList` của Java chứa 3 thành phần:

1. **Dữ liệu thực tế (Item):** Giá trị bạn muốn lưu (ví dụ: "Java", số `10`).
    
2. **Con trỏ `next`:** Trỏ đến Node tiếp theo trong danh sách.
    
3. **Con trỏ `prev`:** Trỏ đến Node đứng ngay trước nó.
    

Nhờ có hai con trỏ `prev` và `next`, Java có thể duyệt qua danh sách này từ đầu đến cuối hoặc từ cuối lên đầu một cách dễ dàng.

---

### 2. Sự Đa Năng Tuyệt Vời Của LinkedList

Một điểm cực kỳ thú vị của `LinkedList` là nó không chỉ `implements List`, mà nó còn triển khai giao diện **`Deque`** (Double Ended Queue - Hàng đợi hai đầu).

Điều này biến `LinkedList` thành một "con dao pha" thực thụ:

- Bạn có thể dùng nó như một **Danh sách (List)** bình thường.
    
- Bạn có thể dùng nó như một **Hàng đợi (Queue)** (Vào trước - Ra trước / FIFO).
    
- Bạn có thể dùng nó như một **Ngăn xếp (Stack)** (Vào sau - Ra trước / LIFO) thay cho lớp `Stack` cũ kỹ.
    

---

### 3. Trận Chiến Kinh Điển: `LinkedList` vs `ArrayList`

Đây là câu hỏi phỏng vấn "câu cơm" mà bất kỳ lập trình viên Java nào cũng phải thuộc nằm lòng. Lúc nào thì dùng cái nào?

|Tiêu chí|`ArrayList` (Dựa trên Mảng)|`LinkedList` (Dựa trên Con trỏ)|
|---|---|---|
|**Truy cập phần tử (get, set)**|**Cực nhanh (O(1))** vì nó biết chính xác vị trí trong bộ nhớ qua chỉ số (index).|**Rất chậm (O(n))** vì phải dò từng mắt xích từ đầu (hoặc cuối) cho đến khi tìm thấy.|
|**Thêm/Xóa ở Giữa danh sách**|**Chậm (O(n))** vì phải dịch chuyển hàng loạt các phần tử phía sau lên/xuống.|**Cực nhanh (O(1))** vì chỉ cần bẻ gãy và nối lại 2 con trỏ, không cần dịch chuyển ai cả.|
|**Thêm ở Đầu/Cuối**|Nhanh ở cuối, chậm ở đầu.|**Cực nhanh (O(1))** ở cả hai đầu.|
|**Tiêu tốn bộ nhớ**|Ít tốn hơn (chỉ lưu dữ liệu).|**Tốn nhiều hơn** (phải lưu thêm 2 con trỏ `prev` và `next` cho mỗi phần tử).|

**Chốt lại:**

- Dùng **`ArrayList`** khi bạn chủ yếu **Đọc/Truy xuất** dữ liệu hoặc chỉ thêm dữ liệu vào cuối danh sách. (Chiếm 95% trường hợp thực tế).
    
- Dùng **`LinkedList`** khi bài toán của bạn yêu cầu **Thêm/Xóa liên tục** ở vị trí đầu tiên hoặc ở giữa danh sách, và bạn ít khi cần truy cập ngẫu nhiên qua index.
    

---

### 4. Các Phương Thức Đặc Trưng (Bên Cạnh Các Hàm Của List)

Vì `LinkedList` triển khai `Deque`, nó cung cấp rất nhiều hàm chuyên dụng để thao tác cực nhanh với hai đầu của danh sách:

- **Thêm:** `addFirst(E e)`, `addLast(E e)`, `push(E e)` (tương đương addFirst).
    
- **Lấy và Xóa:** `removeFirst()`, `removeLast()`, `pop()` (tương đương removeFirst), `poll()` (lấy và xóa phần tử đầu tiên, trả về null nếu rỗng).
    
- **Chỉ Lấy (Không Xóa):** `getFirst()`, `getLast()`, `peek()` (xem phần tử đầu tiên).
    

---

### 5. Ví Dụ Minh Họa

Java

```java
import java.util.LinkedList;

public class BaiTapLinkedList {
    public static void main(String[] args) {
        // Khởi tạo một LinkedList
        LinkedList<String> doanTau = new LinkedList<>();

        // 1. Thêm các toa tàu (hoạt động như List bình thường)
        doanTau.add("Toa Khách 1");
        doanTau.add("Toa Khách 2");
        
        // 2. Sử dụng sức mạnh của Deque (Thêm vào 2 đầu cực nhanh)
        doanTau.addFirst("Đầu Tàu"); // Gắn lên đầu
        doanTau.addLast("Toa Hàng"); // Gắn xuống đít
        
        System.out.println("Đoàn tàu hiện tại: " + doanTau);
        // In ra: [Đầu Tàu, Toa Khách 1, Toa Khách 2, Toa Hàng]

        // 3. Lấy ra và xóa phần tử ở 2 đầu
        String toaDau = doanTau.removeFirst();
        System.out.println("Vừa tháo: " + toaDau);
        
        // 4. Lấy phần tử cuối mà KHÔNG xóa
        System.out.println("Toa cuối cùng là: " + doanTau.getLast());
        
        // Lệnh này chậm vì phải chạy từ Đầu Tàu đến vị trí số 1
        System.out.println("Toa ở giữa: " + doanTau.get(1)); 
    }
}
```