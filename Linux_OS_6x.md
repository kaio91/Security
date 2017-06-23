## 1.	Cài đặt và cập nhật bản vá cho hệ điều hành 
-	Cài đặt phiên bản mới nhất và cập nhật bản vá của hệ điều hành, không mắc các lỗ hổng bảo mật đã được công bố.
-	Kiểm tra phiên bản kernel với lệnh: “uname -a”. Yêu cầu kernel phải được nâng cấp lên phiên bản mới nhất tính tới thời điểm cài đặt.
-	Trong trường hợp cập nhật bản vá, nâng cấp kernel.
-	Trường hợp có kết nối Internet, thực hiện chạy lệnh sau để nâng cấp kernel.
>   yum upgrade kernel 
-	Trường hợp không có kết nối Internet, thực hiện chạy lệnh sau để tiến hành cài đặt:
-   <b> Bước 1: </b> Cài đặt 1 máy ảo với hệ điều hành tương ứng với hệ điều hành cần nâng cấp kernel. *Chú ý máy ảo này phải có kết nối Internet.*
-	<b> Bước 2: </b> Download toàn bộ các gói cần cài đặt nâng cấp kernel mà distro cung cấp về máy ảo
>    mkdir /opt/upgrade <br>
>    yum install yum-downloadonly -y <br>
>    yum install kernel -y --downloadonly --downloaddir=/opt/upgrade
-	<b> Bước 3: </b>Tải toàn bộ gói .rpm trong thư mục */opt/upgrade* của máy tính lên máy chủ và thực hiện cài như cài gói .rpm như thông thường.
-   <font color="red">*Lưu ý: Sau khi cài đặt xong cần khởi động lại máy chủ để hệ điều hành nhận kernel mới.*</font>
## 2.	Xóa hoặc vô hiệu hóa các dịch vụ, ứng dụng, giao thức mạng không cần thiết.
-   Trong thực tế, một server (máy chủ) trong hệ thống sẽ đảm nhiệm một chức năng riêng biệt. 
-   Khi cài đặt hệ điều hành cho server, cần xóa hoặc disable tất cả các dịch vụ, ứng dụng, giao thức không cần thiết.
-   <b> Bước 1: </b> Liệt kê toàn bộ các gói tin với câu lệnh “yum list”, tìm kiếm các gói tin không cần thiết và thực hiện gỡ bỏ bằng cách sau:
> yum remove <package-name>
-	<b> Bước 2: </b> Liệt kê các dịch vụ đang được chạy ở runlevel 3. Tìm kiếm và xoá bỏ các dịch vụ không cần thiết.
> chkconfig --list | grep '3:on'
- Tìm kiếm các dịch vụ chạy ở mức độ 3 không sử dụng, tiến hành tắt chúng bằng cách:
> chkconfig "serviceName" off
-	Bước 3: Kiểm tra các cổng đang mở trên hệ thống và các dịch vụ đang lắng nghe trên các cổng đó, tiến hành tắt các dịch vụ không cần thiết:
> netstat –tulpn </br>
-  Kiểm tra danh sách các dịch vụ không cần thiết, tiến hành tắt các dịch vụ:
> service <serviceName> stop
##3.	Thiết lập chính sách tài khoản
-	Xóa hoặc vô hiệu hóa các toàn khoản không sử dụng trên hệ thống.
- <b> Bước 1: </b> Để tìm những tài khoản đang hoạt động trên hệ thống, ta sử dụng lệnh sau:
> cat /etc/passwd | grep /*sh$ | awk -F: '{print $1}'
- <b> Bước 2: </b> Kiểm tra xem trong danh sách tài khoản hiện ra xem tài khoản nào không sử dụng. Thực hiện xoá các tài khoản đó bằng lệnh sau:
> userdel –r username
=> Ví dụ: Trong danh sách có tài khoản user1 không sử dụng
> userdel –r user1
-	Cấu hình chính sách mật khẩu cho tài khoản:
-	Độ dài tối thiểu của mật khẩu phải lớn hơn hoặc bằng 8 ký tự.
- <b> Bước 1: </b> Mở tập tin /etc/pam.d/system-auth
> vi /etc/pam.d/system-auth
- <b> Bước 2:</b> Thêm hoặc cập nhật cấu hình sau trong tập tin cấu hình của PAM:
> password    requisite     pam_cracklib.so [các option trước đó] minlen=8
- <b> Bước 3: </b> Lưu lại tập tin cấu hình.
-	Mật khẩu phải chứa ký tự viết hoa, viết thường, chữ số, ký tự đặc biệt:
- <b> Bước 1:</b> Mở tập tin /etc/pam.d/system-auth
> vi /etc/pam.d/system-auth
- <b> Bước 2: </b> Thêm hoặc cập nhật cấu hình sau trong tập tin cấu hình của PAM:
password    requisite     pam_cracklib.so [các option trước đó] ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
Bước 3: Lưu lại tập tin cấu hình.
o	Thời gian bắt buộc phải thay đổi mật khẩu: Thiết lập giá trị 90 ngày với hệ thống public và 180 ngày với hệ thống nội bộ. Ví dụ thiết lập với hệ thống public như sau:
Mở tập tin /etc/login.defs, thay đổi tuỳ chọn PASS_MAX_DAYS, ví dụ:
PASS_MAX_DAYS 90
Với các tài khoản đã tồn tại, có thể sử dụng lệnh sau để thay đổi thời gian hết hạn mật khẩu:
#chage –M 90 username
Ví dụ, để thay đổi thời gian hết hạn mật khẩu cho tài khoản user1:
#chage –M 90 user1
o	Giới hạn mật khẩu mới không được trùng với mật khẩu gần nhất: Thiết lập giá trị là 2 với hệ thống nội bộ và 5 với hệ thống public. Ví dụ thiết lập cho hệ thống nội bộ như sau:
Bước 1: Mở tập tin /etc/pam.d/system-auth
#vi /etc/pam.d/system-auth
Bước 2: Thêm hoặc cập nhật cấu hình thuộc tính remember của tuỳ chọn password sufficient trong tập tin cấu hình của PAM:
password    sufficient    pam_unix.so [các option trước đó] remember=2
Bước 3: Lưu lại tập tin cấu hình.
o	Mã hóa mật khẩu sử dụng thuật toán mã hóa an toàn.
Bước 1: Kiểm tra thuật toán mã hoá sử dụng:
#authconfig --test | grep hashing
 password hashing algorithm is sha512
Bước 2: Nếu thuật toán mã hoá sử dụng không phải là sha512, thực hiện sửa đổi và kiểm tra lại:
#authconfig --passalgo=sha512 --update
#authconfig --test | grep hashing
 password hashing algorithm is sha512
o	Mật khẩu không dễ đoán: không bao gồm thông tin cá nhân (tài khoản, họ tên, số điện thoại, số chứng minh thư, ngày tháng năm sinh, mã nhân viên), không được bao gồm các thông tin thông dụng (tên tổ, nhóm, phòng ban, đơn vị, ngày tháng năm thiết lập mật khẩu), không được bao gồm các chuỗi ký tự liên tiếp (từ 4 ký tự trở lên), chuỗi các ký tự giống nhau (từ 4 ký tự trở lên).
Ví dụ: các mật khẩu không được phép: 123456a@, 1234567a@, 12345678a@, qwerty@A, 11112222a@, Hungnh17a@, …
