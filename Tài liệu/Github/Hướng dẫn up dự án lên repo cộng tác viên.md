### Bước 1: Lấy mã nguồn về máy mình (Clone)

Nếu bạn chưa có thư mục code đó trên máy Linux, hãy copy link HTTPS của repo đó và gõ:



```bash
git clone https://github.com/Ten-Ban-Than/Ten-Repo.git
```

Sau đó, di chuyển vào thư mục vừa tải về:



```bash
cd Ten-Repo
```

---

### Bước 2: Thêm file hoặc sửa code

Bây giờ bạn cứ tự nhiên tạo file mới (ví dụ file `Login.fxml`) hoặc sửa các file có sẵn trong thư mục đó.

---

### Bước 3: Quy trình "3 bước thần thánh" để đẩy file

**1. Đánh dấu các thay đổi:**


```bash
git add .
```

_(Dấu chấm nghĩa là lấy tất cả những gì bạn vừa thêm/sửa)._

**2. Ghi chú nội dung thay đổi (Commit):**

```bash
git commit -m "Bạn A thêm giao diện đăng nhập"
```

**3. Đẩy lên GitHub (Push):**

```bash
git push origin main
```

_(Lưu ý: Nếu nhánh chính của bạn là `master` thì thay `main` bằng `master` nhé)._

---


#### 2. Lỗi "Rejected" 

Nếu bạn của bạn vừa đẩy file B lên mà bạn chưa có, Git sẽ chặn bạn push file C.

- **Giải pháp:** Gõ `git pull origin main` để rước file B về máy mình trước, sau đó mới gõ `git push` lại.
    

---

### 💡 Mẹo nhỏ cho nhóm bạn:

Để kiểm tra xem mình đã kết nối đúng với "nhà" của bạn mình chưa, bạn gõ:

Bash

```
git remote -v
```