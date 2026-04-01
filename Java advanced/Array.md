## 1. Bản chất của Mảng (Array) trong Java

Trước khi code, bạn phải "nằm lòng" 3 đặc điểm cốt lõi này của mảng trong Java, vì nó rất khắt khe:

1. **Cùng kiểu dữ liệu:** Một mảng số nguyên (`int`) chỉ được chứa số nguyên, không thể nhét một chuỗi (`String`) vào đó được.
    
2. **Kích thước CỐ ĐỊNH (Fixed Size):** Đây là điểm hạn chế lớn nhất. Khi bạn khai báo một mảng có 10 phần tử, nó vĩnh viễn có 10 phần tử. Bạn không thể tự động phình to hay thu nhỏ nó lại sau khi đã khởi tạo.
    
3. **Chỉ số (Index) bắt đầu từ 0:** Vị trí đầu tiên của mảng luôn là `0`, và vị trí cuối cùng là `độ_dài_mảng - 1`.
    

---

## 2. Mảng một chiều (1D Array)

Mảng 1 chiều giống như một đoàn tàu, các toa tàu nối đuôi nhau liên tiếp trong bộ nhớ.

### Khai báo và Khởi tạo

Có 2 cách chính để tạo mảng 1 chiều:

**Cách 1: Biết trước số lượng, chưa biết giá trị**

Java

```java
// Khai báo một mảng số nguyên có 5 phần tử
int[] numbers = new int[5]; 

// Gán giá trị cho từng phần tử (Index từ 0 đến 4)
numbers[0] = 10;
numbers[1] = 20;
// ... các phần tử chưa gán sẽ nhận giá trị mặc định (với int là 0)
```

**Cách 2: Biết trước luôn các giá trị (Khởi tạo trực tiếp)**

Java

```java
String[] names = {"Hoa", "Lan", "Mai", "Trúc"};
```

### Duyệt mảng (Đọc dữ liệu)

Thuộc tính `length` (không có dấu ngoặc `()`) là "người bạn thân" giúp bạn lấy ra kích thước của mảng. Chúng ta thường dùng vòng lặp `for` hoặc `for-each` để duyệt:

Java

```java
int[] scores = {8, 7, 9, 10};

// Cách 1: Dùng vòng lặp for truyền thống (khi cần quan tâm đến vị trí index)
for (int i = 0; i < scores.length; i++) {
    System.out.println("Điểm thứ " + i + " là: " + scores[i]);
}

// Cách 2: Dùng for-each (ngắn gọn hơn, khi chỉ cần lấy giá trị)
for (int score : scores) {
    System.out.println("Điểm: " + score);
}
```

---

## 3. Mảng đa chiều (Multi-dimensional Array)

Mảng đa chiều phổ biến nhất là **Mảng 2 chiều**. Bản chất của mảng 2 chiều trong Java là **"Mảng chứa các mảng khác"** (Array of arrays). Bạn có thể hình dung nó như một ma trận (Matrix) có bảng tính Excel gồm các **Hàng (Row)** và **Cột (Column)**.

### Khai báo và Khởi tạo Mảng 2 chiều

**Cách 1: Khai báo kích thước (Số hàng x Số cột)**

Java

```java
// Khai báo ma trận 3 hàng, 4 cột
int[][] matrix = new int[3][4];

// Gán giá trị cho phần tử ở hàng 0, cột 1
matrix[0][1] = 15; 
```

**Cách 2: Khởi tạo trực tiếp bằng các mảng lồng nhau**

Java

```java
int[][] myNumbers = {
    {1, 2, 3, 4}, // Hàng 0 (index 0)
    {5, 6, 7},    // Hàng 1 (index 1) - Chú ý: Các hàng không bắt buộc phải dài bằng nhau!
    {8, 9, 10, 11, 12} // Hàng 2 (index 2)
};
```

### Duyệt mảng 2 chiều

Để duyệt mảng 2 chiều, bạn cần 2 vòng lặp `for` lồng nhau. Vòng ngoài chạy theo hàng, vòng trong chạy theo các phần tử của hàng đó (cột).

Java

```java
int[][] board = {
    {1, 2, 3},
    {4, 5, 6}
};

for (int i = 0; i < board.length; i++) { // board.length trả về số lượng hàng (2)
    for (int j = 0; j < board[i].length; j++) { // board[i].length trả về số lượng cột của hàng i (3)
        System.out.print(board[i][j] + " ");
    }
    System.out.println(); // Xuống dòng sau khi in xong 1 hàng
}
```

---

## ⚠️ Lỗi "Quốc Dân" cần tránh

Khi làm việc với mảng, 99% lập trình viên mới đều sẽ gặp lỗi: `java.lang.ArrayIndexOutOfBoundsException`. Lỗi này văng ra khi bạn cố tình truy cập vào một vị trí **không tồn tại** trong mảng (thường là do quên index bắt đầu từ 0, hoặc vòng lặp chạy quá số lượng phần tử).

**Ví dụ gây lỗi:**

Java

```java
int[] arr = {1, 2, 3}; // Index hợp lệ là 0, 1, 2
System.out.println(arr[3]); // 💣 BÙM! ArrayIndexOutOfBoundsException
```

---

Mảng tĩnh như thế này xử lý rất nhanh, nhưng vì nhược điểm "kích thước cố định" nên đôi khi hơi bất tiện nếu dữ liệu thêm bớt liên tục. Bạn có muốn thử nghiệm viết một thuật toán kinh điển như "Tìm số lớn nhất trong mảng", hay muốn mình giới thiệu luôn sang `ArrayList` (mảng động - tự động co giãn kích thước) để khắc phục nhược điểm này?