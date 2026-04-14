## VD1:
tính huống:
```java
class Task {
    volatile boolean running = true;

    void run() {
        while (running) {
            // xử lý
        }
        System.out.println("Đã dừng");
    }

    void stop() {
        running = false;
    }
}
```
```java
public class Main {
    public static void main(String[] args) throws Exception {
        Counter c = new Counter();

        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                c.increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(c.count);
    }
}
```

luồng 1 vào đọc 5
luồng 2 vào đọc 5
luồng 1 +1=6
luồng 2+1=6
-> kết quả mong muốn: 7
## Giải pháp:

```java
class Counter {
    int count = 0;

    synchronized void increment() {
        count++;
    }
}
```
chỉ cho phép 1 luồng dùng hàm này trong 1 thời gian



## Lock Theo từng Object
```java
void increment() {
    synchronized(this) {
        count++;
    }
}
```


giả sử có 2 đối tượng : c1, c2
biến count của cả 2 là khác nhau
và ta chỉ muốn khóa luồng hàm increment trong cùng Object
-> nếu dùng synchronized như giải pháp ở trên thì dù 2 Object khác nhau vẫn sẽ bị chặn


### 1. Ở cấp độ Phương thức (Method Level)

Bạn đặt `synchronized` ngay trong khai báo phương thức.

- **Instance Method (Phương thức thông thường):** Khóa dựa trên đối tượng hiện tại (`this`). Chỉ một luồng có thể chạy bất kỳ phương thức `synchronized` nào của đối tượng đó tại một thời điểm.
    
    Java
    
    ```java
    public synchronized void updateData() {
        // Code xử lý
    }
    ```
    
- **Static Method (Phương thức tĩnh):** Khóa dựa trên Class (như đã giải thích ở câu trước). Nó ngăn chặn tất cả các luồng truy cập vào phương thức này trên toàn bộ ứng dụng.
    
    Java
    
    ```java
    public static synchronized void globalUpdate() {
        // Code xử lý
    }
    ```
    

---

### 2. Ở cấp độ Khối mã (Block Level)

Đây là cách linh hoạt nhất (và thường được khuyến khích hơn) vì bạn có thể chỉ khóa những dòng code thực sự cần thiết, thay vì khóa cả phương thức dài.

- **Sử dụng `this`:** Khóa đối tượng hiện tại.
    
    Java
    
    ```java
    public void method() {
        synchronized(this) {
            // Chỉ phần này được bảo vệ
        }
    }
    ```
    
- **Sử dụng Class object:** Dùng trong phương thức static hoặc khi muốn khóa cấp độ class.
    
    Java
    
    ```java
    synchronized(MyClass.class) {
        // Bảo vệ tài nguyên dùng chung cho toàn bộ Class
    }
    ```
    
- **Sử dụng một đối tượng khóa riêng (Lock Object):** Đây là một kỹ thuật rất hay để tăng hiệu suất. Bạn tạo một đối tượng nhỏ chỉ để làm "chìa khóa".
    
    Java
    
    ```java
    private final Object myLock = new Object();
    
    public void doSomething() {
        synchronized(myLock) {
            // An toàn hơn và không bị ảnh hưởng bởi các luồng khác đang khóa 'this'
        }
    }
    ```
    

---

### So sánh các vị trí đặt `synchronized`

|**Vị trí**|**Phạm vi khóa**|**Hiệu suất**|**Độ linh hoạt**|
|---|---|---|---|
|**Method**|Toàn bộ phương thức|Thấp (khóa lâu)|Thấp|
|**Block (`this`)**|Một phần phương thức|Trung bình|Cao|
|**Block (`Lock Object`)**|Một phần phương thức|**Cao nhất**|**Cao nhất**|
|**Static Method**|Toàn bộ Class|Thấp|Thấp|