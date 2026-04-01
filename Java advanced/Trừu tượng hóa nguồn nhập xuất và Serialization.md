Bức ảnh mới này đề cập đến hai khái niệm cực kỳ mang tính "thực chiến" trong kỹ thuật phần mềm hiện đại: **Trừu tượng hóa (Abstraction)** và **Chuỗi hóa bằng JSON**.

Đặc biệt, khái niệm JSON ở slide dưới chính là thứ mà bạn sẽ dùng hàng ngày nếu sau này đi làm Web hoặc Mobile. Hãy cùng mổ xẻ nhé!

---

### 1. Trừu tượng hóa nguồn Nhập/Xuất (Slide trên)

Ở những bài đầu, chúng ta thường viết code mở file ngay bên trong hàm đọc dữ liệu. Slide này khuyên chúng ta **không nên làm thế**, mà hãy truyền một luồng (như `Scanner`) từ ngoài vào qua tham số của hàm.

**Tại sao lại nói nó "Tăng tính tái sử dụng"?**

Hãy nhìn vào đoạn code trong slide:

Java

```
class Abc {
    // Hàm này nhận vào một cái máy quét (Scanner)
    // Nó KHÔNG QUAN TÂM máy quét này đang quét cái gì!
    public void read(Scanner sc) { 
        // ... code đọc dữ liệu bằng sc.nextLine() ...
    }
}
```

Nhờ cách viết này, hàm `read()` của bạn trở nên "bất tử" và dùng được trong mọi hoàn cảnh. Bạn chỉ cần viết hàm này **một lần duy nhất**, nhưng có thể gọi nó theo vô số cách khác nhau:

Java

```
Abc obj = new Abc();

// 1. Dùng hàm read() để đọc từ Bàn phím
Scanner nhapTuBanPhim = new Scanner(System.in);
obj.read(nhapTuBanPhim);

// 2. Vẫn hàm read() đó, nhưng dùng để đọc từ File txt
Scanner nhapTuFile = new Scanner(new File("du_lieu.txt"));
obj.read(nhapTuFile);

// 3. Vẫn hàm read() đó, dùng để đọc từ một chuỗi String có sẵn
Scanner nhapTuChuoi = new Scanner("Dữ liệu ảo để test code");
obj.read(nhapTuChuoi);
```

**Bài học rút ra:** Đừng "trói chết" code của bạn vào một file cụ thể hay bàn phím cụ thể. Hãy truyền các đối tượng đại diện cho luồng (`Scanner`, `PrintWriter`, `InputStream`...) vào hàm. Code của bạn sẽ linh hoạt hơn gấp 100 lần.

---

### 2. JSON Serialization - "Vị Vua" của thế giới mạng (Slide dưới)

Dù phần nội dung bị cắt mất, nhưng chỉ cần dòng tiêu đề **JSON Serialization** là đủ để thấy môn học này đang cập nhật kiến thức rất sát với thực tế.

Ở bài trước, chúng ta đã học cách ghi đối tượng bằng `Serializable` của Java (ghi ra file `.bin`). Nhưng cách đó có một **nhược điểm chí mạng**:

- Nó ghi ra mã nhị phân, con người mở lên không đọc được.
    
- Nó là "đồ nhà làm" của Java. Nếu bạn gửi file nhị phân đó cho một hệ thống viết bằng Python, C# hay JavaScript, các ngôn ngữ kia sẽ "khóc thét" vì không hiểu cách giải mã.
    

**Giải pháp: Dùng JSON (JavaScript Object Notation)**

Thay vì biến đối tượng thành chuỗi byte nhị phân, chúng ta biến đối tượng thành một chuỗi văn bản (String) theo định dạng chuẩn quốc tế gọi là JSON.

Ví dụ, một đối tượng `SinhVien(ten="Nam", tuoi=20)` khi "chuỗi hóa" ra JSON sẽ trông như thế này:

JSON

```
{
  "ten": "Nam",
  "tuoi": 20
}
```

**Tại sao JSON lại vô đối hiện nay?**

1. **Con người đọc được:** Nhìn vào là hiểu ngay dữ liệu có gì.
    
2. **Đa ngôn ngữ:** Mọi ngôn ngữ lập trình trên thế giới (Java, Python, JS, C++...) đều có công cụ để đọc và ghi định dạng JSON cực kỳ dễ dàng. Đây là tiêu chuẩn để các hệ thống (như App điện thoại và Server) nói chuyện với nhau.
    

_Lưu ý:_ Java không tự làm việc này tốt, người ta thường dùng các thư viện bên ngoài bổ sung vào như **Google Gson** hoặc **Jackson**.

Bạn có muốn mình demo thử một đoạn code nhỏ (dùng thư viện Gson) để xem việc biến một đối tượng Java thành chuỗi JSON nó "ảo diệu" và dễ dàng như thế nào không?