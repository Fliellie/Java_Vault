Trong Java, mọi class đều mặc định kế thừa từ lớp cha cao nhất là `Object`. Lớp `Object` có sẵn một phương thức là `clone()`.


- **Nhiệm vụ:** Khi bạn gọi `super.clone()`, Java sẽ thực hiện một việc cực kỳ đặc biệt: nó **copy nguyên văn từng bit** dữ liệu của đối tượng hiện tại sang một vùng nhớ mới.
    
- **Cơ chế:** Nó tạo ra một đối tượng mới mà **không gọi Constructor**. Đây là điểm khác biệt lớn nhất so với việc dùng `new`.
- 
- 
- 
- **Shallow Copy (Bản sao nông):** Mặc định `super.clone()` chỉ copy các kiểu dữ liệu nguyên thủy (int, double...) và **địa chỉ vùng nhớ** của các đối tượng (như `List`, `String`).
    
- **Vấn đề:** Nếu bạn có một `List<String> sections`, cả bản gốc và bản sao sẽ cùng trỏ vào **một danh sách** duy nhất. Nếu bản sao sửa danh sách, bản gốc cũng bị sửa theo!



```java
@Override
public ReportTemplate clone() {
    try {
        // Bước 1: Copy nông (tạo ra đối tượng mới, copy title, footer)
        ReportTemplate cloned = (ReportTemplate) super.clone();
        
        // Bước 2: Copy sâu (tạo ra danh sách mới hoàn toàn cho bản sao)
        // Nếu không có dòng này, copy1.sections và origin.sections là MỘT.
        cloned.sections = new ArrayList<>(this.sections); 
        
        return cloned;
    } catch (CloneNotSupportedException e) {
        return null; 
    }
}
```