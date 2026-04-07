### 📝 Tóm tắt đề bài "Bảo tàng đồ đạc theo phong cách" (Abstract Factory)

Bạn cần tạo ra một hệ thống sản xuất đồ họa cho ứng dụng sao cho:

1. **Quy tắc 1:** Bạn có 2 loại đồ vật là **Nút bấm (Button)** và **Ô tích (Checkbox)**.
    
2. **Quy tắc 2:** Bạn có 2 phong cách thiết kế là **Windows** và **Mac**.
    
3. **Quy tắc 3:** Bạn phải xây dựng các xưởng (`UIFactory`) sao cho khi khách chọn xưởng Windows, nó sẽ đẻ ra **cả bộ** `WindowsButton` + `WindowsCheckbox`. Tuyệt đối không được râu ông nọ cắm cằm bà kia (kiểu nút của Windows mà ô tích của Mac).
    
4. **Hàm `main`:** Bạn nhập vào chữ `"win"` hoặc `"mac"`, hệ thống tự động bốc đúng xưởng đó về và tạo ra cả bộ giao diện.
    

---

### 🛠️ "Mớ đồ nghề" bạn cần dùng để giải bài này:

Bài này sẽ đẻ ra khá nhiều file/class nhỏ (tầm 9-10 class/interface). Bạn đừng cuống, hãy chia nó làm 3 nhóm đồ nghề cực kỳ logic sau:

#### 1. Đồ nghề tạo "Sản phẩm" (Dùng Interface)

- Tạo 2 cái chuẩn chung:
    
    - `interface Button` $\rightarrow$ Có hàm rỗng `void render();`
        
    - `interface Checkbox` $\rightarrow$ Cũng có hàm rỗng `void render();`
        
- Tạo 4 sản phẩm cụ thể ráp vào chuẩn trên:
    
    - `WindowsButton` và `MacButton` đều `implements Button`. Hãy `@Override` lại hàm `render()` để in ra màn hình kiểu: `"Hiển thị nút bấm kiểu Windows"`.
        
    - `WindowsCheckbox` và `MacCheckbox` đều `implements Checkbox`. Hãy `@Override` lại hàm `render()` để in ra màn hình tương tự.
        

#### 2. Đồ nghề tạo "Xưởng sản xuất nguyên bộ"

Đây là điểm mấu chốt của Abstract Factory:

- **`interface UIFactory`**: Đây là cái "Hợp đồng" chung của xưởng. Nó phải cam kết sản xuất được 2 thứ:
    
    Java
    
    ```
    Button createButton();
    Checkbox createCheckbox();
    ```
    
- **Các xưởng con cụ thể:**
    
    - `WindowsFactory implements UIFactory`: Ở đây, hàm `createButton()` bạn bắt buộc phải trả về `new WindowsButton()`. Hàm `createCheckbox()` trả về `new WindowsCheckbox()`.
        
    - `MacFactory implements UIFactory`: Làm hoàn toàn tương tự nhưng trả về các sản phẩm thuộc họ nhà Mac.
        

#### 3. Đồ nghề chạy thử trong hàm `main`

Ở hàm `main`, bạn sẽ làm bộ khung logic nhận diện như sau:

Java

```
public static void main(String[] args) {
    String config = "win"; // Thử đổi thành "mac" để test
    UIFactory factory;

    // Lựa chọn xưởng theo cấu hình
    if (config.equalsIgnoreCase("win")) {
        factory = new WindowsFactory();
    } else {
        factory = new MacFactory();
    }

    // Bấm nút sản xuất cả bộ
    Button btn = factory.createButton();
    Checkbox cb = factory.createCheckbox();

    // Thử vẽ lên màn hình
    btn.render();
    cb.render();
}
```

---