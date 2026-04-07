### 🟢 Mức độ 1: Tập làm quen với Đọc/Ghi cơ bản

**Bài 1: Ghi đè file**
- [x] 

> **Đề bài:** Viết mã giả mở một file tên là `output.txt` để ghi. Ghi vào đó chuỗi `"Hello World"`. Đảm bảo file được đóng an toàn kể cả khi xảy ra lỗi.

**Bài 2: Copy dữ liệu thô**
- [x] 

> **Đề bài:** Bạn cần copy một file ảnh `avatar.png` sang `backup.png`. Hãy viết mã giả sử dụng luồng hướng Byte (Stream) để đọc từng byte từ file nguồn và ghi sang file đích.

**Bài 3: Đọc file text dòng bằng dòng**
- [x] 

> **Đề bài:** Viết mã giả sử dụng luồng hướng Ký tự có bộ đệm (BufferedReader) để đọc file `lyrics.txt` và in từng dòng ra màn hình cho đến khi hết file.



**Bài 4**
- [ ] 

> **Đề bài:** Viết mã giả đọc file `setup.txt`. Mỗi dòng có dạng `key=value`. Hãy dùng thư viện hiện đại để đọc tất cả các dòng, lọc bỏ các dòng không chứa dấu `=`, và nạp chúng vào một `Map`.

---

### 💡 Gợi ý nhỏ về "Cú pháp mã giả" bạn có thể dùng:

Để dễ viết, bạn có thể dùng các từ khóa mô phỏng như:

- `Thử { ... } Bắt (Tên_Lỗi) { ... } Cuối_cùng { ... }` (Thay cho try-catch-finally)
    
- `Mở_File_Để_Ghi("tên_file")`
    
- `Đọc_Dòng()`, `Ghi_Dòng()`