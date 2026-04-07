### Cơ bản
```java
Thread t = new Thread() {
    public void run() {
        System.out.println("Hello");
    }
};
```


phải override run  để tạo hàm chạy trong luồng



### 🔑 3. Chỉ đặt tên thread
```java
Thread t=new Thread("th_1");

```


### Truyền runnable
```java
new Thread(runnable);
```


### Chạy luồng
```java
t.run();
```


### Mẫu kết hợp Runnable
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

