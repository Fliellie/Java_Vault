**Composite Pattern** là một mẫu thiết kế giúp bạn xử lý một **nhóm đối tượng** giống hệt như một **đối tượng đơn lẻ**.

Nói cách khác, nó giúp bạn xây dựng cấu trúc dữ liệu theo dạng **cây (tree)**.

---

## 1. Ý tưởng cốt lõi

Hãy tưởng tượng hệ thống quản lý tệp tin (File System) trên máy tính của bạn:

- **File**: Là đối tượng đơn giản nhất (Lá - Leaf).
    
- **Folder**: Là một đối tượng phức tạp (Composite). Nó chứa các File và thậm chí chứa cả các Folder con khác.
    

Dù bạn thực hiện thao tác `getSize()` (xem dung lượng), bạn có thể gọi nó trên một File hoặc một Folder mà không cần quan tâm nó là cái nào. Folder sẽ tự động đi hỏi tất cả các con của nó và cộng dồn lại.

---

## 2. Các thành phần trong Composite Pattern

1. **Component**: Là interface hoặc abstract class quy định các phương thức chung cho cả đối tượng đơn giản và đối tượng phức tạp (ví dụ: `showDetails()`).
    
2. **Leaf**: Các đối tượng cơ bản, không có con (ví dụ: `File`).
    
3. **Composite**: Đối tượng chứa các Leaf hoặc các Composite khác. Nó triển khai các phương thức của Component bằng cách chuyển tiếp yêu cầu cho các đối tượng con.
    
4. **Client**: Thao tác với mọi đối tượng thông qua giao diện Component.
    

---

## 3. Ví dụ Code trong Java: Sơ đồ tổ chức công ty

Giả sử bạn cần tính tổng lương của một phòng ban, bao gồm lương của trưởng phòng và tất cả nhân viên cấp dưới.

### Bước 1: Component (Interface chung)

Java

```
interface Employee {
    void showDetails();
    double getSalary();
}
```

### Bước 2: Leaf (Nhân viên bình thường)

Java

```
class Developer implements Employee {
    private String name;
    private double salary;

    public Developer(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void showDetails() {
        System.out.println("Dev: " + name + " | Lương: " + salary);
    }

    public double getSalary() {
        return salary;
    }
}
```

### Bước 3: Composite (Quản lý - có thể có nhân viên cấp dưới)

Java

```
import java.util.ArrayList;
import java.util.List;

class Manager implements Employee {
    private String name;
    private double salary;
    private List<Employee> subordinates = new ArrayList<>();

    public Manager(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void addEmployee(Employee e) {
        subordinates.add(e);
    }

    public void showDetails() {
        System.out.println("Manager: " + name + " | Lương: " + salary);
        for (Employee e : subordinates) {
            e.showDetails(); // Đệ quy gọi cấp dưới
        }
    }

    public double getSalary() {
        double totalSalary = this.salary;
        for (Employee e : subordinates) {
            totalSalary += e.getSalary(); // Cộng dồn lương cấp dưới
        }
        return totalSalary;
    }
}
```

### Bước 4: Client (Sử dụng)

Java

```
public class CompanyClient {
    public static void main(String[] args) {
        // Tạo nhân viên
        Developer dev1 = new Developer("An", 1000);
        Developer dev2 = new Developer("Bình", 1200);

        // Tạo trưởng phòng và thêm nhân viên vào nhóm
        Manager devManager = new Manager("Cường (Tech Lead)", 2500);
        devManager.addEmployee(dev1);
        devManager.addEmployee(dev2);

        // Tạo Giám đốc và thêm Trưởng phòng vào
        Manager generalManager = new Manager("Dũng (CEO)", 5000);
        generalManager.addEmployee(devManager);

        // Xem chi tiết và tổng lương toàn công ty
        generalManager.showDetails();
        System.out.println("Tổng lương toàn hệ thống: " + generalManager.getSalary());
    }
}
```

---

## 4. Tại sao phải dùng Composite?

- **Tính đồng nhất**: Client không cần check `if (obj instanceof Manager)` hay `if (obj instanceof Developer)`. Cứ gọi `getSalary()` là xong.
    
- **Dễ mở rộng**: Bạn có thể thêm các loại nhân viên mới (Tester, Designer) hoặc các cấp bậc quản lý mới mà không cần sửa code cũ.
    
- **Cấu trúc phân cấp**: Rất phù hợp cho các menu đa cấp, hệ thống thư mục, sơ đồ tổ chức, hoặc các thành phần UI (Panel chứa Button, Label và các Panel khác).
    

## 5. So sánh nhanh với Adapter

| **Đặc điểm**           | **Adapter**                                      | **Composite**                                              |
| ---------------------- | ------------------------------------------------ | ---------------------------------------------------------- |
| **Mục đích**           | Thay đổi giao diện đối tượng để khớp với client. | Gộp các đối tượng thành cấu trúc cây.                      |
| **Số lượng đối tượng** | Thường là 1-1 (1 Adapter bọc 1 Adaptee).         | Là 1-N (1 Composite chứa nhiều con).                       |
| **Tính chất**          | Làm cho 2 thứ khác nhau làm việc được với nhau.  | Làm cho nhóm đối tượng hoạt động như 1 đối tượng duy nhất. |