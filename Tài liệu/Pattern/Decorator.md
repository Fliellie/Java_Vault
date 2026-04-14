**Decorator Pattern** là một mẫu thiết kế thuộc nhóm **Structural Pattern**, cho phép bạn "gắn" thêm các trách nhiệm hoặc tính năng mới vào một đối tượng một cách linh hoạt mà **không làm thay đổi cấu trúc** của lớp đó.

Thay vì dùng kế thừa (Inheritance) để mở rộng chức năng (dễ dẫn đến bùng nổ số lượng lớp con), Decorator sử dụng **Composition** (chứa một đối tượng khác bên trong).

---

## 1. Ý tưởng cốt lõi: "Cái bánh và các lớp topping"

Hãy tưởng tượng bạn mua một ly **Cà phê đen** ($2).

- Bạn muốn thêm **Sữa** (+$0.5).
    
- Bạn muốn thêm **Trân châu** (+$0.7).
    

Nếu dùng kế thừa, bạn sẽ phải tạo các lớp: `CafeSua`, `CafeTranChau`, `CafeSuaTranChau`... Rất rắc rối.

Với Decorator, bạn chỉ cần một đối tượng `Cafe` và "bọc" nó bởi các lớp `SuaDecorator`, `TranChauDecorator`.

---

## 2. Ví dụ Code trong Java: Hệ thống đặt trà sữa

### Bước 1: Component (Giao diện chung)

Java

```java
interface MilkTea {
    double getCost();
    String getDescription();
}
```

### Bước 2: Concrete Component (Đối tượng gốc)

Java

```java
class BasicMilkTea implements MilkTea {
    @Override
    public double getCost() {
        return 5.0; // Giá gốc 5$
    }

    @Override
    public String getDescription() {
        return "Trà sữa truyền thống";
    }
}
```

### Bước 3: Decorator (Lớp trừu tượng bọc đối tượng gốc)

Java

```java
abstract class MilkTeaDecorator implements MilkTea {
    protected MilkTea milkTea; // Giữ tham chiếu đến đối tượng được bọc

    public MilkTeaDecorator(MilkTea milkTea) {
        this.milkTea = milkTea;
    }

    @Override
    public double getCost() {
        return milkTea.getCost();
    }

    @Override
    public String getDescription() {
        return milkTea.getDescription();
    }
}
```

### Bước 4: Concrete Decorators (Các loại Topping cụ thể)

Java

```java
class Bubble extends MilkTeaDecorator {
    public Bubble(MilkTea milkTea) {
        super(milkTea);
    }

    @Override
    public double getCost() {
        return super.getCost() + 1.0; // Thêm 1$ cho trân châu
    }

    @Override
    public String getDescription() {
        return super.getDescription() + " + Trân châu";
    }
}

class Pudding extends MilkTeaDecorator {
    public Pudding(MilkTea milkTea) {
        super(milkTea);
    }

    @Override
    public double getCost() {
        return super.getCost() + 1.5; // Thêm 1.5$ cho pudding
    }

    @Override
    public String getDescription() {
        return super.getDescription() + " + Pudding";
    }
}
```

### Bước 5: Sử dụng (Client)

Java

```java
public class Main {
    public static void main(String[] args) {
        // 1. Một ly trà sữa cơ bản
        MilkTea myTea = new BasicMilkTea();
        
        // 2. Thêm trân châu (Bọc ly cơ bản)
        myTea = new Bubble(myTea);
        
        // 3. Thêm pudding (Bọc ly đã có trân châu)
        myTea = new Pudding(myTea);

        System.out.println("Sản phẩm: " + myTea.getDescription());
        System.out.println("Tổng tiền: $" + myTea.getCost());
    }
}
```

---

## 3. Khi nào nên dùng?

- Khi bạn muốn thêm tính năng vào một đối tượng cụ thể tại **thời điểm chạy (runtime)** mà không ảnh hưởng đến các đối tượng khác cùng lớp.
    
- Khi việc mở rộng bằng kế thừa trở nên quá phức tạp (quá nhiều lớp con).
    
- **Ví dụ kinh điển trong Java:** Các lớp Input/Output như `BufferedReader(new FileReader("file.txt"))`. Ở đây `BufferedReader` chính là Decorator cho `FileReader`.
    

---

## 4. So sánh Decorator và Adapter

Hai mẫu này trông khá giống nhau vì đều "bọc" (wrap) một đối tượng khác, nhưng mục đích hoàn toàn khác:

|**Đặc điểm**|**Adapter**|**Decorator**|
|---|---|---|
|**Mục đích**|**Thay đổi** interface để khớp với khách hàng.|**Bổ sung** tính năng nhưng giữ nguyên interface.|
|**Giao diện**|Giao diện mới khác giao diện cũ.|Giao diện của Decorator phải trùng với đối tượng nó bọc.|
|**Sử dụng**|"Cục chuyển đổi" để kết nối.|"Lớp vỏ" để làm giàu tính năng.|