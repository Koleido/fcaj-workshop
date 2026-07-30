---
title: "Triển khai Backend (EC2 & SES)"
date: 2026-07-28
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

# Triển khai Backend API & Dịch vụ Email (EC2 & SES)

Trong phần này, chúng ta sẽ tiến hành 2 tác vụ lớn:
1. Khởi tạo, cấu hình tường lửa và đẩy mã nguồn Node.js lên máy chủ ảo Amazon EC2.
2. Cấu hình dịch vụ Amazon SES (Simple Email Service) để hệ thống tự động gửi mã OTP xác thực và vé điện tử qua email.

---

## Phần 1: Triển khai máy chủ Amazon EC2

Amazon EC2 đóng vai trò là trái tim của hệ thống Backend, cung cấp sức mạnh tính toán để xử lý các logic phức tạp như tìm vé, tính tiền và duy trì kết nối WebSocket thời gian thực (Socket.IO).

![Sơ đồ EC2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/diagram_ec2.png)

### Khởi tạo máy chủ ảo (Instance)

**Bước 1:** Từ trang chủ **AWS Console**, tìm kiếm dịch vụ **EC2**. Tại bảng điều khiển EC2, nhấn nút **Launch instance** (Khởi chạy phiên bản) màu cam.

![Bước 1](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.1.png)

**Bước 2:** Đặt tên cho máy chủ của bạn (ví dụ: `HCMUT-Cinema-Backend-Server`). Ở phần **Application and OS Images**, chọn hệ điều hành **Ubuntu Server 22.04 LTS** (đây là hệ điều hành ổn định, rất phổ biến cho Node.js và nằm trong gói Free Tier).

![Bước 2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.2.png)

**Bước 3:** Kéo xuống mục **Instance type**, giữ nguyên lựa chọn `t2.micro`. Ở mục **Key pair (login)**, bạn tạo mới hoặc chọn một khóa bảo mật `.pem` đã có sẵn. File `.pem` này chính là "chìa khóa" duy nhất để bạn có thể truy cập (SSH) vào máy chủ sau này.

![Bước 3](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.3.png)

**Bước 4:** Ở mục **Network settings**, nhấn nút **Edit**. Bạn cần mở thêm cổng mạng để Backend có thể hoạt động:
* Giữ nguyên Rule mặc định: **SSH (Cổng 22)** để có thể kết nối vào máy.
* Nhấn *Add security group rule*, chọn Type là **Custom TCP**, Port là **3000** (cổng mà Node.js đang lắng nghe), và Source type là **Anywhere (0.0.0.0/0)** để cho phép giao diện Frontend truy cập được vào API.

![Bước 4](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.4.png)

**Bước 5:** Ở bảng Summary bên góc phải, kiểm tra lại toàn bộ cấu hình. Sau khi chắc chắn, bạn hãy nhấn nút **Launch instance** để AWS bắt đầu tạo máy chủ. Chờ khoảng 1 phút, máy chủ sẽ ở trạng thái *Running*.

![Bước 5](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.5.png)

### Đẩy mã nguồn và khởi chạy Node.js

**Bước 6:** Mở Terminal (PowerShell hoặc Git Bash) trên máy tính cá nhân. Chúng ta sẽ sử dụng lệnh `scp` (Secure Copy Protocol) để sao chép toàn bộ thư mục chứa code Backend lên máy chủ EC2 (đừng quên thay địa chỉ IP bằng IP thật của máy chủ EC2 của bạn):

```bash
scp -i hcmutkey.pem -o StrictHostKeyChecking=no -r backend/routes backend/config backend/server.js backend/package.json backend/.env ubuntu@<IP_CUA_EC2>:/home/ubuntu/backend/
```

![Hình chụp Terminal đẩy code bằng SCP](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.6.png)

**Bước 7:** Dùng lệnh `ssh` để đăng nhập trực tiếp vào máy chủ EC2:

```bash
ssh -i hcmutkey.pem -o StrictHostKeyChecking=no ubuntu@<IP_CUA_EC2>
```

![Hình chụp Terminal kết nối SSH](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.7.png)

**Bước 8:** Từ bên trong máy chủ EC2, ta tiến hành cài đặt thư viện (`node_modules`) và dùng **PM2** để khởi chạy server. PM2 sẽ giúp Backend chạy ngầm 24/7 và tự động khởi động lại nếu máy chủ bị sập.

```bash
cd /home/ubuntu/backend
npm install
pm2 start server.js --name "hcmut-cinema"
```

![Hình chụp Terminal khởi chạy PM2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.8.png)

Hệ thống Backend API bây giờ đã chính thức online trên Cloud!

---

## Phần 2: Cấu hình Amazon SES (Simple Email Service)

Dịch vụ Amazon SES được lựa chọn nhờ khả năng gửi lượng lớn email một cách ổn định, chi phí siêu rẻ và khả năng tích hợp trực tiếp vào thư viện `aws-sdk` của Node.js cực kỳ mượt mà.

Vì tài khoản AWS của sinh viên đang ở chế độ thử nghiệm (Sandbox), chúng ta bắt buộc phải xác minh trước địa chỉ email muốn dùng để gửi và nhận thư. Dưới đây là các bước thực hiện:

**Bước 1:** Truy cập vào **AWS Console**, tìm kiếm dịch vụ **Amazon SES**. Tại thanh menu bên trái, chọn mục **Verified identities** (Danh tính đã xác minh). Sau đó, nhấn vào nút **Create identity** màu cam.

![Bước 1](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.1.png)

**Bước 2:** Chọn **Identity type** (Loại danh tính) là **Email address**. Nhập địa chỉ Email cá nhân của bạn vào ô **Email address**. Sau đó cuộn xuống dưới cùng và nhấn **Create identity**.

![Bước 2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.2.png)

**Bước 3:** Ngay lúc này, trạng thái của Email sẽ báo là *Unverified* (Chưa xác minh). AWS vừa tự động gửi một email chứa link xác nhận vào hộp thư của bạn. 

Hãy mở hộp thư (Inbox) ra, tìm email có tiêu đề "Amazon Web Services – Email Address Verification Request...", mở thư và nhấn vào đường link xác nhận. Quay lại trang SES và tải lại trang (F5), bạn sẽ thấy trạng thái đã chuyển thành **Verified** (Đã xác minh) màu xanh lá tuyệt đẹp. 

Bắt đầu từ thời điểm này, Backend Node.js đã chính thức có quyền sử dụng danh tính email này để gửi vé cho khách hàng!

![Bước 3](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.3.png)