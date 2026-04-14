# Định nghĩa
![[Pasted image 20260227204435.png]]
![[Pasted image 20260227204502.png]]
![[Pasted image 20260228103949.png]]
# khởi tạo object
public class Person {
	public String name;
	public int age;
	public float height;
	
	public Person(String name, int age, float height) {
		this.name = name;
		this.age = age;
		this.height = height;
	}
	
	public void eat(String foodName) {
		System.out.println(name + " is eating "+ foodName);
	}
	
	public int getAge() {
		return age;
	}
}
