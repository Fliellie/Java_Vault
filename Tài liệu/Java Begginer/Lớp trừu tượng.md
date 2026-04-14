trừu tượng là sự ẩn đi những đối tượng bên lề để tập trung vào chức năng chính của nó

`` ví dụ trong vật lú , ta ẩn đi các bộ phận của xe oto và coi nó là 1 chất điểm duy nhất

### Code
![[Pasted image 20260303201358.png]]




`` Trong java, trừu tượng có nghĩa là ẩn hàm gốc đi , các hàm trong hàm gốc chỉ có thể được sử dụng bởi các lớp con kế thừa
`` ví dụ: tạo 1 lớp con người. Ta không muốn người dùng tạo đối tượng với lớp con người mà chỉ muốn tạo đối tượng với lớp student chẳng hạn...
### Phương thức trừu tượng
là các hàm ở hàm cha mà chưa được định nghĩa
định nghĩa của nó chắc chắn được overite lại ở lớp con
### 1. Cú pháp khai báo

Phương thức này sử dụng từ khóa `abstract` và kết thúc bằng dấu chấm phẩy `;` thay vì cặp ngoặc nhọn `{ }`.

Java

```
abstract void tinhToan(); // Không có nội dung bên trong
```

### 2. Các quy tắc "vàng"

- **Chỉ nằm trong lớp trừu tượng:** Bạn không thể khai báo một phương thức trừu tượng trong một lớp thông thường (`class`). Lớp chứa nó cũng phải được đánh dấu là `abstract class`.
    
- **Bắt buộc phải ghi đè (Override):** Khi một lớp con kế thừa từ lớp trừu tượng, lớp con đó **bắt buộc** phải viết nội dung cụ thể cho phương thức trừu tượng đó (trừ khi lớp con đó cũng là lớp trừu tượng).
    
- **Không được dùng `private`, `static`, hoặc `final`:** Vì mục đích của phương thức trừu tượng là để lớp con kế thừa và sửa đổi, nên bạn không thể khóa nó lại bằng các từ khóa trên.
### 2. Ý nghĩa của `abstract Object clone()` trong bài này

Tại sao bạn lại viết như vậy? Ý nghĩa thực sự là:

- Bạn định nghĩa rằng mọi "Con người" (`Person`) đều có khả năng "nhân bản" (`clone`).
    
- Nhưng ở cấp độ lớp `Person` chung chung, bạn chưa biết quy trình nhân bản cụ thể sẽ như thế nào (ví dụ: nhân bản một `Student` có thể khác với nhân bản một `Teacher`).
    
- Bạn để nó là `abstract` để **ép buộc** các lớp con sau này (như `class Student extends Person`) phải tự viết logic cho hàm `clone()` này.
- ![[Pasted image 20260303202314.png]]
