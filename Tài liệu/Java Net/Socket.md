### 1. Phác họa quy trình (Workflow)

Trước khi viết code, bạn cần hiểu luồng đi của dữ liệu:

1. **Server** mở một cổng (Port) và ngồi đợi (`accept`).
    
2. **Client** tìm địa chỉ IP của Server và gõ cửa kết nối.
    
3. **Server** đồng ý, một "đường ống" (Socket) được hình thành.
    
4. Hai bên dùng **InputStream** (để nghe) và **OutputStream** (để nói).



## Các cách sử dụng
```java

// Tạo ống và kết nối nó đến server bất kì
try (Socket socket = new Socket("localhost", 9090))
//Mở server để đợi người dùng kết nối
try (ServerSocket serverSocket = new ServerSocket(9090))
// Đợi kết nối từ Client
Socket socket = serverSocket.accept();
// Thiết lập bộ đọc viết 
BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream())); PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
// đọc tin nhắn từ Client và phản hồi
String tinNhan = in.readLine(); 
System.out.println("Client nói: " + tinNhan);
out.println("Server đã nhận được tin: " + tinNhan);
// Bộ đọc ghi ở phía Client
PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
// Gửi tin nhắn đi
out.println("Chào Server, tôi là Client đây!"); 
// Nghe phản hồi từ Server 
String phanHoi = in.readLine(); 
System.out.println("Server phản hồi: " + phanHoi);

```