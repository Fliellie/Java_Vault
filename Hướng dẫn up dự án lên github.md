## Bước 1: Chuẩn bị trên GitHub

Trước khi gõ lệnh, bạn cần có một "ngôi nhà" cho file của mình:

1. Đăng nhập vào [GitHub](https://github.com).
    
2. Nhấn nút **New** để tạo Repository mới.
    
3. Đặt tên Repo và nhấn **Create repository**.
    
4. **Quan trọng:** GitHub hiện dùng **Personal Access Token (PAT)** thay cho mật khẩu khi đẩy file qua Terminal. Bạn hãy vào _Settings > Developer settings > Personal access tokens_ để tạo một cái nhé.
    

---

## Bước 2: Cài đặt Git trên Ubuntu

Mở Terminal (`Ctrl + Alt + T`) và kiểm tra xem máy đã có Git chưa:



```bash
sudo apt update
sudo apt install git
```

Sau đó, hãy giới thiệu bản thân với Git (chỉ cần làm lần đầu):



```bash
git config --global user.name "Tên Của Bạn"
git config --global user.email "email@vidu.com"
```

---

## Bước 3: Quy trình đưa file lên (The Workflow)

Giả sử bạn có một file tên là `tailieu.pdf` trong thư mục `Documents/MyProject`.

## 1. Khởi tạo Git trong thư mục

Di chuyển đến thư mục chứa file và khởi tạo:

Bash

```bash
cd ~/Documents/MyProject
git init
```

## 2. Thêm file vào "vùng chờ" (Staging Area)

Bạn có thể thêm một file cụ thể hoặc tất cả các file:

- Thêm file cụ thể: `git add tailieu.pdf`
    
- Thêm tất cả: `git add .`
    

## 3. Ghi lại thay đổi (Commit)

Đặt một cái tên cho lần đăng này để sau này dễ tìm lại:



```bash
git commit -m "Đăng file tài liệu đầu tiên"
```

## 4. Kết nối và Đẩy lên GitHub

Bây giờ, hãy copy đường dẫn Repository bạn vừa tạo (có dạng `https://github.com/user/repo.git`) và chạy các lệnh sau:



```bash
# Kết nối với GitHub (chỉ làm lần đầu cho mỗi project)
git remote add origin https://github.com/ten-user/ten-repo.git

# Đổi tên nhánh chính thành main (chuẩn mới của GitHub)
git branch -M main

# Đẩy file lên
git push -u origin main
```

> **Lưu ý:** Khi Terminal hỏi **Username**, hãy nhập tên đăng nhập GitHub. Khi hỏi **Password**, hãy dán cái **Personal Access Token** mà bạn đã tạo ở Bước 1 vào (nó sẽ không hiện ký tự khi dán, cứ yên tâm nhấn Enter).
Khi bạn cố gõ lại lệnh `git remote add origin <link_mới>`, Git sẽ lập tức chặn lại và báo lỗi đại loại như: `fatal: remote origin already exists.` (Tên origin đã được sử dụng).

Để giải quyết, bạn không dùng lệnh `add` (thêm mới) nữa, mà phải dùng lệnh **đổi link (set-url)** hoặc **xóa link cũ đi rồi thêm lại**. Dưới đây là 2 cách xử lý nhanh gọn nhất:

### Cách 1: Ghi đè link mới lên trực tiếp (Khuyên dùng)

Đây là cách gọn gàng nhất để cập nhật lại đường dẫn HTTP mới mà không cần xóa đi cài lại. Bạn chỉ cần mở Terminal tại thư mục project và gõ lệnh:



```bash
git remote set-url origin <link_http_mới_của_bạn>
```

_(Ví dụ: `git remote set-url origin https://github.com/ten-cua-ban/repo-moi.git`)_

### Cách 2: Xóa hẳn link cũ đi rồi gán lại từ đầu

Nếu bạn muốn "làm sạch" hoàn toàn, hãy tháo cái link cũ ra trước rồi mới gắn cái mới vào:



```bash
# Bước 1: Cắt đứt kết nối với link cũ
git remote remove origin

# Bước 2: Gắn kết nối với link mới
git remote add origin <link_http_mới_của_bạn>
```

---

**🛠 Mẹo nhỏ để kiểm tra xem đã thành công chưa:** Sau khi thực hiện xong 1 trong 2 cách trên, bạn hãy gõ lệnh này để kiểm tra xem Git đã nhận đúng địa chỉ nhà mới chưa nhé:



```bash
git remote -v
```



## 5. kiểm tra phiên bản

Để kiểm tra xem Git đã được cài đặt trên máy Ubuntu của bạn hay chưa, bạn chỉ cần dùng một câu lệnh cực kỳ đơn giản trong Terminal.

Bạn mở Terminal (`Ctrl + Alt + T`) và gõ:

Bash

```bash
git --version
```





## 6. kiểm tra khai sinh 
**3. Nếu danh sách quá dài và bạn lười tìm:** Bạn có thể kiểm tra đích danh từng mục một bằng cách gõ:

- `git config user.name`
    
- `git config user.email` _(Nếu nó hiện ra tên/email ngay bên dưới là xong, nếu nó im lặng hoặc báo lỗi nghĩa là chưa có)._
    

---
Để kiểm tra xem bạn đã "khai sinh" (cấu hình) tên và email cho Git trên máy Ubuntu của mình chưa, bạn dùng lệnh này trong Terminal nhé:

Bash

```bash
git config --list
```

## Cách đọc kết quả:

Sau khi chạy lệnh, bạn hãy tìm hai dòng sau trong danh sách hiện ra:

- `user.name=Tên Của Bạn`
    
- `user.email=email@vidu.com`





# Cài đặt GitHub CLI
## Để Ubuntu "sướng" như Windows:

Nếu bạn muốn trải nghiệm đăng nhập bằng trình duyệt (giống hệt Windows), bạn hãy cài **GitHub CLI** (công cụ chính chủ của GitHub dành cho dòng lệnh). Chạy các lệnh này:

Bash

```bash
# Cài đặt GitHub CLI
type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```


Vì bạn đang ở trong thư mục `Java` (theo như dòng `➜ Java git:(main)` bạn gửi lúc nãy), hãy thực hiện quy trình **3 bước thần thánh** này:

## 1. Gom hàng (Staging)

Bạn chọn những file nào muốn đưa lên. Nếu muốn đưa **tất cả** mọi thứ trong thư mục hiện tại lên:

Bash

```bash
git add .
```

## 2. Dán nhãn (Commit)

Đặt một cái tên cho lần đăng này để sau này biết mình đã làm gì:

Bash

```bash
git commit -m "Đăng sản phẩm đầu tiên từ Ubuntu"
```

## 3. Đẩy hàng (Push)

Vì lúc nãy bạn đã cấu hình xong xuôi, giờ chỉ cần một lệnh duy nhất:

Bash

```bash
git push
```

_(Nếu nó có hỏi lại Username/Password một lần cuối, hãy dùng cái Token bạn đã tạo. Nhưng nếu bạn đã dùng `gh auth login` thành công thì nó sẽ chạy vèo một cái là xong)._

---

## Kiểm tra kết quả

Bạn hãy lên lại trang GitHub của mình, F5 (tải lại trang) cái Repository `requirements-for-BTL`. Nếu thấy file hiện ra kèm cái dòng chữ "Đăng sản phẩm đầu tiên từ Ubuntu" thì có nghĩa là bạn đã thành công rực rỡ!

## Mẹo cho lần sau:

Mỗi khi bạn sửa code hoặc thêm file mới vào thư mục này, bạn chỉ cần lặp lại bộ 3:

1. `git add .`
    
2. `git commit -m "mô tả thay đổi"`
    
3. `git push`