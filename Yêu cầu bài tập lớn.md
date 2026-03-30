1. dễ bảo trì và mở rộng
2. phải commit thường xuyên trên github cho từng bản cập nhật
3. bất kì thành viên nào không hiểu mã nguồn cả nhóm 0 điểm
4. tên hệ thống: bidding
## Chức năng cần có
1. đăng kí đăng nhập
2. vai trò:
		- tham gia đấu giá: bidder
		- seller: người bán
		- người trọng tài: admin
3. thêm, xóa sản phẩm (quyền của admin và người bán)
4. thông tin sản phẩm:
		1. tên sản phẩm
		2. mô tả sản phẩm
		3. giá khởi điểm
		4. giá hiện tại
		5. thời gian bắt đầu đăng bán
		6. thời gian kết thúc
5. chức năng player:
	1. người chơi đấu giá
		1. chọn phiên đang đấu giá
		2. xem được thông tin sản phẩm
		3. giá sàn
		4. bước giá
		5. lượng người online
		6. lượng người đã đấu giá cho sản phẩm
		7. lựa chọn autobidding
			1. nhập maxbid(giá lớn nhất mà bot đem đi đấu giá)
			2. bước giá: (bằng bước giá sản phẩm hoặc cao hơn)
			3. chấp nhận cam kết
		8. lựa chọn normal bidding:
			1. chấp nhận cam kết
			2. nhập giá đem đi đấu
				1. yêu cầu giá nhập lớn hơn giá lớn nhất hiện tại
				2. giá phải cao hơn giá hiện tại bước giá
	2. cập nhật người đang dẫn đầu phiên (người có tiền lớn nhất)
	3. tự động dừng phiên và lấy thông tin người chiến thắng khi hết thời gian đấu giá
	4. trước khi một player có thể đấu giá, sẽ hiện lên bảng cam kết với bên bán và của bên chủ trì đấu giá, cam kết:
		1. không tự ý huỷ kết quả đấu giá
		2. nếu hủy thì phải trả cho bên bán lượng tiền bằng giá khởi điểm, đồng thời bị đưa vào danh sách đen(danh sách đen tức khi bạn tham gia các phiên đấu giá khác, khi có người đấu giá ngang bạn thì bạn sẽ không được ưu tiên, tuy nhiên nếu player còn lại cũng bị vào danh sách đen thì mức ưu tiên là tương tự)
		3. nếu là hủy lần 2 thì vẫn phải bồi thường lượng tiền bằng giá khởi điểm và bị cấm tham gia đấu giá vĩnh viễn
	5. nếu sau 1 ngày kể từ thời điểm phiên đấu giá diễn ra, phiên đó sẽ ngừng hiển thị trên sàn, nhưng thông tin về phiên vẫn sẽ hiện trên tài khoản admin
	6. ```
	   3.2.5 Bid History Visualization – Realtime Price Curve
		• Hiển thị biểu đồ đường (line chart) giá đấu cao nhất theo thời gian thực
		• Trục X: Thời gian (timestamp)
		• Trục Y: Giá đấu hiện tại
		• Mỗi bid hợp lệ → biểu đồ tự động cập nhật (không cần refresh)
	   ```
6. chức năng seller:
	1. chọn mục bán
	2. chọn ảnh
	3. viết thông tin sản phẩm
	4. tick cam kết với nhà thầu
	5. chọn giá sàn 
	6. chọn bước giá
	7. chọn thời gian :
		1. không quá 30 phút cho 1 phiên
		2. niêm yết trên sàn trước 1 ngày
		3. trong 1 phút cuối nếu có người đấu giá thêm thì gia hạn 5 phút
		4. gia hạn tối đa 3 lần
	8. bấm nút đăng, đến ngày đấu giá phiên sẽ tự diễn ra
	9. trong phiên nếu người bán hài lòng với bất kì mức giá nào thì có thể kết thúc phiên luôn
7. Lỗi , ngoại lệ:
	1. đặt giá thấp hơn hiện tại
	2. đấu giá khi phiên đã đóng
	3. Lỗi dữ liệu(do mạng,do thông tin sai lệch, ....)
	4. Lỗi kết nối(do mạng,...)
	5. xử lí lỗi hai player đồng thời cùng đấu giá:
		1. so sánh thời gian
		2. so sánh mức ưu tiên
		3. nếu 2 người cùng dùng autobidding thì người đănh kí auto bidding sớm hơn sẽ được ưu tiên
	6. trong trường hợp hi hữu 1 phần tỷ tỷ hai người cùng đặt giá và thời điểm đặt giống nhau, thì nguwoif có id thấp hơn sẽ được ưu tiên
	7. không được phép xảy ra 2 người cùng thắng
	8. sử dụng websocket: khi có dữ liệu mới lập tức đẩy dữ liệu về máy client
		1. observer pattern
		2. socket /event-based communication
		3. thread-safe notify
	9. không dùng polling
	10. 
8. Giao diện :
	1. sử dụng javaFX
	2. màn hình vào phiên
	3. màn hình đăng nhập
	4. màn hình chính(nơi hiện các phiên đấu giá đang diễn ra và phiên đã kết thúc trong ngày)
	5. chi tiết sản phẩm
	6. màn hình đấu giá trực tiếp
	7. quản lí sản phẩm cho seller
	8. quản lí đấu giá cho admin
	9. quản lí hàng đã mua cho player





##  Hướng Dẫn:

![[Pasted image 20260330121524.png]]
![[Pasted image 20260330121536.png]]
![[Pasted image 20260330121551.png]]
![[Pasted image 20260330121613.png]]
