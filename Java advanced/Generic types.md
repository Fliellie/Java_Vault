#### Hạn chế 1: Không thể dùng Kiểu Nguyên Thủy (Primitive Types)

Bạn không thể truyền các kiểu nguyên thủy (`int`, `double`, `char`, `boolean`...) vào Generics. Bạn **bắt buộc** phải sử dụng các Lớp Bao Bọc (Wrapper Classes) tương ứng.

Java

```java
// LỖI BIÊN DỊCH
List<int> list1 = new ArrayList<>(); 
List<double> list2 = new ArrayList<>();

// ĐÚNG
List<Integer> list1 = new ArrayList<>();
List<Double> list2 = new ArrayList<>();
```

#### Hạn chế 2: Không thể khởi tạo trực tiếp tham số kiểu `new T()`

Bạn không thể dùng từ khóa `new` để tạo trực tiếp một đối tượng từ tham số kiểu `T`. Vì lúc chương trình chạy (runtime), Java không hề biết `T` là `String`, `Integer` hay lớp nào cả.

Java

```java
class TaoDoiTuong<T> {
    private T obj;

    public TaoDoiTuong() {
        // LỖI BIÊN DỊCH: Không thể khởi tạo trực tiếp T
        // obj = new T(); 
    }
}
```

#### Hạn chế 3: Không thể tạo mảng của tham số kiểu `new T[]`

Tương tự như trên, bạn không thể khởi tạo trực tiếp một mảng chứa kiểu `T`.

Java

```java
class TaoMang<T> {
    private T[] mang;

    public TaoMang() {
        // LỖI BIÊN DỊCH: Không thể tạo mảng generic trực tiếp
        mang = new T[10]; 
    }
}
```


### 3. Cách "Lách Luật" để khởi tạo T hoặc mảng T
**Cách khởi tạo mảng T (Sử dụng ép kiểu từ mảng Object):** Đây là cách phổ biến nhất mà ngay cả mã nguồn nội bộ của Java (như `ArrayList`) cũng đang sử dụng.

Java

```java
class DanhSachCuaToi<T> {
    private T[] mang;

    @SuppressWarnings("unchecked") // Tắt cảnh báo ép kiểu
    public DanhSachCuaToi(int kichThuoc) {
        // Khởi tạo một mảng Object, sau đó ép kiểu sang mảng T[]
        mang = (T[]) new Object[kichThuoc];
    }
}
```

## 4. Cách khởi tạo đối tượng T (Sử dụng Reflection):

Java

```java
class NhaMay<T> {
    private T sanPham;

    // Truyền Class<T> vào qua constructor
    public NhaMay(Class<T> kieuDuLieu) {
        try {
            // Sử dụng Reflection để tạo đối tượng mới (yêu cầu class T phải có constructor rỗng)
            this.sanPham = kieuDuLieu.getDeclaredConstructor().newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    public T getSanPham() {
        return sanPham;
    }
}

public class Main {
    public static void main(String[] args) {
        // Khởi tạo bằng cách truyền String.class vào
        NhaMay<String> nhaMayChuoi = new NhaMay<>(String.class);
    }
}
```