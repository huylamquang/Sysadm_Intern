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
## Startup Scripts: Tự động chạy tác vụ khi hệ thống vừa khởi động
### Sử dụng Crontab
Các bước thiết lập:
- B1: tạo file.sh

  ![image](https://github.com/user-attachments/assets/028353f3-24f0-4237-8c19-6af49301de19)
- B2: cấu hình file `crontab -e`

  ![image](https://github.com/user-attachments/assets/1c0bb6e4-4ab2-45e9-bebf-13e02a6d664b)
- B3: Thực hiện reboot và kiểm tra kết quả

  ![image](https://github.com/user-attachments/assets/8708f7ab-a6f5-417f-9b6d-c5b8e9cc6116)
### Sử dụng  etc/rc.local
Các bước thiết lập:
- Bước 1: tạo file huy.sh

  ![image](https://github.com/user-attachments/assets/9afadb5a-2f3b-4a17-8fa0-b1b20e3dcb1f)

- Bước 2: cấu hinh file /etc/rc.local

  ![image](https://github.com/user-attachments/assets/f09c55c0-04dc-449c-ae97-735e2dd09333)

- Bước 3: kiểm tra cấu hình

  ![image](https://github.com/user-attachments/assets/306f8486-d8a4-4193-8d10-89ed0796209d)

## FS ext4 và xfs, partition và các lệnh mkfs, fdisk, fsck, mount, umount
FS hệ thống tệp tin là 1 dạng cấu trúc dữ liệu được sử dung bởi HĐH để tổ chức và quản lý tệp tin trên 1 thiết bị ổ cứng, ổ đĩa, thẻ nhớ.
### Những lưu ý khi chọn FS ext4 và xfs
- Hiệu suất: Thông thường với những ứng dụng, dịch vụ phù hợp thì cả 2 FS đều đạt hiệu suất rất tốt.
- Khả năng mở rộng và kích thước: Xfs nhỉnh hơn về khả năng mở rộng và kích thước
- Tính toàn vẹn và phục hồi dữ liệu: Xfs nhỉnh hơn về dung lượng lưu trữ, khả năng mở rộng nhưng khả năng phục hồi không tốt bằng ext4 vì kích thước lớn.
- Tính năng đặc biệt: Mỗi 1 fs đều có các tính năng riêng biệt phù hợp cho các mục đích dịch vụ khác nhau. 
- Sử dụng thực tế: Tùy thuộc vào phần cứng, phần mềm ổ cứng thì sẽ cho ra những kết quả khác nhau.
- Khả năng tương thích và hỗ trợ: Khả năn tương thích và hỗ trợ thì ext4 nhỉnh hơn và có cộng đồng sử dụng nhiều hơn.
Nhìn chung, `EXT4` đề cao tính ổn định và phổ biến, hiệu suất và tốc độ truy cập dữ liệu tốt, tối ưu cho các tập tin có kích thước trung bình đến lớn. Tương thích tốt với tất cả bản phân phối. Nhưng giới hạn kích thước của 1 phân vùng ổ - Không tối ưu cho SSD bằng các FS khác.
`XFS` Hiệu suất cao với các tệp lớn, phù hợp cho máy chủ và ứng lưu trữ dữ liệu. Khả năng mở rộng linh hoạt: có thể mở rộng phân vùng mà không cần umount, hữu ích khi cần tăng dung lượng lưu trữ nhanh chóng. Độ ổn định cao nhờ cơ chế kiểm tra lỗi mạnh mẽ và khả năn phục hồi. Hỗ trợ dung lượng không giới hạn. Nhưng ít phổ biến và yêu cầu kỹ thuật trong việc cấu hình khi tối ưu hệ thống.
- Về mount: khi thực hiện mount lên 1 thư mục chứa dữ liệu thì dữ liệu trong thư mục được mount bị che khuất, không bị xóa hay ghi đè mà chỉ tạm thời không thể truy cập qua đường dẫn đó. Dữ liệu cũ sẽ xuất hiện trở lại khi ta unmount pv ra khỏi thư mục và dữ liệu vẫn đc lưu trên đĩa. Lưu ý trong việc tự động mount sử dụng fstab: Cần kiểm tra phân vùng, đảm bảo phân vùng không sử dụng( tức là không có 1 tiến trình nào sử dụng phân vùng trước khi umount or format). Đặc biệt là việc kiểm tra /etc/fstab và check cấu hình file /etc/fstab đã đúng chưa bằng lệnh 'sudo findmnt --verify --fstab` vì nếu không kiểm tra và cấu hình sai sẽ ảnh hưởng đến quá trình khởi động hệ thống.(Lệnh này sẽ cho ra các cảnh báo và lỗi cụ thể)
  
- Về fsck: Sử dụng để kiểm tra và sửa chữa các lỗi trên hệ thống tệp tin. Giúp duy trì tính toán toàn vẹn của hệ thống tệp tin. Yêu cầu khi sử dụng fsck: Cần có quyền root - Bắt buộc sao lưu dữ liệu trước khi thực hiện và phải umount phân vùng để đảm bảo an toan dữ liệu vì nó gây ra các lỗi hoặc hỏng dữ liệu trong quá trình kiểm tra và sửa chữa.
- Về mkfs: Khi tạo mới hoặc sửa hệ thống tệp tin trên 1 phân vùng đã có hệ thống tệp tin và dữ liệu trước đó, hệ thống tệp tin cũ sẽ bị ghi đè và toàn bộ dữ liệu trên phân vùng đó sẽ bị mất. VD chuyển từ Ext4 sang Xfs và ngược lại. Các lưu ý khi thực hiện:
  1. Cần xóa toàn bộ dữ liệu trên phân vùng để thay đổi FS.(Không thể thay đổi trực tiếp)
  2. Sao lưu dữ liệu: Quá trình chuyển đổi cần định dạng lại phân vùng, điều này sẽ xóa toàn bộ dữ liệu nên cần sao lưu dữ liệu có trên pv. Lệnh sao lưu `rsync -avr /URL/dir_mnt /URL/dir_backup`.
  3. Umount phân vùng trước khi sử dụng fsck để ktr phân vùng để đảm bảo không có lỗi trước khi chuyển đổi.
  4. Định dạng phân vùng với XFS: `sudo mkfs.xfs /dev/sda1`
  5. Kiểm tra và mount pv mới: `mount /dev/sda1 /DATA1`
  6. Khôi phục dữ liệu từ bản sao lưu: `rsync -avr /URL/dir_backup /DATA1


  




