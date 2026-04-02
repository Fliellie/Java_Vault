### 1. Luồng Byte (Byte Stream): Kẻ Làm Việc Với Số Máy

Đây là luồng dữ liệu nguyên thủy và cơ bản nhất trong Java. Nó đối xử với mọi dữ liệu như là một chuỗi các số nhị phân (các byte 8-bit).

- **Đơn vị thao tác:** Từng byte (8-bit) một.
    
- **Các lớp Nền tảng:** Mọi luồng byte đều được kế thừa từ hai lớp trừu tượng tối cao:
    
    - `java.io.InputStream`: Dùng để **đọc** dữ liệu (từ file, mạng...) vào chương trình.
        
    - `java.io.OutputStream`: Dùng để **ghi** dữ liệu từ chương trình ra ngoài.
        
- **Mục đích sử dụng (Nguồn/Đích):** Như slide có ghi ("File, network connection, memory block"), luồng byte được dùng để vận chuyển dữ liệu thô.
    
- **Khi nào nên dùng?** Khi bạn làm việc với **dữ liệu nhị phân (binary data)**. Ví dụ: copy một bức ảnh `.png`, tải một bài nhạc `.mp3`, stream một video, hoặc gửi các gói tin qua mạng (network socket). Lúc này, chương trình không cần (và không nên) hiểu nội dung bên trong là chữ gì, nó chỉ cần bê nguyên vẹn các byte từ nơi này sang nơi khác.
    

---

### 2. Luồng Char (Character Stream): Sứ Giả Của Ngôn Ngữ

Máy tính thì hiểu byte, nhưng con người thì đọc chữ. Lịch sử ngành IT từng đau đầu vì vấn đề "loạn font" khi mỗi quốc gia dùng một bảng mã khác nhau. Java giải quyết việc này bằng cách dùng bảng mã **Unicode (16-bit)** làm chuẩn nội bộ.

Luồng Char ra đời như một "bộ phiên dịch" tự động: Nó đọc các luồng byte nguyên thủy, sau đó dịch chúng thành các ký tự Unicode để bạn có thể dễ dàng thao tác dưới dạng chuỗi (String).

- **Đơn vị thao tác:** Từng ký tự (Character - 16-bit Unicode).
    
- **Các lớp Nền tảng:** * `java.io.Reader`: Dùng để **đọc** luồng văn bản.
    
    - `java.io.Writer`: Dùng để **ghi** luồng văn bản.
        
- **Khi nào nên dùng?** Bất cứ khi nào bạn thao tác với **dữ liệu văn bản (text data)**. Ví dụ: đọc file `.txt`, file cấu hình `.xml`, file `.json`, hoặc đọc mã nguồn `.java`.
    

---

### 3. Bảng Phân Biệt Nhanh Để Viết Code Không Bị Sai

Đây là kim chỉ nam để bạn chọn đúng class khi code:

|**Tiêu chí**|**Luồng Byte (Byte Stream)**|**Luồng Ký Tự (Character Stream)**|
|---|---|---|
|**Bản chất**|Đọc/Ghi dữ liệu thô (Raw data)|Đọc/Ghi văn bản con người đọc được|
|**Kích thước xử lý**|1 Byte (8-bit) mỗi lần|1 Ký tự (16-bit) mỗi lần|
|**Lớp cha cao nhất**|`InputStream` / `OutputStream`|`Reader` / `Writer`|
|**Tên class thường gặp**|`FileInputStream`, `FileOutputStream`, `BufferedInputStream`|`FileReader`, `FileWriter`, `BufferedReader`|
|**Ví dụ thực tế**|Copy file ảnh `avatar.jpg`|Đọc nội dung file `danh_sach.csv`|

**Quy tắc ngầm:** Nếu mở một file bằng Notepad lên mà bạn đọc được chữ bình thường $\rightarrow$ Dùng **Luồng Char**. Nếu mở lên mà thấy toàn ký tự giun dế loằng ngoằng $\rightarrow$ Dùng **Luồng Byte**.
##  Cây inputStream và outputStream
![[Pasted image 20260401203046.png]]
##  Cây reader và writer
![[Pasted image 20260401203108.png]]
## Đối tượng vào ra
![[Pasted image 20260401203154.png]]


![[Pasted image 20260401204119.png]]

