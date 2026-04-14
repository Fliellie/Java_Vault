 Khi đã nhận ra sự bất tiện của mảng (Array) với kích thước cố định, **Java Collections Framework (JCF)** chính là "kho vũ khí" tối thượng mà mọi lập trình viên Java đều phải thành thạo.

Giao diện `Collection` là gốc rễ của hầu hết các cấu trúc dữ liệu động trong Java (nằm trong gói `java.util`). Nó giúp bạn lưu trữ, thêm, xóa và tìm kiếm dữ liệu một cách linh hoạt mà không cần bận tâm về kích thước ban đầu.

Dưới đây là bức tranh toàn cảnh về giao diện `Collection` và các nhánh con của nó.

---

## 1. Bức tranh tổng thể: Các nhánh chính của Collection

Giao diện `Collection` phân nhánh thành 3 giao diện con (sub-interfaces) chính, mỗi nhánh giải quyết một bài toán lưu trữ khác nhau:

### 📜 Nhánh 1: Giao diện `List` (Danh sách)

Đây là cấu trúc dữ liệu phổ biến nhất. Nó giống như một mảng có thể tự động co giãn.

- **Đặc điểm:** Giữ nguyên thứ tự khi thêm vào (Insertion Order) và **cho phép các phần tử trùng lặp** (Duplicates allowed).
    
- **Các lớp con tiêu biểu (Implementations):**
    
    - `ArrayList`: Sử dụng mảng động bên dưới. **Cực kỳ nhanh khi truy xuất dữ liệu** (đọc), nhưng chậm khi thêm/xóa phần tử ở giữa danh sách vì phải dịch chuyển các phần tử khác.
        
    - `LinkedList`: Sử dụng danh sách liên kết kép (Doubly Linked List). **Rất nhanh khi thêm/xóa phần tử**, nhưng chậm khi truy xuất vì phải duyệt từ đầu hoặc cuối mảng để tìm.
        
    - `Vector`: Giống `ArrayList` nhưng an toàn trong môi trường đa luồng (Thread-safe). Tuy nhiên, hiện nay ít được dùng vì hiệu năng kém, thường được thay thế bằng các giải pháp hiện đại hơn.
        

### 🎯 Nhánh 2: Giao diện `Set` (Tập hợp)

Mô phỏng khái niệm "Tập hợp" trong toán học.

- **Đặc điểm:** **TUYỆT ĐỐI KHÔNG cho phép các phần tử trùng lặp**. Nếu bạn cố thêm một phần tử đã tồn tại, nó sẽ bị phớt lờ. Thứ tự lưu trữ phụ thuộc vào lớp con.
    
- **Các lớp con tiêu biểu:**
    
    - `HashSet`: Sử dụng bảng băm (Hash table). Nhanh nhất trong các loại Set. **Không đảm bảo bất kỳ thứ tự nào** khi duyệt.
        
    - `LinkedHashSet`: Là con của `HashSet` nhưng duy trì thêm một danh sách liên kết. Đảm bảo **thứ tự thêm vào** được giữ nguyên.
        
    - `TreeSet`: Lưu trữ dữ liệu dưới dạng cây nhị phân (Red-Black tree). Các phần tử sẽ được **tự động sắp xếp tăng dần** (theo bảng chữ cái hoặc từ điển).
        

### 🚶 Nhánh 3: Giao diện `Queue` (Hàng đợi)

Mô phỏng việc xếp hàng ngoài đời thực.

- **Đặc điểm:** Hoạt động theo nguyên tắc **FIFO** (First-In-First-Out: Vào trước ra trước). Người xếp hàng đầu tiên sẽ được phục vụ đầu tiên.
    
- **Các lớp con tiêu biểu:**
    
    - `PriorityQueue`: Hàng đợi ưu tiên. Các phần tử không xếp theo thứ tự thêm vào mà xếp theo "mức độ ưu tiên" (do bạn định nghĩa, ví dụ: khách hàng VIP luôn được đẩy lên đầu hàng).
        
    - `LinkedList`: Bất ngờ chưa? Lớp này vừa triển khai `List`, vừa triển khai `Queue` (cụ thể là `Deque` - hàng đợi hai đầu), nên nó có thể dùng làm hàng đợi rất tốt.
        ![[Pasted image 20260402074035.png]]
        
        

---

## 2. ⚠️ Lưu ý "Chí mạng" khi phỏng vấn: Giao diện `Map`

Trong hệ sinh thái Java Collections Framework, có một thành viên cực kỳ quan trọng là `Map` (ví dụ: `HashMap`, `TreeMap`). Tuy nhiên, bạn phải nhớ kỹ: **`Map` KHÔNG kế thừa từ giao diện `Collection`**.

Nó là một nhánh hoàn toàn độc lập chuyên lưu trữ dữ liệu theo cặp **Khóa - Giá trị (Key - Value)**, giống như một cuốn từ điển vậy.

---

## 3. Bảng tổng kết "Chọn đúng vũ khí"

Để quyết định dùng cấu trúc nào cho bài toán của bạn, hãy xem bảng đối chiếu nhanh sau:

|**Giao diện**|**Có cho phép trùng lặp không?**|**Có giữ thứ tự thêm vào không?**|**Khi nào nên dùng?**|
|---|---|---|---|
|**List**|Có|Có|Cần một danh sách tự giãn, truy cập qua vị trí (index), chấp nhận trùng lặp.|
|**Set**|Không|Tùy lớp con (`HashSet`: Không, `LinkedHashSet`: Có)|Cần lọc dữ liệu trùng lặp (ví dụ: danh sách mã sinh viên duy nhất).|
|**Queue**|Thường là có|Tùy lớp con (Thường là FIFO)|Cần xử lý dữ liệu theo thứ tự xếp hàng (ví dụ: hàng đợi tin nhắn, xử lý tiến trình).|

Bạn muốn chúng ta đào sâu vào viết code thực hành cho `ArrayList` (kẻ thống trị mảng động) hay thử sức với `HashSet` để xem cách nó loại bỏ dữ liệu trùng lặp vi diệu như thế nào?