chỉ đứng trước 1 biến
### Ví Dụ:
luồng A đang thực hiện nhiệm vụ
luồng B yêu cầu dừng
biến volatile yêu cầu luồng A luôn cập nhật trạng thái của nó


khi không dùng volatile
```java
```java

public class Main {
    public static void main(String[] args) {

        Runnable task = () -> {
            AppConfig config = AppConfig.getInstance();

        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();
    }
}
```


có dùng volatile
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