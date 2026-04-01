**Công cụ hỗ trợ:** Hầu hết các IDE hiện nay (IntelliJ, Eclipse, VS Code) đều có chức năng tự động tạo (Generate) Setter/Getter. Đừng ngồi gõ tay từng cái nếu bạn có hàng chục thuộc tính nhé!
## Sử dụng phím tắt (Nhanh nhất)

1. **Khai báo các thuộc tính** của lớp trước (ví dụ: `private String name;`, `private int age;`).
    
2. Nhấn tổ hợp phím:
    
    - **Windows:** `Alt` + `Insert` (hoặc `Source Action...` trong menu chuột phải).
        
    - **Mac:** `Command` + `.` (dấu chấm).
        
3. Chọn dòng **Generate Getters and Setters...**.
    
4. Chọn các thuộc tính bạn muốn tạo (hoặc chọn tất cả) và nhấn **OK**.
## Getter, setter

Hãy nhìn vào biến `private int age;`. Vì nó là `private`, người ngoài không thể sửa lung tung (như kiểu `p.age = -500;`).

- **Hàm `setAge`:** Đóng vai trò là "người gác cổng". Nó chỉ cho phép đổi tuổi nếu giá trị nằm trong khoảng từ 0 đến 100. Đây chính là tác dụng lớn nhất của Setter mà chúng ta đã nói ở trên: **Đảm bảo dữ liệu luôn đúng thực tế.**
    
- **Hàm `getAge`:** Cho phép người khác xem tuổi nhưng không được chạm trực tiếp vào biến gốc.
    
![[Pasted image 20260301164459.png]]

## 2. Constructor


Hàm `public Person(...)` giúp bạn tạo ra một đối tượng "đầy đủ phụ kiện" ngay lập tức chỉ với một dòng lệnh, thay vì phải set từng cái tên, cái tuổi thủ công.
![[Pasted image 20260301164518.png]]

## 4. Hàm `getInfo()`

Đơn giản là một công cụ tiện ích để bạn kiểm tra nhanh thông tin của đối tượng ra màn hình console mà không cần viết lệnh `System.out.println` nhiều lần ở hàm Main.
```
public void getInfo() { System.out.println("Name:"+this.name); System.out.println("Age:"+this.age); System.out.println("Height:"+this.height); }
```
## Cẩn thận với mảng setter, getter
![[Pasted image 20260301163737.png]]
![[Pasted image 20260301163747.png]]
### Giải pháp
![[Pasted image 20260301164112.png]]
![[Pasted image 20260301164122.png]]

## Clone chính bản thân
![[Pasted image 20260301164232.png]]
![[Pasted image 20260301164318.png]]
	- **Tác dụng:** Nó tạo ra một **bản sao hoàn toàn mới** của đối tượng hiện tại.
    
- **Tại sao cần nó?** Trong lập trình, nếu bạn gán `Person p2 = p1;`, thì thực chất cả hai đang trỏ vào cùng một người. Nếu `p2` đổi tên, `p1` cũng đổi theo.
    
- **Cơ chế của `clone()`:** Nó tạo ra một vùng nhớ mới (`new Person`), copy toàn bộ thông tin sang. Lúc này, `p1` và `p2` là hai cá thể độc lập. Nếu bạn sửa `p2`, `p1` vẫn giữ nguyên giá trị cũ.