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
