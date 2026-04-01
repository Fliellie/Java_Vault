### 1. Giao Diện (Interface)

**Interface** trong Java giống như một bản hợp đồng. Nó định nghĩa _những gì_ một lớp phải làm, nhưng không quan tâm đến _cách_ lớp đó thực hiện như thế nào (cho đến trước Java 8).
- **Biến:** Mọi biến trong interface mặc định đều là hằng số (`public static final`).
    
- **Không thể khởi tạo:** Giống như lớp trừu tượng, bạn không thể dùng từ khóa `new` để tạo đối tượng trực tiếp từ interface.
![[Pasted image 20260401160049.png]]
![[Pasted image 20260401160106.png]]
### 2. Giao Diện Hàm (Functional Interface)

**Functional Interface** là một interface đặc biệt, **chỉ chứa duy nhất MỘT phương thức trừu tượng (abstract method)**.
#### Các đặc điểm chính:

- **Quy tắc "Duy nhất một":** Chỉ được phép có đúng 1 phương thức trừu tượng.
    
- **Annotation `@FunctionalInterface`:** Thường được đặt trên đầu interface để báo cho trình biên dịch biết đây là một giao diện hàm. Nếu bạn vô tình thêm phương thức trừu tượng thứ 2, trình biên dịch sẽ báo lỗi ngay lập tức.
    
- **Phương thức Default/Static:** Một Functional Interface vẫn có thể chứa bao nhiêu phương thức `default` hoặc `static` tùy thích, miễn là số lượng phương thức trừu tượng vẫn đúng bằng 1.
    
- **Các interface hàm có sẵn:** Java cung cấp sẵn rất nhiều Functional Interface trong gói `java.util.function` (như `Predicate`, `Consumer`, `Function`, `Supplier`) hoặc các giao diện cũ như `Runnable`, `Callable`, `Comparator`.
![[Pasted image 20260401160321.png]]
![[Pasted image 20260401160336.png]]
