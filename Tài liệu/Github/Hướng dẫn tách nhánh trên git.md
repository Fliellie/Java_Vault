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

Trước khi bắt đầu code hoặc sau khi đã code xong nhưng chưa `comJavafx(haianh)mit`, bạn cần tạo một "con đường riêng" cho mình:

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

### 1. Sử dụng dòng lệnh (Terminal) - Cách chuẩn nhất

Mở Terminal ngay tại thư mục dự án BTL của bạn và sử dụng lệnh `git checkout` hoặc lệnh mới hơn là `git switch`.

- **Cách kiểm tra danh sách các nhánh đang có:**
    
    Bash
    
    ```
    git branch
    ```
    
    _(Nhánh bạn đang đứng sẽ có dấu sao `*` và màu xanh)._
    
- **Cách chuyển sang một nhánh đã tồn tại:**
    
    Bash
    
    ```
    git switch tên-nhánh
    # Ví dụ: git switch dev-server
    ```
    
    _(Lệnh cũ là `git checkout tên-nhánh` vẫn dùng tốt)._
    
- **Cách tạo nhánh mới và chuyển sang luôn:**
    
    Bash
    
    ```
    git switch -c nhánh-mới
    # Ví dụ: git switch -c feature-login
    ```
    

---

### 2. Sử dụng VS Code (Nếu bạn dùng để code Java)

Nếu bạn đang mở dự án bằng VS Code, bạn không cần gõ lệnh:

1. Nhìn xuống **góc dưới cùng bên trái** của thanh trạng thái (Status Bar).
    
2. Bạn sẽ thấy tên nhánh hiện tại (ví dụ: `main` hoặc `master`).
    
3. **Click chuột trái** vào tên nhánh đó.
    
4. Một danh sách các nhánh sẽ hiện ra ở giữa phía trên màn hình. Bạn chỉ cần chọn nhánh muốn chuyển sang là xong.
    

---

### 3. Lưu ý cực kỳ quan trọng: "Stash" trước khi Switch

Đây là lỗi mà các nhóm làm BTL hay gặp phải: Bạn đang code dở ở nhánh `main`, nhưng chợt nhớ ra phải sang nhánh `dev` để sửa lỗi. Nếu bạn switch ngay, Git có thể báo lỗi vì code dở chưa được lưu (commit).

- **Giải pháp 1 (Nếu code đã xong):** `git add .` và `git commit -m "tạm lưu"` trước khi switch.
    
- **Giải pháp 2 (Nếu code chưa xong và không muốn commit):** Dùng lệnh **Stash** để tạm cất code vào "túi":
    
    Bash
    
    ```
    git stash        # Tạm cất code đang làm dở
    git switch dev   # Chuyển nhánh thoải mái
    # Sau khi quay lại nhánh cũ:
    git stash pop    # Lấy code dở ra làm tiếp
    ```