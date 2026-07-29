---
title: "Triển khai Frontend (Amazon S3)"
date: 2026-07-28
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# Triển khai Giao diện Web lên Amazon S3

Trong phần thực hành này, chúng ta sẽ đẩy toàn bộ source code giao diện tĩnh (HTML, CSS, JS) của **HCMUT Cinema** lên đám mây thông qua dịch vụ lưu trữ **Amazon S3**. 

Amazon S3 có một tính năng tuyệt vời là **Static Website Hosting**, cho phép biến một Bucket lưu trữ thông thường thành một Web Server tĩnh với chi phí cực kỳ rẻ và khả năng chịu tải (scalability) gần như vô hạn mà không cần phải cấu hình máy chủ.

Dưới đây là sơ đồ luồng hoạt động của Frontend tĩnh trên S3:

![Sơ đồ S3 Frontend](/images/5-Workshop/5.3-Deploy-S3-Frontend/diagram_s3.png)

---

### Các bước thực hiện chi tiết

**Bước 1:** Truy cập vào giao diện quản trị (Console) của Amazon S3. Tại giao diện chính, nhấn vào nút **Create bucket** (Tạo bucket).

![Bước 1](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.1.png)

**Bước 2:** Đặt tên cho Bucket của bạn. Lưu ý rằng tên Bucket phải là **duy nhất trên toàn bộ thế giới** (Global Unique). Chọn Region (Khu vực) là `ap-southeast-1` (Singapore) để cho tốc độ truy cập từ Việt Nam nhanh nhất.

![Bước 2](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.2.png)

**Bước 3:** Tại phần **Block Public Access settings for this bucket**, bạn cần **bỏ dấu tick** ở ô "Block all public access". Việc này cho phép mọi người trên Internet có thể truy cập và xem được trang web của bạn.

![Bước 3](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.3.png)

**Bước 4:** AWS sẽ hiển thị một cảnh báo xác nhận việc mở quyền truy cập công khai. Bạn hãy tick vào ô **I acknowledge...** để xác nhận mình hiểu rõ rủi ro này.

![Bước 4](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.4.png)

**Bước 5:** Cuộn xuống cuối trang và nhấn nút **Create bucket** để hoàn tất việc khởi tạo.

![Bước 5](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.5.png)

**Bước 6:** Sau khi tạo xong, click vào tên Bucket vừa tạo. Chuyển sang tab **Properties** (Thuộc tính) và cuộn xuống mục dưới cùng là **Static website hosting**. Nhấn **Edit** (Chỉnh sửa).

![Bước 6](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.6.png)

**Bước 7:** Bật (Enable) tính năng **Static website hosting**. Ở ô **Index document**, bạn nhập vào `index.html` (đây là trang chủ mặc định sẽ tải khi người dùng truy cập link website). Nhấn Save changes.

![Bước 7](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.7.png)

**Bước 8:** Chuyển sang tab **Permissions** (Quyền). Kéo xuống mục **Bucket policy** và nhấn Edit. Bạn dán đoạn mã JSON (Policy) cấp quyền `s3:GetObject` cho mọi người (`"Principal": "*"`). Điều này biến các file code của bạn thành các trang web có thể đọc được công khai.

![Bước 8](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.8.png)

**Bước 9:** Cuối cùng, quay lại tab **Objects**, nhấn nút **Upload** và tải lên toàn bộ các file giao diện (HTML, CSS, JS, Images) của thư mục `s3_frontend`. Sau khi upload xong, quay lại tab Properties, copy đường link website ở phần Static website hosting và dán vào trình duyệt để chiêm ngưỡng thành quả!

![Bước 9](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.9.png)

---
> **💡 Mẹo:** Bất cứ khi nào bạn chỉnh sửa file Code (ví dụ: đổi IP của API trong file `app.js`), bạn chỉ cần Upload đè file đó lên mục Objects của S3 Bucket này là giao diện sẽ lập tức được cập nhật!