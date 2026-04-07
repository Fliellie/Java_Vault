Giả sử bạn đang ở nhánh main:
### Tạo nhánh mới là nhiệm vụ riêng:
```bash
git checkout -b feature/tên nhánh
```


### Đẩy nhánh mới đó lên git:
```bash
git push -u origin feature/tên nhánh
```

### Bước 1: Tạo và chuyển sang nhánh mới

Trước khi bắt đầu code hoặc sau khi đã code xong nhưng chưa `commit`, bạn cần tạo một "con đường riêng" cho mình:

Bash

```
git checkout -b feature/ten-cua-ban
```

- **`checkout -b`**: Là lệnh "tạo mới và nhảy sang luôn".
    
- **`feature/ten-cua-ban`**: Bạn thay bằng tên tính năng bạn đang làm (ví dụ: `feature/login`, `feature/gui`).
    

### Bước 2: Lưu lại các thay đổi vào nhánh này

Bây giờ, mọi thao tác `add` và `commit` sẽ chỉ có tác dụng trên nhánh mới này:

Bash

```
git add .
git commit -m "Mô tả việc bạn vừa làm"
```

---

### Bước 3: Đẩy nhánh mới lên GitHub

Đây là lệnh quan trọng nhất để trả lời câu hỏi của bạn. Thay vì gõ `git push`, bạn phải chỉ định rõ tên nhánh:

Bash

```
git push -u origin feature/ten-cua-ban
```

**Tại sao phải thêm `-u origin ...`?**

- **Lần đầu tiên:** Bạn cần báo cho GitHub biết: "Hãy tạo một nhánh tên là `feature/ten-cua-ban` trên Server và kết nối nó với nhánh Local của tôi".
    
- **Từ lần sau:** Bạn chỉ cần gõ `git push` ngắn gọn, Git sẽ tự hiểu là đẩy vào đúng cái nhánh đó.