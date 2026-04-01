### Protected(kế thừa)
![[Pasted image 20260228204913.png]]
`` Lớp con có thể kế thừa từ 1 lớp cha
`` Lớp con có thể sửa phương thức trong lớp cha
`` Cha không biết con
`` Phạm vi này thì có thể dùng trong chung thư mục, hoặc cùng cha con
### Private
**`private` (Riêng tư tuyệt đối):**

- **Ý nghĩa:** Chỉ code nằm _bên trong chính cái class đó_ mới được xài.

### Default
- **Ý nghĩa:** Khi bạn không ghi từ khóa nào cả, Java sẽ mặc định hiểu là những file code nằm _chung một package (chung thư mục)_ thì mới thấy nhau.
    
- **Ví dụ:** Các file `Dat.java`, `Da.java`, `Co.java` nằm chung trong package `world`. Chúng có thể tự do lấy thông số của nhau để tạo ra bản đồ, nhưng nhân vật ở package khác thì không can thiệp được.

### Public
- **Ý nghĩa:** Hoàn toàn công khai. Gọi từ bất kỳ file nào, bất kỳ thư mục nào cũng được.
    
- **Ví dụ:** Nút `StartGame()` ở menu chính. Rõ ràng bạn muốn bất kỳ thành phần nào cũng có thể gọi hàm này để bắt đầu trò chơi.