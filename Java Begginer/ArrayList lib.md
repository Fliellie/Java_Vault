Trong Java, **ArrayList** là một phần của Java Collections Framework (nằm trong gói `java.util`). Đây là một bảng trợ thủ đắc lực cho các lập trình viên vì nó khắc phục được nhược điểm lớn nhất của mảng (Array) truyền thống: **kích thước cố định.**

Hãy tưởng tượng `ArrayList` như một chiếc "mảng co giãn" (Resizable Array).

---

### 1. Sự khác biệt cốt lõi giữa Array và ArrayList

|**Đặc điểm**|**Mảng (Array)**|**ArrayList**|
|---|---|---|
|**Kích thước**|Cố định (phải khai báo ngay từ đầu).|Linh hoạt (tự động tăng/giảm khi thêm/xóa).|
|**Kiểu dữ liệu**|Lưu được cả kiểu nguyên thủy (`int`, `char`) và đối tượng.|**Chỉ lưu đối tượng** (phải dùng `Integer`, `Double` thay vì `int`, `double`).|
|**Tốc độ**|Nhanh hơn một chút do cấu trúc đơn giản.|Chậm hơn một chút do có thêm các phương thức quản lý.|

---

### 2. Cách thức hoạt động bên trong

Khi bạn thêm phần tử vào một `ArrayList` mà nó đã đầy, Java sẽ thực hiện các bước sau một cách âm thầm:

1. Tạo một mảng mới có kích thước lớn hơn (thường là gấp 1.5 lần mảng cũ).
    
2. Sao chép toàn bộ phần tử từ mảng cũ sang mảng mới.
    
3. Giải phóng mảng cũ.
    

Chính vì vậy, bạn không bao giờ phải lo lắng về việc bị "tràn" mảng.

---

### 3. Cách sử dụng cơ bản

Để dùng `ArrayList`, bạn cần import: `import java.util.ArrayList;`

Java

```
// 1. Khai báo một danh sách chứa các chuỗi (String)
ArrayList<String> cars = new ArrayList<>();

// 2. Thêm phần tử
cars.add("Volvo");
cars.add("BMW");
cars.add("Ford");

// 3. Truy cập phần tử (lưu ý: dùng get thay vì [ ])
String myCar = cars.get(0); // Kết quả: "Volvo"

// 4. Thay đổi phần tử
cars.set(0, "Toyota");

// 5. Xóa phần tử
cars.remove(1); // Xóa BMW

// 6. Lấy kích thước danh sách
int size = cars.size();
```

---

### 4. Khi nào nên dùng ArrayList?

- Khi bạn **không biết trước** có bao nhiêu dữ liệu sẽ được nhập vào.
    
- Khi bạn cần các tính năng tiện ích có sẵn như: sắp xếp (`sort`), tìm kiếm (`contains`), xóa hàng loạt.
    
- **Lưu ý:** Nếu bạn cần chèn hoặc xóa phần tử ở **giữa** danh sách cực kỳ nhiều lần với dữ liệu khổng lồ, `LinkedList` có thể sẽ hiệu quả hơn `ArrayList`.



## 1. Phân biệt List và ArrayList

- **List (Cái khung/Hợp đồng):** Là một `interface`. Nó chỉ đưa ra các "quy định" như: "Nếu anh muốn là một danh sách, anh phải có các phương thức `add`, `remove`, `get`...". Bản thân `List` không biết cách lưu trữ dữ liệu thế nào, nó chỉ là cái danh mục các tính năng.
    
- **ArrayList (Công cụ thực thi):** Là một `class` cụ thể. Nó triển khai các quy định của `List` bằng cách sử dụng một **mảng (array)** bên dưới lớp vỏ.
    

---

## 2. Tại sao lại khai báo `List<String> list = new ArrayList<>()`?

Hãy tưởng tượng bạn đang viết một chương trình quản lý vận chuyển.

#### Kịch bản A: Bạn dùng `ArrayList<String> list = new ArrayList<>()`

Bạn gắn chặt code của mình vào "Xe tải" (ArrayList). Sau này, sếp bảo: "Xe tải chạy tốn xăng quá, đổi sang Tàu hỏa (LinkedList) đi!".

Lúc này, vì bạn đã khai báo mọi biến, mọi tham số hàm là `ArrayList`, bạn sẽ phải đi lục lại hàng nghìn dòng code để sửa từ `ArrayList` thành `LinkedList`. **Rất mệt và dễ lỗi.**

#### Kịch bản B: Bạn dùng `List<String> list = new ArrayList<>()` (Tính đa hình)

Bạn chỉ nói: "Tôi cần một **Phương tiện vận chuyển (List)**".

- Hôm nay bạn chọn **Xe tải (ArrayList)**: `List list = new ArrayList();`
    
- Ngày mai bạn muốn đổi sang **Tàu hỏa (LinkedList)**: Bạn chỉ cần sửa đúng 1 dòng: `List list = new LinkedList();`
    

Tất cả các hàm phía sau như `list.add()` hay `list.size()` đều hoạt động bình thường vì cả Xe tải và Tàu hỏa đều tuân thủ quy định của "Phương tiện vận chuyển".

---

## 3. So sánh nhanh

| **Đặc điểm**       | **List (Interface)**                                                        | **ArrayList (Class)**                                         |
| ------------------ | --------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Bản chất**       | Là một bản thiết kế, không thể khởi tạo trực tiếp (`new List()` là lỗi).    | Là một thực thể cụ thể, dùng để lưu trữ dữ liệu thật.         |
| **Tính linh hoạt** | **Cao.** Giúp code của bạn "mềm dẻo", dễ thay đổi cấu trúc dữ liệu sau này. | **Thấp.** Code bị bó buộc vào một kiểu lưu trữ duy nhất.      |
| **Khi nào dùng?**  | Dùng làm kiểu dữ liệu cho biến, tham số hàm, hoặc kiểu trả về của hàm.      | Chỉ dùng ở vế bên phải dấu `=` khi bạn cần tạo đối tượng mới. |
