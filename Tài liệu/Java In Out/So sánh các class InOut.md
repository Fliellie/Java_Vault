## 1. Chia phe: Hướng Byte vs Hướng Ký tự

Trước khi đi vào chi tiết, bạn chỉ cần nhớ quy tắc "vàng" này để không bao giờ bị nhầm lẫn:

- **Phe Byte Stream (Luồng hướng Byte):** Tên luôn kết thúc bằng chữ **`Stream`**. Đọc/ghi dữ liệu thô (ảnh, video, file nhị phân, nhạc...).
    
- **Phe Character Stream (Luồng hướng Ký tự):** Tên luôn kết thúc bằng chữ **`Reader`** hoặc **`Writer`**. Đọc/ghi dữ liệu dạng văn bản (chữ viết đọc được).
    

---

## 2. So sánh chi tiết các đối tượng trong ảnh

Để bạn dễ hình dung sự khác nhau, mình lập một bảng so sánh dựa đúng trên các từ khóa trong ảnh của bạn:

|**Đối tượng**|**Thuộc phe nào?**|**Nhiệm vụ & Đặc điểm**|**Khi nào nên dùng?**|
|---|---|---|---|
|**`InputStream` / `OutputStream`**|Luồng Byte|Là 2 lớp trừu tượng (Abstract class) cấp cao nhất của phe Byte. Chúng chỉ định nghĩa các quy tắc chung chứ không trực tiếp dùng được.|Không dùng trực tiếp, chỉ dùng làm kiểu dữ liệu chung hoặc tự chế custom stream.|
|**`Reader` / `Writer`**|Luồng Ký tự|Là 2 lớp trừu tượng cấp cao nhất của phe Ký tự. Định nghĩa cách đọc/ghi các ký tự Unicode (2 bytes).|Giống như trên, là "ông tổ" của các lớp đọc/ghi file text.|
|**`FileInputStream` / `FileOutputStream`**|Luồng Byte|Làm việc trực tiếp với file trên ổ cứng. Đọc/ghi dữ liệu thô cấp thấp theo từng byte đơn lẻ.|Khi cần copy file ảnh, file nén, hoặc file nhị phân (giống Bài 3 của bạn).|
|**`FileReader` / `FileWriter`**|Luồng Ký tự|Làm việc trực tiếp với file văn bản. Đọc/ghi trực tiếp các ký tự chữ viết hoa, viết thường, tiếng Việt...|Khi cần đọc/ghi file `.txt` đơn giản, ngắn gọn.|
|**`BufferedInputStream`**|Luồng Byte|Là "vỏ bọc" có thêm bộ nhớ đệm (Buffer). Thay vì đọc từng byte lắt nhắt từ ổ cứng, nó bốc một mớ lớn nạp vào RAM rồi xử lý dần.|Dùng bọc ngoài `FileInputStream` để tăng tốc độ đọc file nhị phân dung lượng lớn.|
|**`BufferedReader`**|Luồng Ký tự|"Vỏ bọc" có bộ nhớ đệm cho file text. Cực kỳ bá đạo nhờ có hàm đọc nguyên một dòng chữ: `readLine()`.|Dùng bọc ngoài `FileReader` để đọc file cấu hình, file log (giống Bài 5 của bạn).|

---

## 💡 Tóm tắt mẹo phối hợp (Design Pattern)

Trong Java I/O, người ta thường lồng các đối tượng này vào nhau (gọi là mô hình Decorator). Bạn cứ nhớ công thức lồng ghép kinh điển này nhé:

1. **Nếu làm việc với file văn bản :**
    
    $$\text{BufferedReader} \rightarrow \text{FileReader} \rightarrow \text{Tệp tin trên ổ cứng}$$
    
    _(Code: `new BufferedReader(new FileReader("file.txt"))`)_
    
1. **Nếu làm việc với file nhị phân :**
    
    $$\text{DataInputStream} \rightarrow \text{BufferedInputStream} \rightarrow \text{FileInputStream} \rightarrow \text{Tệp tin trên ổ cứng}$$
    

Ở các câu hỏi trước, chúng mình đã cùng nhau mổ xẻ `DataOutputStream` (dùng để ghi dữ liệu). Thì `DataInputStream` chính là **mảnh ghép ngược lại hoàn hảo** của nó đấy!

Để dễ hiểu nhất, chúng ta hãy tiếp tục dùng hình ảnh ẩn dụ về dòng nước nhé:

- **`FileInputStream`** giống như **đường ống hút nước thô sơ** từ file trên ổ cứng vào chương trình. Nó chỉ biết hút từng giọt nước (từng byte dữ liệu) lên một cách máy móc.
    
- **`DataInputStream`** giống như **bộ vòi chia và đo lường thông minh** được gắn vào đầu đường ống hút kia. Nó sẽ gộp các giọt nước thô lại và giúp bạn lấy ra đúng can 4 lít (đọc ra số nguyên `int`), can 8 lít (đọc ra số thực `double`),... một cách cực kỳ chính xác.
    

---

### 1. DataInputStream là gì?

`DataInputStream` là một lớp thuộc dòng **Processing Stream** (Luồng xử lý) trong Java.

- **Nhiệm vụ:** Nó giúp ứng dụng của bạn đọc các kiểu dữ liệu nguyên thủy trong Java (`int`, `double`, `boolean`, `char`...) từ một luồng dữ liệu nhị phân một cách nguyên bản và tiện lợi.
    
- **Đặc điểm:** Nó không làm việc trực tiếp với file trên ổ cứng. Nó bắt buộc phải bọc (wrap) quanh một luồng cơ sở khác, điển hình nhất là `FileInputStream`.
    

---

### 2. Các hàm "mì ăn liền" đỉnh nhất của DataInputStream

Thay vì bạn phải tự đọc từng byte rồi dùng các phép toán dịch bit phức tạp để ghép chúng lại thành một con số, `DataInputStream` cung cấp sẵn các phương thức đọc cực nhàn:

- `readInt()`: Đọc 4 byte tiếp theo và dịch thành một số nguyên `int`. (Giống như bạn dùng ở Bài 3 và Bài 4).
    
- `readDouble()`: Đọc 8 byte tiếp theo và dịch thành một số thực `double`.
    
- `readBoolean()`: Đọc 1 byte và dịch thành giá trị `true`/`false`.
    
- `readUTF()`: Đọc một chuỗi văn bản được mã hóa theo chuẩn UTF-8 sửa đổi của Java.
    

---

### 💡 Quy tắc "Cặp đôi hoàn cảnh" cực kỳ quan trọng

Đây là điều cốt lõi bạn cần nhớ khi đi làm: **Bạn dùng cái gì để ghi thì phải dùng đúng cái đó để đọc.**

- Nếu bạn dùng `DataOutputStream.writeInt(100)` để ghi số `100` xuống file.
    
- Thì ở file đọc, bạn **bắt buộc** phải dùng `DataInputStream.readInt()` để lấy lại số `100` đó.
    

Nếu bạn cố tình dùng `readDouble()` hoặc `readUTF()` để đọc dữ liệu vốn được ghi bằng `writeInt()`, Java sẽ cố gắng dịch mớ byte đó sai cách, dẫn đến việc bạn nhận được một giá trị "rác" hoặc chương trình bị nổ lỗi `EOFException` ngay lập tức.
