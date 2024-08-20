# Vim
## Các vấn đề vim có thể làm ở chế độ lệnh
- Chèn thêm ký tự vào cuối dòng: `%s/$/;/` -> chèn ký tự `;` vào cuối mỗi dòng. Thay thế `$` bằng `^` nếu muốn chèn vào đầu dòng. 
- Xóa ký tự cuối của mỗi dòng: `%s/.$//` với `.` đại diện cho 1 ký tự cuối dòng. Giả sử `%/...$//` thì `.` đại diện cho 3 ký tự cuối cùng.
- Xóa các dòng trống: `:g/^$/d`
- Xóa các dòng chứa số : `:g/\d/d` - với `:g/` là lệnh để áp dụng hành động trên các dòng khớp với mẫu tìm kiếm. `\d` là mẫu tìm kiếm biểu diễn bất kỳ chữ số nào. `d` là xóa dòng. Có thể thay g bằng v để xóa các dòng không chứa chữ số.
- Xóa ký tự cuối của mỗi dòng với số lượng lớn `%s/.\{10}$//` -> xóa 10 ký tự cuối dòng
- Xóa ký tự đầu của mỗi dòng với số lượng lớn `%s/^.\{10}//` -> Xóa 10 ký tự đầu dòng
- Xóa các dòng có chứa mẫu cụ thể: `:g/pattern/d`
- Chèn sau 1 chuỗi ký tự cụ thể: `%s/example/example;/` -> Tìm kiếm `example` và thêm `;` vào cuối từ example trên mỗi dòng
- Tìm kiếm chuỗi ký tự: `/string` -> tìm kiếm từ vị trí con trỏ xuống dưới
- `>` và `<` : sử dụng để thụt lề vào ra - Kết hợp với việc sử dụng `v` để bôi đen 
- Tạo 1 shell script bằng vim: `vim script.sh` và chạy script bằng `./script.sh`. Lưu ý: sau khi tạo file.sh cần cấp quyền thực thi cho nó.
- Lưu phiên làm việc: `:mksesstion session.vim`
- Khôi phục phiên làm việc đã lưu: `:source sesstion.vim`
- Chạy 1 lệnh shell và xem kết quả: `!ls`
- 
# Package và cài đặt phần mềm
## Các khái niệm - Vị trí của chúng trong hê thống
- Repo: Kho lưu trữ các URL của package
- `cat /etc/apt/sources.list` - xem nội dung 
- `ls /etc/apt/sources.list.d/` - xem các kho lưu trữ bổ sung trong thư mục
- Depend: Phụ thuộc của 1 phần mềm vào 1 phần mềm khác khi cài đặt
- `apt show [software]` -> hiển thị thông tin và các phụ thuộc cần để cài đặt phần mềm này hoặc sử dụng 'apt-cache depend [software]`.
- Nội dung trong tệp `package.gz` chứa các thông tin trong đó có phụ thuộc - Nó được lưu trữ trong CSDL gói cục bộ `/var/lib/apt/lists`
- Binary: Là tệp nhị phân đã đc biên dịch sẵn khi cài đặt phần mềm. Các gói nhị phân(.deb) được lưu trữ trên các máy chủ kho lưu trữ. Khi update hệ thống sẽ tải danh sách các gói từ kho lưu trữ. 
- Các file này được lưu trữ tại:  `/var/cache/apt/archives/`
- Các file binary từ các Repo được tải xuống và cài đặt trên hệ thống khi
- Cài đặt gói mới: `sudo apt install [package_name]`
- Nâng cấp các gói đã cài đặt: ` Sudo apt upgrade`
- Cài đặt gói thủ công: `sudo dpkg -i package_name.deb`
- `cd /var/cache/apt/archives` - Nơi lưu trữ file.deb do ng dùng tải về.
- `/etc/apt/sources.list` - Trong các kho lưu trữ
## Các cách cài đặt phần mềm
- Trình quản lý dpkg, apt
Sử dụng file Binary Package
Sử dụng Source Code - Mã nguồn mở
## Lệnh apt update và apt upgrade
- `apt update`: Cập nhật danh sách các gói có sẵn từ các kho lưu trữ. Không cài đặt or nâng cấp bất kỳ gói nào. chỉ cập nhật CSDL của `apt` về các gói có sẵn.
  - B1: Kết nối tới các kho lưu trữ được liệt kê trong các tệp cấu hình `/etc/apt/sources.list` và `/etc/apt/sources.líst.d`
  - B2: Tải danh sách các gói mới: Hệ thống tải các tệp chứa thông tin về các gói phần mềm có sẵn từ các kho lưu trữ chứa thông tin về tên gói phiên bản, kích thước, phụ thuộc và các chi tiết khác. Thường là `packages.gz` chứa các thông tin: Tên gói - Phiên bản gói - Mô tả ngắn gọn(mục đích và chức năng của gói) -      - Phụ thuộc(danh sách các gói khác mà gói này phụ thuộc vào) - Kiến trúc - URL của tệp gói.
  - Tức là các thông tin tải về từ kho lưu trữ baogom danh sách các gói phần mềm có sẵn, thông tin về bản phát hành, thông tin bảo mật, nhật ký thay đổi, và các tệp mã nguồn hoặc bản dịch.
- B3: Cập nhật cơ sở dữ liệu của gói cục bộ, thông tin tải về từ các kho lưu trữ sẽ được lưu trữ trong CSDL gói cục bộ trên hệ thống. CSDL này cho phép công cụ quản lý gói apt biết được các gói nào có sẵn, các phiên bản nào mới nhất, các phụ thuộc cần thiết cho việc cài đặt hoặc nâng cấp phần mềm.
  - Vị trí lưu trữ CSDL gói cục bộ: `/var/lib/apt/lists/` thư mục này chứa các tệp đại diện cho danh sách các gói có sẵn từ các kho lưu trữ mà bạn đã cấu hình trong hệ thống.
- B4: Thông báo về gói mới hoặc phiên bản mới, nếu có phiên bản mới của các gói phân mềm có sẵn trong kho lưu trữ, `hệ thống sẽ cập nhật thông tin này`. Lệnh `apt update` cũng kiểm tra các kho lưu trữ để phát hiện bất kỳ bản cập nhật họăc sửa lỗi nào đối với các gói đã cài đặt
- B5: Nếu có vấn đề với các kho lưu trữ(như không thể kết nối, kho lưu trữ không còn hoạt động, hoặc các lỗi cấu hình), lệnh apt update sẽ thông báo về những vấn đề này.
- Mục đích sử dụng:
- Được sử dụng để đảm bảo rằng hệ thống luôn có thông tin mới nhất về các gói và bản cập nhật có sẵn từ Repo
- Hỗ trợ các lệnh quản lý gói khác như `apt install` và `apt upgrade` hoạt động 1 cách chính xác nhất.
- Giúp hệ thống có thông tin chính xác về các gói hiện có và các phiên bản mới của chúng
- Cải thiện bảo mật và tính ổn định vì các bản cập nhật thường có các bản vá bảo mật quan trọng
-`apt upgrade`: Nâng cấp các gói phần mềm đã cài đặt lên phiên bản mới nhất. Chỉ nâng cấp các gói đã có sẵn mà không thêm hay gỡ bỏ các gói khác.
- `/var/lib/dpkg/status`: Tệp chứa thông tin về tất cả các gói đã được cài đặt trên hệ thống gồm tên, phiên bản, trạng thái, mô tả, phụ thuộc, .... Là nơi so sánh phiên bản với các gói mới nhất được cập nhật từ repo và lưu trữ tại `/var/lib/apt/lists/`
## Startup Scripts
### Sử dụng systemd
### Sử dụng Crontab
### Sử dụng  etc/rc.local
## FS ext4 và xfs, partition và các lệnh mkfs, fdisk, fsck, mount, umount





