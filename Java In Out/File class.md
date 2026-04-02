
Chào bạn! Sau khi đã học cách dùng `OutputStream` để ghi dữ liệu, chắc hẳn bạn đang thắc mắc: _"Làm sao để kiểm tra xem file đó có tồn tại không? Làm sao để tạo thư mục mới hay xóa một file cũ?"_.

Đó chính là lúc lớp **`File`** trong gói `java.io` xuất hiện. Hãy nhớ một điều tối quan trọng: Lớp `File` **không dùng để đọc hoặc ghi nội dung file** (đó là việc của Stream/Reader/Writer), nó dùng để **quản lý các thuộc tính của file và thư mục** trên ổ đĩa.

---

## 1. Bản chất của lớp `File`

Lớp `File` là một biểu diễn trừu tượng của đường dẫn tệp tin (file) và danh mục (directory). Khi bạn tạo một đối tượng `File`, nó không có nghĩa là file đó đã tồn tại trên ổ cứng. Nó chỉ là một "cái tên" hoặc "địa chỉ" mà bạn định làm việc cùng.

---

## 2. Các phương thức "quyền lực" nhất

Dưới đây là bảng tổng hợp các thao tác mà bạn sẽ dùng tới 90% thời gian khi làm việc với file:

### 🔍 Kiểm tra thông tin

|Phương thức|Ý nghĩa|
|---|---|
|`exists()`|Kiểm tra file hoặc thư mục có tồn tại thật hay không.|
|`isFile()`|Kiểm tra xem đây có phải là một file bình thường không.|
|`isDirectory()`|Kiểm tra xem đây có phải là một thư mục (folder) không.|
|`getName()`|Lấy tên file (ví dụ: `data.txt`).|
|`getAbsolutePath()`|Lấy đường dẫn đầy đủ từ ổ đĩa (ví dụ: `C:\Users\Admin\data.txt`).|
|`length()`|Trả về kích thước file tính bằng **byte**.|

### 🛠️ Thao tác tạo và xóa

| Phương thức           | Ý nghĩa                                                        |
| --------------------- | -------------------------------------------------------------- |
| `createNewFile()`     | Tạo một file mới (trả về `true` nếu tạo thành công).           |
| `mkdir()`             | Tạo một thư mục mới.                                           |
| `mkdirs()`            | Tạo thư mục bao gồm cả các thư mục cha nếu chúng chưa tồn tại. |
| `delete()`            | Xóa file hoặc thư mục (thư mục phải rỗng mới xóa được).        |
| `renameTo(File dest)` | Đổi tên hoặc di chuyển file đến vị trí mới.                    |
|                       |                                                                |
|                       |                                                                |
|                       |                                                                |

![[Pasted image 20260401204053.png]]

---

## 3. Code ví dụ: Khám phá một file

Hãy thử xem đoạn code dưới đây để thấy lớp `File` hoạt động như một "thám tử" như thế nào:



```java
import java.io.File;
import java.io.IOException;

public class FileClassExample {
    public static void main(String[] args) {
        // Khởi tạo đối tượng File (chỉ là cái tên, chưa tạo trên đĩa)
        File myFile = new File("demo_java.txt");

        try {
            if (myFile.createNewFile()) {
                System.out.println("File đã được tạo: " + myFile.getName());
            } else {
                System.out.println("File đã tồn tại rồi!");
            }

            // Kiểm tra các thuộc tính
            if (myFile.exists()) {
                System.out.println("Đường dẫn tuyệt đối: " + myFile.getAbsolutePath());
                System.out.println("Có thể ghi không? " + myFile.canWrite());
                System.out.println("Kích thước file: " + myFile.length() + " bytes");
            }

        } catch (IOException e) {
            System.out.println("Có lỗi xảy ra khi thao tác với file.");
            e.printStackTrace();
        }
    }
}
```

---

## 💡 Mẹo nhỏ: Dấu gạch chéo (`/` hay `\`)?

Một rắc rối kinh điển là Windows dùng dấu gạch chéo ngược (`\`), còn Linux/macOS dùng dấu gạch chéo xuôi (`/`). Để code của bạn chạy được trên mọi hệ điều hành (cross-platform) mà không bị lỗi đường dẫn, hãy dùng:

- **`File.separator`**: Java sẽ tự chọn dấu gạch đúng cho bạn.
    
- Hoặc đơn giản là dùng dấu `/` (Java trên Windows vẫn hiểu được dấu này).
    

---

## 🚀 Xu hướng hiện đại: `java.nio.file.Path`

Mặc dù lớp `File` rất phổ biến, nhưng từ Java 7 trở đi, các "cao thủ" thường chuyển sang dùng **`Path`** và **`Files`** trong gói `java.nio`. Chúng mạnh mẽ hơn, hỗ trợ xử lý lỗi tốt hơn và làm việc cực nhanh với các thư mục lớn.

Bạn có muốn mình giới thiệu sự khác biệt giữa cách làm "truyền thống" (`File`) và cách làm "hiện đại" (`Path/Files`) để bạn chọn hướng đi tối ưu nhất cho dự án của mình không?



## Cách đọc
![[Pasted image 20260401204311.png]]
Nhìn vào tấm hình bạn gửi, chúng ta thấy một mô hình "xếp chồng" (nesting) kinh điển trong Java I/O. Nếu `OutputStream` là để ghi, thì đây chính là cách phổ biến và hiệu quả nhất để **Đọc dữ liệu văn bản** từ một tệp tin.

Để mình giải thích cơ chế "Búp bê Nga" (lớp này bọc lớp kia) mà đoạn code trong ảnh đang sử dụng nhé:

---

## 1. Phân tích 3 lớp "vàng" trong đoạn code

Trong ví dụ của bạn, dữ liệu đi qua 3 "trạm kiểm soát" trước khi đến được tay hàm `read()`:

1. **`File file = new File("data.txt")`**: Xác định vị trí tệp tin trên ổ cứng. (Như mình đã nói, nó chỉ là cái địa chỉ).
    
2. **`FileReader reader = new FileReader(file)`**: Mở một luồng đọc ký tự. Tuy nhiên, `FileReader` khá "thô sơ", nó đọc từng ký tự một từ đĩa, tốc độ sẽ rất chậm nếu file lớn.
    
3. **`BufferedReader in = new BufferedReader(reader)`**: Đây là "VIP". Nó bọc lấy `FileReader` và tạo ra một bộ đệm (buffer). Thay vì đọc từng chữ, nó đọc một mớ dữ liệu rồi cất vào RAM.
    

---

## 2. Tại sao lại dùng `BufferedReader`?

Tại sao code trong ảnh không dùng luôn `FileReader` cho gọn? Có 2 lý do cực kỳ thuyết phục:

- **Hiệu năng (Performance):** Đọc từ RAM (buffer) nhanh hơn hàng nghìn lần so với đọc trực tiếp từ ổ đĩa mỗi khi cần 1 ký tự.
    
- **Phương thức `readLine()`:** Đây chính là "vũ khí hạng nặng" xuất hiện trong ảnh. `FileReader` không thể đọc theo từng dòng, nhưng `BufferedReader` thì có thể. Nó giúp bạn lấy nguyên một dòng văn bản chỉ bằng một lệnh duy nhất.
    

---

## 3. Giải mã hàm `read(BufferedReader in)` trong ảnh

Đoạn code định nghĩa lớp `Abc` có một điểm rất thông minh về mặt thiết kế:

Java

```
public void read(BufferedReader in) {
    String s;
    try {
        s = in.readLine(); // Đọc một dòng từ file
        // ... xử lý chuỗi s ở đây
    } catch (IOException e) { 
        // Xử lý lỗi nếu file đang đọc bị lỗi hoặc ngắt kết nối
    }
}
```

- **Tính linh hoạt:** Hàm `read` này không quan tâm `BufferedReader` đến từ đâu (có thể từ một file, hoặc thậm chí là dữ liệu từ internet gửi về). Chỉ cần là `BufferedReader`, nó sẽ đọc được.
    
- **Xử lý ngoại lệ:** Như chúng ta đã thảo luận, các thao tác I/O luôn tiềm ẩn rủi ro (file bị xóa giữa chừng, lỗi ổ đĩa...), nên khối `try-catch` ở đây là bắt buộc để chương trình không bị sập.
    

---

## ⚠️ Một thiếu sót nhỏ trong ảnh bạn cần lưu ý!

Đoạn code trong ảnh rất tốt để minh họa cách đọc, nhưng nó **thiếu bước đóng luồng (`close`)**.

Trong thực tế, sau khi `abc.doSomething()` xong, bạn nên đóng `BufferedReader` để trả lại tài nguyên cho hệ điều hành. Cách tốt nhất là dùng **`try-with-resources`** mà mình đã giới thiệu ở phần Exception:

Java

```
// Cách viết chuẩn chỉnh hiện nay:
try (BufferedReader in = new BufferedReader(new FileReader("data.txt"))) {
    abc.read(in);
    abc.doSomething();
} catch (IOException e) {
    e.printStackTrace();
} // 'in' sẽ tự động đóng tại đây, cực kỳ an toàn!
```

Bạn đã nắm vững cách đọc file văn bản rồi đấy! Bạn có muốn thử sức với việc đọc dữ liệu phức tạp hơn, ví dụ như đọc một file CSV rồi chuyển nó thành một `List<Student>` không? Đây là bài tập thực hành "tổng hợp" cực hay cho tất cả các kiến thức từ đầu đến giờ đấy!

![[Pasted image 20260401204345.png]]
Tấm hình này cực kỳ thú vị! Nó đưa ra hai giải pháp hoàn toàn khác nhau để giải quyết cùng một bài toán kinh điển: **Copy File (Sao chép tệp tin)**.

Việc hiểu được sự khác biệt giữa `CopyFile` (Cách 1) và `CopyFile2` (Cách 2) sẽ giúp bạn biết cách tối ưu hóa code cực tốt trong thực tế. Hãy cùng mình "mổ xẻ" từng cách nhé!

---

## 1. Cách 1: Copy theo từng Dòng (Line-by-Line)

Trong file ảnh đầu tiên (`CopyFile`), tác giả sử dụng bộ đôi **`BufferedReader`** và **`PrintWriter`**.

- **Cách hoạt động:** Chương trình đọc nguyên một dòng chữ từ file nguồn bằng `readLine()`, sau đó ghi dòng chữ đó sang file đích bằng `println()`. Quá trình lặp đi lặp lại cho đến khi `readLine()` trả về `null` (hết file).
    
- **Ưu điểm:** Cực kỳ dễ đọc, dễ hiểu. Rất phù hợp khi bạn copy các file văn bản thuần túy (như file code `.java`, file `.txt`) và bạn muốn thao tác hoặc chỉnh sửa nội dung trên từng dòng trước khi ghi.
    
- **Nhược điểm:** **Không dùng được cho file nhị phân (Binary)** như hình ảnh (.png, .jpg), video hay file nén (.zip). Vì phương thức `println()` sẽ tự động thêm ký tự xuống dòng của hệ điều hành vào cuối mỗi dòng, làm sai lệch cấu trúc dữ liệu nhị phân nguyên bản $\rightarrow$ File copy ra sẽ bị hỏng, không mở được.
    

---

## 2. Cách 2: Copy theo từng Khối (Buffer/Block-by-Block)

Trong file ảnh thứ hai (`CopyFile2`), tác giả sử dụng mảng ký tự `char buf[] = new char[128]` làm bộ đệm.

- **Cách hoạt động:** Thay vì quan tâm file có bao nhiêu dòng, chương trình sẽ bốc từng "nắm" dữ liệu (tối đa 128 ký tự) nhét vào mảng `buf[]` rồi bê sang file đích để ghi bằng lệnh `des.write(buf, 0, charsRead)`. Vòng lặp dừng lại khi `src.read()` trả về `-1`.
    
- **Ưu điểm:** * **Tốc độ nhanh hơn rất nhiều** đối với file lớn vì nó đọc/ghi theo cụm chứ không tốn công xử lý ngắt dòng.
    
    - Đảm bảo giữ nguyên vẹn 100% định dạng thô của dữ liệu.
        
- **Nhược điểm:** Code nhìn có vẻ phức tạp hơn một chút vì phải quản lý biến đếm `charsRead` (số lượng ký tự thực tế đọc được ở lượt cuối cùng có thể ít hơn 128).
    

---

## 💡 Góc nhìn Chuyên gia: Đâu là cách tối ưu nhất?

Thực tế, cả 2 cách trong slide bài giảng của bạn đều đang dùng luồng ký tự (`FileReader` / `FileWriter`), nghĩa là chúng vẫn chỉ đang phục vụ tốt nhất cho các file **Văn bản**.

Nếu bạn muốn viết một chương trình Copy File "bất bại" – copy được **mọi loại file** trên đời từ text cho đến ảnh, nhạc, video – bạn buộc phải dùng luồng Byte:

- Thay `FileReader` bằng `FileInputStream`.
    
- Thay `FileWriter` bằng `FileOutputStream`.
    
- Kết hợp với mảng `byte[] buf = new byte[4096]` (4KB là kích thước bộ đệm lý tưởng trong thực tế).
    

---

Bạn có muốn mình viết lại đoạn code tối ưu nhất (vừa nhanh, vừa cân được mọi loại file, vừa dùng `try-with-resources` chuẩn hiện đại) dựa trên "bài mẫu" trong ảnh của bạn không? Hay bạn muốn đi tiếp một phần kiến thức nào khác?
![[Pasted image 20260401204410.png]]


Cũng giống như việc đọc dữ liệu, Java thiết kế việc ghi dữ liệu theo mô hình "lắp ráp" các lớp bọc lại với nhau (Decorator Pattern). Hãy cùng bóc tách đoạn code trong slide nhé!

---

### 1. Phân tích Ví dụ 1: Dây chuyền ghi file (Slide trên)

Trong đoạn code này, tác giả đã sử dụng 3 lớp lồng vào nhau để tạo ra một luồng ghi hoàn chỉnh:

Java

```
// 1. Chỉ định vị trí tệp trên ổ cứng
File file = new File("data.out"); 

// 2. Tạo luồng Ký tự (Character Stream) để ghi vào file đó
FileWriter writer = new FileWriter(file); 

// 3. Bọc lớp PrintWriter ra ngoài cùng để có các hàm định dạng tiện lợi
PrintWriter out = new PrintWriter(writer); 

String s = "Hello";
try {
    // 4. Ghi dữ liệu giống hệt như in ra màn hình Console
    out.println(s); 
    
    // 5. Đóng luồng (RẤT QUAN TRỌNG)
    out.close(); 
} catch (IOException e) {
    // Xử lý lỗi nếu việc ghi thất bại
}
```

**Tại sao lại cần `PrintWriter`?**

Nếu bạn chỉ dùng `FileWriter`, bạn sẽ phải ghi dữ liệu khá thủ công bằng hàm `.write("Hello\n")`. Nhưng khi bọc nó bằng `PrintWriter`, bạn được cấp cho các "vũ khí" quen thuộc y hệt như `System.out` (bởi `System.out` thực chất cũng là một `PrintStream` tương tự). Bạn có thể dùng `out.println()`, `out.print()`, hoặc `out.printf()` cực kỳ nhàn hạ.

---

### 2. Phân tích Ví dụ 2: Truyền luồng làm tham số (Slide dưới)

Ở slide thứ hai, thay vì tạo trực tiếp luồng ghi bên trong hàm, tác giả truyền hẳn đối tượng `PrintWriter` vào qua tham số của hàm:

Java

```
class Abc {
    // Hàm này nhận vào một "ống ghi" (PrintWriter) bất kỳ
    public void write(PrintWriter out) { 
        try {
            out.println(s);
            // ...
```

**Ý nghĩa thiết kế của việc này:**

Đây là một cách code rất thông minh (tính linh hoạt cao). Hàm `write()` của bạn bây giờ **không thèm quan tâm** là nó đang ghi ra file txt, ghi ra màn hình Console, hay ghi qua mạng. Miễn là bạn đưa cho nó một cái `PrintWriter`, nó sẽ in dữ liệu `s` vào đó. Bạn có thể tái sử dụng hàm này ở khắp nơi.

---

### 🌟 Lời Khuyên Của Chuyên Gia (Code Tối Ưu Hơn)

Đoạn code trong slide mang tính chất minh họa khái niệm rất tốt. Tuy nhiên, nếu áp dụng vào dự án thực tế, đoạn code trên có hai điểm rủi ro nhỏ:

1. **Lỗi ném ra từ Constructor:** Lệnh `new FileWriter(file)` bản thân nó có rủi ro ném ra `IOException` (ví dụ: ổ đĩa bị đầy, hoặc không có quyền ghi). Nên việc khởi tạo này cũng cần được đưa vào trong khối `try-catch`.
    
2. **Quên đóng file nếu bị lỗi:** Nếu dòng `out.println(s)` xảy ra lỗi và nhảy thẳng xuống `catch`, lệnh `out.close()` sẽ bị bỏ qua $\rightarrow$ Gây kẹt file.
    

**Cách viết chuẩn mực hiện đại (Sử dụng Try-with-resources của Java 7+):**

Java

```java
import java.io.*;

public class GhiFileChuan {
    public static void main(String[] args) {
        String s = "Xin chào thế giới Java!";
        
        // Khởi tạo toàn bộ dây chuyền bên trong dấu ngoặc () của try.
        // File sẽ TỰ ĐỘNG ĐÓNG dù code chạy thành công hay xảy ra ngoại lệ.
        try (PrintWriter out = new PrintWriter(new FileWriter("data.out"))) {
            out.println(s);
            System.out.println("Đã ghi file thành công!");
        } catch (IOException e) {
            System.err.println("Lỗi khi ghi file: " + e.getMessage());
        }
    }
}
```

Viết gộp lại như thế này vừa ngắn gọn, vừa đảm bảo 100% an toàn không bao giờ bị rò rỉ tài nguyên!

![[Pasted image 20260401204732.png]]
Bức ảnh mới này đưa chúng ta lên một "tầm cao mới" của việc xử lý luồng nhị phân (Byte Stream).

Nếu ở các bài trước, chúng ta chỉ mới học cách "xúc" từng byte (từ 0-255) một cách thô sơ, thì ở hai slide này, Java cung cấp cho chúng ta những công cụ để thao tác với các kiểu dữ liệu có cấu trúc hơn.

Hãy cùng phân tích 3 cấp độ thao tác với tệp dữ liệu tuần tự nhé!

---

### 1. Ba cấp độ luồng dữ liệu (Slide trên)

Java thiết kế các luồng byte theo mô hình nâng cấp dần. Tùy vào việc bạn muốn "nhét" cái gì vào file, bạn sẽ chọn bộ công cụ tương ứng:

- **Cấp 1: Dữ liệu thô (`FileInputStream` / `FileOutputStream`)**
    
    - Chỉ biết đọc/ghi từng byte rời rạc. Giống như bạn vận chuyển từng viên gạch.
        
- **Cấp 2: Kiểu dữ liệu nguyên thủy (`DataInputStream` / `DataOutputStream`)**
    
    - Dùng khi bạn muốn ghi/đọc các kiểu dữ liệu cơ bản của Java (int, double, boolean, char...). Máy sẽ tự động gom các byte lại cho bạn.
        
- **Cấp 3: Toàn bộ đối tượng (`ObjectInputStream` / `ObjectOutputStream`)**
    
    - Dùng khi bạn muốn bê nguyên một Object phức tạp (ví dụ: một đối tượng `SinhVien` gồm cả tên, tuổi, điểm số) ném thẳng vào ổ cứng. Kỹ thuật này gọi là **Serialization** (Tuần tự hóa).
        

---

### 2. Sức mạnh của `DataInputStream` & `DataOutputStream` (Slide dưới)

**Vấn đề:** Giả sử bạn muốn ghi số `100.000` (kiểu `int` - tốn 4 bytes) ra file. Nếu chỉ dùng `FileOutputStream` cơ bản, nó chỉ ghi được 1 byte (giá trị tối đa 255) mỗi lần. Bạn sẽ phải tự tay chẻ số `100.000` đó ra làm 4 mảnh nhỏ bằng các phép toán dịch bit (`>>`, `<<`) cực kỳ phức tạp.

**Giải pháp:** Lớp `DataOutputStream` ra đời để gánh vác việc đó. Bạn chỉ cần gọi một hàm duy nhất, nó sẽ tự lo phần tách byte và ghi xuống ổ cứng.

**Các "Vũ khí" ăn liền:** Thay vì chỉ có hàm `.write()` và `.read()` nhàm chán, hai lớp này cung cấp một bộ hàm tương ứng cho mọi kiểu dữ liệu nguyên thủy:

- `readInt()` / `writeInt()` (Ghi/Đọc 4 bytes)
    
- `readDouble()` / `writeDouble()` (Ghi/Đọc 8 bytes)
    
- `readBoolean()` / `writeBoolean()` (Ghi/Đọc 1 byte)
    
- ...
    

---

### 3. Nguyên tắc "Sống Còn" (Tuần tự)

Từ "tuần tự" (Sequential) trong tiêu đề slide mang một ý nghĩa cực kỳ quan trọng: **Bạn ghi dữ liệu vào file theo thứ tự nào, thì bắt buộc phải đọc ra theo đúng thứ tự đó.**

Nếu bạn ghi một số `int` rồi đến một số `double`, nhưng lúc đọc bạn lại gọi `readDouble()` trước, chương trình sẽ đọc sai số lượng byte và trả về một kết quả "rác" không thể hiểu nổi (và không báo lỗi gì cả).

**Code minh họa thực tế:**

Java

```
import java.io.*;

public class DataStreamExample {
    public static void main(String[] args) {
        String fileName = "thong_tin.bin"; // Dùng đuôi .bin vì đây là file nhị phân

        // 1. GHI DỮ LIỆU
        // Bọc FileOutputStream bằng DataOutputStream
        try (DataOutputStream dos = new DataOutputStream(new FileOutputStream(fileName))) {
            dos.writeInt(2023);         // Ghi số nguyên
            dos.writeDouble(3.14);      // Ghi số thập phân
            dos.writeBoolean(true);     // Ghi giá trị boolean
            System.out.println("Đã ghi xong!");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 2. ĐỌC DỮ LIỆU (Phải đúng thứ tự Int -> Double -> Boolean)
        try (DataInputStream dis = new DataInputStream(new FileInputStream(fileName))) {
            int nam = dis.readInt();
            double pi = dis.readDouble();
            boolean dungSai = dis.readBoolean();
            
            System.out.println("Năm: " + nam + ", Pi: " + pi + ", Check: " + dungSai);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Bạn có muốn mình giải thích tiếp về cấp độ cao nhất là `ObjectInputStream/ObjectOutputStream` (Lưu cả một đối tượng Java) hoạt động như thế nào không?

![[Pasted image 20260401204750.png]]
Đến slide này, chúng ta chính thức chạm trán với "trùm cuối" của hệ thống Nhập/Xuất trong Java: **Đọc/Ghi nguyên một Đối tượng (Object)** hay còn gọi là quá trình **Chuỗi hóa (Serialization)**.

Việc ghi một số nguyên `int` hay một dòng chữ `String` ra file thì rất dễ vì chúng là những khối dữ liệu liền mạch. Nhưng việc ghi một đối tượng lại là một bài toán phức tạp hơn rất nhiều. Slide này giải thích tại sao lại như vậy và Java giải quyết nó như thế nào.

---

### 1. Bản chất phân mảnh của Đối tượng trong bộ nhớ (Slide Ý 1)

Khi bạn tạo một đối tượng phức tạp trong Java, dữ liệu của nó không nằm co cụm tại một chỗ.

Hãy tưởng tượng bạn có một đối tượng `SinhVien`. Bên trong `SinhVien` có thuộc tính `int tuoi` (kiểu nguyên thủy) và `DiaChi diaChiNha` (kiểu đối tượng).

- Biến `tuoi` nằm gọn bên trong `SinhVien`.
    
- Nhưng biến `diaChiNha` thực chất chỉ là một "sợi dây cáp" (tham chiếu/reference) trỏ tới một vùng nhớ khác trên Heap chứa thông tin địa chỉ thực sự.
    

**Vấn đề:** Khi muốn lưu `SinhVien` xuống ổ cứng, Java không thể chỉ copy mỗi cái "vỏ" `SinhVien`. Nó phải lần theo sợi dây cáp, đi tìm đối tượng `DiaChi`, gom tất cả lại và là phẳng chúng thành một chuỗi byte liên tục (chuỗi hóa).

---

### 2. "Giấy phép" `Serializable` (Slide Ý 2)

Vì việc gom nhặt các vùng nhớ rải rác này rất phức tạp và tiềm ẩn rủi ro bảo mật (bạn có thể vô tình ghi cả những dữ liệu nhạy cảm ra file), Java yêu cầu bạn phải cấp **"giấy phép"** thì nó mới chịu làm. Giấy phép đó chính là giao diện **`java.io.Serializable`**.

**Đặc điểm của `Serializable`:**

- **Giao diện nhãn (Marker Interface):** Đây là một loại Interface cực kỳ đặc biệt vì nó **không có bất kỳ phương thức nào** bên trong. Việc bạn viết `class SinhVien implements Serializable` chỉ đơn giản là gắn một cái mác lên lớp đó, ngầm báo với máy ảo Java (JVM) rằng: _"Tôi, lập trình viên, cho phép đối tượng này được biến thành chuỗi byte để ghi ra file hoặc gửi qua mạng"_.
    
- **Hiệu ứng Domino (Quy tắc lây lan):** Đây là điểm rất hay gây lỗi cho người mới học (như slide đã nhấn mạnh ở dòng cuối). Nếu đối tượng `SinhVien` được cấp phép `Serializable`, thì tất cả các thuộc tính đối tượng bên trong nó (như `DiaChi`, `LopHoc`...) cũng **bắt buộc** phải implements `Serializable`. Chỉ cần một "mắt xích" bên trong không có giấy phép, toàn bộ quá trình sẽ sụp đổ và ném ra lỗi `NotSerializableException`.
    

---

### Tóm tắt luồng hoạt động:

1. Bạn gọi hàm ghi đối tượng: `objectOutputStream.writeObject(sinhVien);`
    
2. Java kiểm tra xem `SinhVien` có gắn mác `Serializable` không? (Không có $\rightarrow$ Báo lỗi).
    
3. Java quét toàn bộ các thuộc tính bên trong `SinhVien`. Nếu có thuộc tính đối tượng nào, nó lại kiểm tra tiếp xem đối tượng đó có `Serializable` không.
    
4. Nếu tất cả đều hợp lệ, Java sẽ "đóng gói" toàn bộ mạng lưới đối tượng này thành một chuỗi byte (101010...) và tống xuống file. Lúc đọc lên (`readObject`), nó sẽ làm quá trình ngược lại (Giải chuỗi hóa - Deserialization) để tái tạo lại mạng lưới đối tượng nguyên bản trên RAM.
    

Trong một đối tượng, đôi khi có những thông tin rất nhạy cảm như `matKhau` (mật khẩu) hoặc `soTaiKhoan` mà bạn tuyệt đối không muốn Java ghi chúng ra file khi thực hiện "chuỗi hóa". Bạn có muốn tìm hiểu về từ khóa "phép thuật" trong Java giúp bạn làm tàng hình các thuộc tính này không?
![[Pasted image 20260401204807.png]]
Tấm hình này đưa chúng ta đến một cặp bài trùng cực kỳ bá đạo khác trong Java I/O: **`DataOutputStream`** và **`DataInputStream`**.

Nếu ở các ví dụ trước chúng ta chỉ thao tác với Chuỗi (String) hoặc Ký tự (Char), thì cặp đôi này sinh ra để giúp bạn **Đọc/Ghi trực tiếp các kiểu dữ liệu nguyên thủy (Primitive Data)** như `int`, `double`, `boolean`, `char`,... xuống file mà không cần mất công đổi chúng sang dạng text.

Dưới đây là cơ chế hoạt động cực kỳ thông minh đằng sau hai đoạn code trong slide của bạn:

---

## 1. Bản chất của việc "Ghi dữ liệu nguyên thủy" (Phần trên)

Trong ảnh đầu tiên, mảng `int a[] = {2, 3, 5, 7, 11}` được ghi xuống file bằng phương thức `writeInt()`.

- **Nó hoạt động ra sao?** Một số nguyên `int` trong Java chiếm 4 byte bộ nhớ. `DataOutputStream` sẽ bê nguyên xi 4 byte nhị phân thô đó đập thẳng xuống file.
    
- **Kết quả:** Nếu bạn dùng Notepad để mở file này lên sau khi ghi, bạn sẽ thấy toàn những ký tự "ngoài hành tinh" bị lỗi font. Đừng hoảng hốt nhé! Đó là vì dữ liệu được lưu dưới dạng mã nhị phân chứ không phải mã ASCII hay UTF-8 để con người đọc được.
    

---

## 2. Nghệ thuật "Đọc dữ liệu nguyên thủy" (Phần dưới)

Vì file được lưu bằng mã nhị phân của kiểu `int`, nên cách duy nhất để đọc lại nó một cách nguyên vẹn là dùng đúng người anh em của nó: `DataInputStream` với phương thức `readInt()`.

- **Cách hoạt động:** `readInt()` sẽ tự động bốc đúng 4 byte tiếp theo từ luồng dữ liệu và ghép chúng lại thành một số nguyên `int` hoàn chỉnh cho bạn.
    
- **Vòng lặp `while(true)` vô hạn:** Đoạn code trong ảnh dùng một mẹo rất thú vị. Nó cứ đọc liên tục không ngừng nghỉ bằng vòng lặp vô hạn. Khi nào file hết dữ liệu, phương thức `readInt()` sẽ tự động ném ra ngoại lệ **`EOFException`** (End Of File).
    
- Khối `catch (EOFException e)` bên dưới chính là "điểm dừng" ngầm của vòng lặp, giúp chương trình kết thúc êm đẹp mà không bị sập.
    

---

## ⚠️ Quy tắc "Sống còn" khi dùng Data Stream

Hai lớp này bọc lấy nhau rất chặt chẽ, vì vậy bạn bắt buộc phải tuân theo nguyên tắc: **Ghi thứ tự nào thì phải đọc đúng thứ tự đó!**

> **Ví dụ:** Nếu bạn ghi vào file theo thứ tự: 1 số `int` $\rightarrow$ 1 số `double` $\rightarrow$ 1 giá trị `boolean`.
> 
> Thì khi đọc ra, bạn cũng bắt buộc phải gọi: `readInt()` $\rightarrow$ `readDouble()` $\rightarrow$ `readBoolean()`. Nếu bạn tráo đổi thứ tự (ví dụ gọi `readDouble()` trước), dữ liệu lấy ra sẽ bị nát bét và hoàn toàn vô nghĩa vì phân bổ số byte của các kiểu dữ liệu này khác nhau.

---

Bạn có nhận thấy đoạn code này cũng đang thiếu bước đóng luồng `close()` ở phần đọc file giống như ví dụ trước không? Bạn có muốn mình viết lại cả 2 quá trình Đọc và Ghi này gộp chung vào 1 class hoàn chỉnh, sạch đẹp và đúng chuẩn an toàn tài nguyên không? Hay bạn muốn đi tiếp sang chủ đề `ObjectInputStream` / `ObjectOutputStream` để lưu trữ cả một Object?