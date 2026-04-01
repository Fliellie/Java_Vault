### 1. Interface là gì? (Cái "Bản hợp đồng")

Interface không phải là một đối tượng cụ thể (như con người, cái xe). Nó chỉ là một danh sách các **hành động**.

- **Ví dụ:** Bạn có một Interface tên là `ISpeak` (Có khả năng nói). Trong đó ghi: "Ai ký hợp đồng này **phải** biết `speak()`".
    
- Nó không dạy bạn nói như thế nào, nó chỉ bắt bạn phải biết nói.
    
![[Pasted image 20260303202737.png]]
![[Pasted image 20260303202750.png]]
![[Pasted image 20260303203203.png]]
![[Pasted image 20260303203234.png]]
# Hoàn thiện
```
public class Student extends Person implements IStudy{
	
	public String universityName;

	public Student(String name, int age, float height, String universityName) {
		super(name, age, height);
		this.universityName = universityName;
	}
	

	public void getInfo() {
		super.getInfo();
		System.out.println("University Name:"+this.universityName);
	}


	@Override
	public Object clone() {
		Student other = new Student(this.name, this.getAge(), this.height, this.universityName);
		return other;
	}


	@Override
	public void study() {
		// TODO Auto-generated method stub
		System.out.println(this.name+" is studing");
	}


	@Override
	public void speak() {
		// TODO Auto-generated method stub
		System.out.println(this.name+" is speaking");
	}
}

```
### 2. Tại sao phải sử dụng?

Có 2 lý do cực kỳ quan trọng mà bài học đã nhắc tới:

- **Thay thế đa kế thừa:** Trong Java, một lớp con chỉ có thể có **một** cha (extends). Nhưng một lớp có thể ký **nhiều** bản hợp đồng (implements nhiều interface).
    
    - _Ví dụ:_ `Student` vừa là con của `Person` (kế thừa đặc điểm con người), vừa ký hợp đồng `IStudy` (phải biết học), vừa ký `ISpeak` (phải biết nói).
        
- **Sự thống nhất (Tính chuẩn hóa):** Giúp hệ thống đồng bộ. Ví dụ: Mọi sinh viên trong hệ thống dù học trường nào thì khi gọi hàm `study()` đều phải hoạt động.
    

---

### 3. Những đặc điểm "Lạ" của Interface

Để dễ nhớ, hãy coi Interface là một nơi "cực đoan":

- **Không có ruột:** Bạn không được viết code xử lý bên trong các hàm (phương thức luôn là `abstract`).
    
- **Không có tính riêng tư:** Mọi thứ luôn là `public` (để ai cũng thấy mà làm theo).
    
- **Không thể tự đứng một mình:** Bạn không thể dùng lệnh `new IStudy()` vì nó chỉ là cái khung, chưa có thịt.
    

---

### 4. Giải thích ví dụ trong bài của bạn

Hãy nhìn vào chuỗi kế thừa này để thấy sự thú vị:

1. **Person (Lớp cha trừu tượng):** Ký hợp đồng `ISpeak` nhưng vì nó là lớp ảo (`abstract`) nên nó... "lầy", nó không thèm làm (không cần override hàm `speak()`).
    
2. **Student (Lớp con):** Đây là lớp cụ thể. Nó phải "gánh" hết nợ nần của cha:
    
    - Phải thực hiện `speak()` (thừa kế từ cha `Person`).
        
    - Phải thực hiện `study()` (từ bản hợp đồng `IStudy` mà nó tự ký).