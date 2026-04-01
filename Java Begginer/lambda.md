**Ví dụ so sánh:** Giả sử ta có interface `Greeting` với một hàm `sayHello()`.

- **Cách cũ (Anonymous Class):**
    
    Java
    
    ```
    Greeting g = new Greeting() {
        @Override
        public void sayHello() {
            System.out.println("Chào bạn!");
        }
    };
    ```

ở đây ta đang tạo 1 đối tượng g mới, đối tượng g này được khởi tạo bởi constructor greeting. Nhưng lớp này ta chưa định nghĩa nên ta có thể định nghĩa ngay bên trong luôn với mục đích tạo ra 1 hàm sayHello cho đối tượng g và có thể dùng luôn

- **Cách mới (Functional Interface + Lambda):**
    
    Java
    
    ```
    Greeting g = () -> System.out.println("Chào bạn!");
    ```
    

> Nhờ Functional Interface, Java hiểu rằng: "À, vì interface này chỉ có 1 hàm duy nhất, nên đoạn code Lambda kia chắc chắn là nội dung của hàm đó!"

## Ý nghĩa

Trong lập trình truyền thống, bạn truyền **Số (int)**, **Chữ (String)** vào hàm. Nhưng với Functional Interface, bạn truyền được cả một **Thuật toán** vào hàm.

**Ví dụ thực tế:** Bạn viết một hàm `tinhToan(a, b, phepTinh)`.

- Lần này bạn truyền vào phép cộng: `(a, b) -> a + b`
    
- Lần sau bạn truyền vào phép nhân: `(a, b) -> a * b`
    
- Hàm `tinhToan` không thay đổi, nhưng kết quả thay đổi tùy theo "hành động" bạn gửi vào.



Hãy tưởng tượng bạn là một đầu bếp, và bạn có một chiếc "Máy vạn năng". Thay vì mua 10 cái máy cho 10 việc, bạn chỉ mua 1 cái máy có một cái khe cắm "Chip hành động". Bạn cắm Chip Xay thì nó xay, cắm Chip Ép thì nó ép.

---

## 1. Phân tích ví dụ `tinhToan`

Để làm được điều này, chúng ta cần một **Functional Interface** làm "cái khe cắm". Ở đây, Java có sẵn `BiFunction<T, U, R>` (nhận 2 số, trả về 1 số), nhưng để dễ hiểu, mình tự tạo một cái tên là `PhepToan`.

#### Bước 1: Tạo "khe cắm" (Interface)

Java

```
@FunctionalInterface
interface PhepToan {
    int thucHien(int a, int b); // Chỉ có 1 hành động duy nhất
}
```

#### Bước 2: Viết hàm "Máy vạn năng"

Hàm này không hề biết nó sẽ cộng hay trừ. Nó chỉ biết: "Tôi nhận 2 số, và tôi sẽ dùng cái `phepToan` được đưa vào để xử lý chúng".

Java

```
public static int tinhToan(int a, int b, PhepToan phepToan) {
    return phepToan.thucHien(a, b);
}
```

#### Bước 3: Truyền "Hành động" (Thuật toán) vào

Bây giờ, tại hàm `main`, bạn có thể "bơm" thuật toán vào mà không cần viết thêm hàm mới nào cả:

Java

```
// Truyền hành động CỘNG
int tong = tinhToan(5, 3, (a, b) -> a + b); 

// Truyền hành động NHÂN
int tich = tinhToan(5, 3, (a, b) -> a * b);

// Truyền hành động "Lấy số lớn nhất"
int max = tinhToan(5, 3, (a, b) -> (a > b) ? a : b);
```

---

## 2. Tại sao cái này lại "xịn" hơn cách truyền thống?

Hãy so sánh với cách làm "cũ kỹ" khi chưa có Functional Interface:

|**Đặc điểm**|**Cách truyền thống (Hard-code)**|**Cách dùng Functional Interface**|
|---|---|---|
|**Cấu trúc**|Bạn phải viết `tinhTong()`, `tinhNhan()`, `timMax()`...|Chỉ cần **duy nhất 1 hàm** `tinhToan()`.|
|**Mở rộng**|Muốn thêm phép chia? Phải sửa code, thêm hàm mới.|Muốn thêm phép chia? Chỉ cần truyền `(a, b) -> a / b` vào lúc gọi.|
|**Tính đóng gói**|Logic bị lộ ra ngoài hoặc phân tán nhiều nơi.|Logic xử lý (hành động) được đóng gói gọn trong một dòng Lambda.|

---

## 3. Ứng dụng thực tế 

```
@FunctionalInterface // Interface chỉ có 1 hàm duy nhất 
interface Operation<T> { T execute(T a, T b); }
```

![[Pasted image 20260319222450.png]]
