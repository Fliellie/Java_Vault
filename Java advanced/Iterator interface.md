## 1. Bản chất của Iterator là gì?

`Iterator` (Bộ lặp) là một interface thuộc gói `java.util`. Bạn có thể hình dung nó giống như một **"con trỏ" (cursor)** chạy dọc theo các phần tử của một Collection.

Nó sinh ra để cung cấp một cách **thống nhất** để duyệt qua bất kỳ cấu trúc dữ liệu nào (dù đó là `ArrayList` có index, hay `HashSet` không hề có index) theo một chiều duy nhất: từ đầu đến cuối.

---

## 2. Ba phương thức "cốt lõi" của Iterator

Để điều khiển con trỏ `Iterator`, bạn chỉ cần nắm vững 3 phương thức quyền lực này:

|Phương thức|Ý nghĩa|Cách hoạt động|
|---|---|---|
|`hasNext()`|**"Phía trước còn ai không?"**|Trả về `true` nếu vẫn còn phần tử để duyệt, ngược lại trả về `false`. Thường dùng làm điều kiện cho vòng lặp `while`.|
|`next()`|**"Nhặt lấy người tiếp theo!"**|Di chuyển con trỏ lên 1 bước và trả về phần tử vừa bị bước qua. Nếu không còn phần tử mà vẫn gọi, nó sẽ ném lỗi `NoSuchElementException`.|
|`remove()`|**"Xóa kẻ vừa nhặt được!"**|Xóa phần tử cuối cùng vừa được `next()` trả về khỏi Collection.|

---

## 3. Tại sao phải dùng Iterator trong khi đã có vòng lặp `for-each`?

Đây là câu hỏi phỏng vấn kinh điển! Vòng lặp `for-each` nhìn có vẻ ngắn gọn và dễ viết hơn nhiều, vậy tại sao `Iterator` vẫn tồn tại và cực kỳ quan trọng?

**Lý do "chí mạng": Xóa phần tử khi đang duyệt!**

- Nếu bạn dùng vòng lặp `for-each` để duyệt một danh sách, và ở giữa vòng lặp bạn gọi `list.remove()`, chương trình của bạn sẽ **CRASH (sập) ngay lập tức** với lỗi huyền thoại: `ConcurrentModificationException`. Java không cho phép bạn vừa duyệt vừa thay đổi cấu trúc danh sách theo cách thông thường.
    
- **Cách giải quyết duy nhất:** Sử dụng phương thức `remove()` của chính **`Iterator`**. Nó được thiết kế riêng để đảm bảo việc xóa phần tử khi đang lặp diễn ra an toàn tuyệt đối.
    

---

## 4. Code thực chiến: Cách dùng Iterator chuẩn xác

Hãy xem ví dụ dưới đây để thấy cách `Iterator` cứu bạn khỏi lỗi crash app như thế nào khi cần lọc bỏ các dữ liệu không hợp lệ:

Java

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Mango");

        // 1. Lấy Iterator từ danh sách
        Iterator<String> iterator = fruits.iterator();

        // 2. Dùng vòng lặp while kết hợp hasNext()
        while (iterator.hasNext()) {
            // Lấy phần tử ra
            String fruit = iterator.next();
            
            // XÓA AN TOÀN: Xóa "Banana" khỏi danh sách
            if (fruit.equals("Banana")) {
                iterator.remove(); // KHÔNG dùng fruits.remove(fruit) ở đây!
            }
        }

        System.out.println(fruits); // Kết quả: [Apple, Mango]
    }
}
```

---

## 5. Người anh em nâng cấp: `ListIterator`

Nếu `Iterator` thông thường chỉ đi được một chiều (tiến lên), thì Java cung cấp thêm một interface con tên là **`ListIterator`** dành riêng cho nhánh `List` (`ArrayList`, `LinkedList`).

Nó là phiên bản "độ súng", cho phép bạn:

- Đi lùi bằng `hasPrevious()` và `previous()`.
    
- Chèn thêm phần tử mới ngay tại vị trí con trỏ bằng `add()`.
    
- Sửa phần tử hiện tại bằng `set()`.
    

---

Sau khi đã nắm được cách dùng `Iterator` để lùng sục mọi ngóc ngách của dữ liệu, bạn muốn quay lại hoàn thiện nốt nhánh `Set` (Tập hợp triệt tiêu sự trùng lặp), hay muốn tiến thẳng đến trùm cuối `Map` (Từ điển Khóa - Giá trị)?