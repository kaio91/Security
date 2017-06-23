# 1.	Cài đặt và cập nhật bản vá cho hệ điều hành 
-	Cài đặt phiên bản mới nhất và cập nhật bản vá của hệ điều hành, không mắc các lỗ hổng bảo mật đã được công bố.
-	Kiểm tra phiên bản kernel với lệnh: “uname -a”. Yêu cầu kernel phải được nâng cấp lên phiên bản mới nhất tính tới thời điểm cài đặt.
-	Trong trường hợp cập nhật bản vá, nâng cấp kernel:
-	Trường hợp có kết nối Internet, thực hiện chạy lệnh sau để nâng cấp kernel:
<h1>   yum upgrade kernel <h1>
-	Trường hợp không có kết nối Internet, thực hiện chạy lệnh sau để tiến hành cài đặt:
-   <b> Bước 1: </b> Cài đặt 1 máy ảo với hệ điều hành tương ứng với hệ điều hành cần nâng cấp kernel. Chú ý máy ảo này phải có kết nối Internet.
-	<b> Bước 2: </b> Download toàn bộ các gói cần cài đặt nâng cấp kernel mà distro cung cấp về máy ảo:
## mkdir /opt/upgrade
--    yum install yum-downloadonly -y
--    yum install kernel -y --downloadonly --downloaddir=/opt/upgrade
-	Bước 3: Tải toàn bộ gói .rpm trong thư mục /opt/upgrade của máy tính lên máy chủ và thực hiện cài như cài gói .rpm như thông thường.
-   Lưu ý: Sau khi cài đặt xong cần khởi động lại máy chủ để hệ điều hành nhận kernel mới.
-	Hệ điều hành phải được cập nhật các bản vá security đã được Tập đoàn cảnh báo.
# 2.	Xóa hoặc vô hiệu hóa các dịch vụ, ứng dụng, giao thức mạng không cần thiết 
- Trong thực tế, một server (máy chủ) trong hệ thống sẽ đảm nhiệm một chức năng riêng biệt. Khi cài đặt hệ điều hành cho server, cần xóa hoặc disable tất cả các dịch vụ, ứng dụng, giao thức không cần thiết.
  Bước 1: Liệt kê toàn bộ các gói tin với câu lệnh “yum list”, tìm kiếm các gói tin không cần thiết và thực hiện gỡ bỏ bằng cách sau:
