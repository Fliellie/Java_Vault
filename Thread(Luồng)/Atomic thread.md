### Nguyên lý hoạt động:
ở synchronized khi một luồng hoạt động xong thì luồng khác mới được vào xử lí tiếp
nhưng ở atomic, luồng 1 xử lí được phân nửa nhiệm vụ thì luồng 2 nhảy vào luôn, không xảy ra lỗi biến dùng chung chưa được cập nhật vì atomic luôn tự động cập nhật trước khi hoạt động
### Code Mẫu
```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {
    AtomicInteger count = new AtomicInteger(0);

    void increment() {
        count.incrementAndGet(); // ✔ atomic
    }
}
```


test
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

        System.out.println(c.count.get());
    }
}
```

atomic không quan tâm là object nào, miễn là biến của nó thì mọi luồng được xếp ngang hàng
![[Pasted image 20260403114527.png]]
