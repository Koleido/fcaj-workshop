---
title: "Dọn dẹp"
date: 2026-07-28
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Sau khi hoàn thành workshop và đã chụp đầy đủ các ảnh cần thiết cho báo cáo, việc **xóa toàn bộ tài nguyên** đã tạo ra là rất quan trọng. Nếu để các tài nguyên chạy mà không sử dụng sẽ tiếp tục phát sinh chi phí (ngay cả khi đang dùng Free Tier sau khi hết thời gian miễn phí).

Hãy thực hiện các bước dưới đây **theo đúng thứ tự** để dọn dẹp mọi thứ một cách an toàn.

---

### 1. Terminate EC2 Instance

**Bước 1:** Vào **EC2** Console → chọn **Instances**.

**Bước 2:** Chọn instance `HCMUT-Cinema-Backend-Server` → nhấn **Instance state** → **Terminate instance**.

**Bước 3:** Xác nhận việc terminate.

![Terminate EC2](/images/5-Workshop/5.6-Cleanup/5.6.1.jpg)

![Terminate EC2](/images/5-Workshop/5.6-Cleanup/5.6.2.jpg)

---

### 2. Xóa S3 Bucket

**Bước 1:** Vào **S3** Console.

**Bước 2:** Chọn bucket frontend của bạn → nhấn **Empty**. Xác nhận bằng cách gõ `permanently delete`.

**Bước 3:** Sau khi bucket đã trống, chọn lại bucket → nhấn **Delete**. Gõ tên bucket để xác nhận.

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.3.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.4.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.5.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.6.jpg)

---

### 3. Xóa RDS Database

**Bước 1:** Vào **RDS** Console → **Databases**.

**Bước 2:** Chọn `hcmut-cinema-db` → nhấn **Actions** → **Delete**.

**Bước 3:** 
- Bỏ tick “Create final snapshot” (trừ khi bạn muốn giữ lại dữ liệu).
- Bỏ tick "Retain automated backups".
- Tick vào ô "I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available."
- Gõ `delete me` để xác nhận.
- Nhấn **Delete**.

> **Lưu ý:** Việc xóa database có thể mất vài phút để hoàn tất.

![Terminate RDS Database](/images/5-Workshop/5.6-Cleanup/5.6.7.jpg)

![Terminate RDS Database](/images/5-Workshop/5.6-Cleanup/5.6.8.jpg)

---

### 4. Xóa DynamoDB Table

**Bước 1:** Vào **DynamoDB** Console → **Tables**.

**Bước 2:** Chọn bảng `HCMUTCinema_SeatLocks` → nhấn **Delete**.

**Bước 3:** Xác nhận bằng cách gõ `delete`.

![Terminate DynamoDB Table](/images/5-Workshop/5.6-Cleanup/5.6.9.jpg)

![Terminate DynamoDB Table](/images/5-Workshop/5.6-Cleanup/5.6.10.jpg)

---

### 5. Xóa SES Email Identity

**Bước 1:** Vào **Amazon SES** Console → **Identities**.

**Bước 2:** Chọn địa chỉ email đã xác minh → nhấn **Delete**.

**Bước 3:** Xác nhận việc xóa.

![Terminate SES](/images/5-Workshop/5.6-Cleanup/5.6.11.jpg)

![Terminate SES](/images/5-Workshop/5.6-Cleanup/5.6.12.jpg)

---

### 6. Xóa IAM User & Access Key

**Bước 1:** Vào **IAM** Console → **Users**.

**Bước 2:** Nhấn vào user `hcmut-cinema-backend`.

**Bước 3:** Chuyển sang tab **Security credentials** → deactivate toàn bộ **Access keys**.

**Bước 4:** Quay lại và nhấn **Delete** user. Xác nhận bằng cách gõ tên username.

![Terminate IAM](/images/5-Workshop/5.6-Cleanup/5.6.13.jpg)

![Terminate IAM](/images/5-Workshop/5.6-Cleanup/5.6.14.jpg)