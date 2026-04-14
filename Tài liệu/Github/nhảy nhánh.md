**Bước 1: Cập nhật danh sách nhánh từ GitHub về máy bạn** Nếu bạn không chạy lệnh này, máy bạn sẽ không biết nhánh mới của bạn mình vừa tạo là gì.

Bash

```
git fetch origin
```

**Bước 2: Chuyển sang nhánh đó**

Bash

```
git checkout feature/server-logic
# Hoặc lệnh mới hơn:
git switch feature/server-logic
```