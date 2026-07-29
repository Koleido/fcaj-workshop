---
title: "Thiết lập Database (RDS & DynamoDB)"
date: 2026-07-28
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

# Thiết lập Cơ sở dữ liệu (Amazon RDS & DynamoDB)

Trong kiến trúc của HCMUT Cinema, phần dữ liệu được chia làm 2 kho lưu trữ chuyên biệt để tối ưu hóa hiệu suất:
1. **Amazon RDS (PostgreSQL):** Lưu trữ các dữ liệu có tính quan hệ chặt chẽ, cần đảm bảo tính toàn vẹn (ACID) như thông tin Khách hàng, Phim, Rạp, và các Vé đã thanh toán.
2. **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL tốc độ siêu cao, được dùng riêng cho tính năng "Khóa ghế" (Seat Locking) với cơ chế tự hủy (TTL) sau 5 phút.

---

## Phần 1: Triển khai Amazon RDS (PostgreSQL)

![Sơ đồ RDS](/images/5-Workshop/5.4-Database-RDS-DynamoDB/diagram_rds.png)

### Khởi tạo cơ sở dữ liệu

**Bước 1:** Truy cập vào **AWS Console**, tìm kiếm dịch vụ **RDS** và nhấn vào nút **Create database**.

![Bước 1](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.1.png)

**Bước 2:** Chọn phương thức **Standard create** (Thiết lập đầy đủ). Ở mục Engine options, chọn **PostgreSQL**.

![Bước 2](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.2.png)

**Bước 3:** Kéo xuống mục **Templates**, bắt buộc phải chọn **Free tier** để tối ưu hóa chi phí dự án.

![Bước 3](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.3.png)

**Bước 4:** Ở phần **Settings**, cấu hình các thông số sau:
- DB instance identifier: `hcmut-cinema-db`
- Master username: `postgres`
- Master password: Khai báo mật khẩu quản trị.

![Bước 4](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.4.png)

**Bước 5:** Tại mục **Connectivity**, cấu hình **Public access** thành **Yes**. Việc này cho phép máy tính cá nhân (Local) và máy chủ EC2 có thể kết nối thẳng vào Database.

![Bước 5](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.5.png)

**Bước 6:** Cuộn xuống dưới cùng và nhấn **Create database**. Cần chờ khoảng 5-10 phút để AWS cấp phát tài nguyên. Khi trạng thái báo *Available*, DB đã sẵn sàng!

![Bước 6](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.6.png)

### Nạp dữ liệu mồi (Seed Data)

**Bước 7:** Mở Terminal ở máy tính, chuyển vào thư mục `backend` và chạy script `node import_db.js`. Script này sẽ tự động kết nối lên RDS, tạo các bảng (Phim, Rạp, Vé...) và đẩy dữ liệu mẫu vào.

```bash
cd backend
node import_db.js
```
![Bước 7](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.7.png)

---

## Phần 2: Triển khai Amazon DynamoDB (NoSQL)

Hệ thống đặt vé yêu cầu một cơ chế giữ ghế (Lock) trong vòng 5 phút khi người dùng đang thao tác thanh toán. DynamoDB với tính năng **Time-To-Live (TTL)** là giải pháp hoàn hảo, giúp hệ thống tự động dọn dẹp các ghế "hết hạn chờ" mà không cần phải viết code vòng lặp quét liên tục gây nặng máy chủ.

![Sơ đồ DynamoDB](/images/5-Workshop/5.4-Database-RDS-DynamoDB/diagram_dynamo.png)

### Khởi tạo Bảng lưu trạng thái ghế

**Bước 1:** Truy cập dịch vụ **DynamoDB** trên AWS Console, nhấn **Create table**.

![Bước 1](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.8.png)

**Bước 2:** Ở ô **Table name**, nhập đúng tên bảng mà Backend quy định là `HCMUTCinema_SeatLocks`.

![Bước 2](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.9.png)

**Bước 3:** Ở ô **Partition key**, nhập `LockID` và chọn kiểu dữ liệu là **String**. Sau đó kéo xuống dưới cùng và nhấn **Create table**.

![Bước 3](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.10.png)

### Kích hoạt cơ chế tự hủy ghế (TTL)

**Bước 4:** Sau khi tạo bảng xong, click vào tên bảng. Chuyển sang tab **Additional settings**, cuộn xuống mục **Time to Live (TTL)** và nhấn Enable. 
Khai báo thuộc tính cấu hình TTL là `ExpirationTime`. Từ giờ trở đi, bất cứ chiếc ghế nào bị khóa đẩy vào DynamoDB, đúng 5 phút sau AWS sẽ tự động xóa sạch để nhả ghế cho người khác!

![Bước 4](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.11.png)

Hệ thống lưu trữ dữ liệu của chúng ta đã được triển khai hoàn tất!
