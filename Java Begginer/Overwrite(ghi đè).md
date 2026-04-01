## 2. Overriding (Ghi đè phương thức)

**Overriding là gì?** Overriding xảy ra khi một **lớp con (subclass)** định nghĩa lại một phương thức đã có sẵn ở **lớp cha (superclass)** của nó. Phương thức ở lớp con phải có **cùng tên, cùng danh sách tham số và cùng kiểu trả về** như phương thức ở lớp cha.

- **Mục đích:** Cung cấp một cách triển khai cụ thể, mang đặc trưng riêng của lớp con cho một hành động mà lớp cha đã định nghĩa chung chung.
    
- **Bản chất:** Đây là thể hiện của tính đa hình lúc chạy (Run-time Polymorphism).
    

**Cách sử dụng (Ví dụ trong Java):** Luôn sử dụng từ khóa `extends` (kế thừa) hoặc `implements` (triển khai giao diện). Bạn nên đặt chú thích (annotation) `@Override` ngay trên phương thức để trình biên dịch kiểm tra xem bạn đã ghi đè đúng cú pháp hay chưa.

Java

```
// Lớp cha
class Animal {
    public void makeSound() {
        System.out.println("Động vật đang kêu...");
    }
}

// Lớp con
class Dog extends Animal {
    // Ghi đè phương thức makeSound của lớp cha
    @Override
    public void makeSound() {
        System.out.println("Chó sủa: Gâu gâu!");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        myDog.makeSound(); // Kết quả in ra sẽ là: "Chó sủa: Gâu gâu!"
    }
}
```
### Cách chống ghi đè
![[Pasted image 20260303082452.png]]
