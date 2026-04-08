```java
public class LazySingleton {
    private static LazySingleton instance;

    // Private constructor để ngăn chặn việc tạo đối tượng từ bên ngoài
    private LazySingleton() {}

    public static LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton(); // Chỉ tạo khi cần
        }
        return instance;
    }
}
```

Đơn luồng


```java
public class ThreadSafeLazySingleton {
    // volatile giúp đảm bảo các luồng luôn đọc giá trị mới nhất của instance
    private static volatile ThreadSafeLazySingleton instance;

    private ThreadSafeLazySingleton() {}

    public static ThreadSafeLazySingleton getInstance() {
        if (instance == null) { // Kiểm tra lần 1
            synchronized (ThreadSafeLazySingleton.class) {
                if (instance == null) { // Kiểm tra lần 2
                    instance = new ThreadSafeLazySingleton();
                }
            }
        }
        return instance;
    }
}
```


Đa luồng



Nếu bạn muốn khởi tạo lười mà không cần dùng `synchronized` phức tạp, hãy dùng **Static Inner Class**. Java đảm bảo rằng lớp bên trong chỉ được load khi phương thức `getInstance()` được gọi.
```java
public class BillPughSingleton {
    private BillPughSingleton() {}

    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```


