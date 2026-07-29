---

title: "Đề xuất dự án"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# HCMUT Cinema

# Hệ thống đặt vé rạp chiếu phim Cloud-Native trên Amazon Web Services

## 1. Tổng quan dự án

Dự án **HCMUT Cinema** là một hệ thống quản lý và đặt vé rạp chiếu phim theo kiến trúc **Cloud-Native** được phát triển trên nền tảng **Amazon Web Services (AWS)**. Dự án hướng đến việc tái cấu trúc một ứng dụng đặt vé rạp chiếu phim truyền thống theo mô hình monolithic thành một kiến trúc Cloud-Native hiện đại bằng cách tách biệt frontend và backend, tận dụng các **managed AWS services** và áp dụng các **best practices** về khả năng mở rộng, độ tin cậy và khả năng bảo trì hệ thống.

Một trong những mục tiêu quan trọng nhất của dự án là giải quyết các bài toán thực tế thường gặp trong hệ thống đặt vé trực tuyến, đặc biệt là **high concurrent access** và **seat booking conflicts (Race Condition)** trong các khung giờ cao điểm. Để giải quyết vấn đề này, hệ thống kết hợp nhiều dịch vụ AWS như **Amazon EC2**, **Amazon S3**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB** và **Amazon Simple Email Service (SES)**.

Hệ thống bao gồm hai phân hệ chính:

* **Phân hệ khách hàng (Customer Portal)**

  * Xem danh sách phim và lịch chiếu.
  * Chọn ghế với cơ chế **real-time seat locking**.
  * Mua vé bằng xác thực **OTP qua email**.
  * Nhận **vé điện tử chứa QR Code** sau khi thanh toán thành công.

* **Phân hệ quản trị (Administrator Portal)**

  * Quản lý phim, rạp, lịch chiếu và suất chiếu.
  * Tự động phát hiện và ngăn chặn xung đột lịch chiếu.
  * Theo dõi doanh thu và thống kê tỷ lệ lấp đầy ghế.
  * Quản lý thông tin khách hàng và hoạt động của rạp chiếu phim.

Việc triển khai hệ thống trên AWS giúp minh họa cách các công nghệ **Cloud-Native** có thể cải thiện hiệu năng, khả năng mở rộng và trải nghiệm người dùng, đồng thời giảm độ phức tạp trong vận hành hệ thống.

---

## 2. Bài toán đặt ra

### Thách thức hiện tại

Các hệ thống quản lý rạp chiếu phim truyền thống thường được triển khai theo kiến trúc **monolithic**, trong đó frontend, backend và cơ sở dữ liệu được liên kết chặt chẽ trong cùng một ứng dụng. Mặc dù mô hình này tương đối đơn giản ở giai đoạn đầu, nhưng khi số lượng người dùng tăng lên, hệ thống sẽ gặp nhiều khó khăn trong việc bảo trì và mở rộng.

Một trong những vấn đề nghiêm trọng nhất của hệ thống đặt vé trực tuyến là xử lý **high concurrent requests**. Trong các thời điểm cao điểm, nhiều khách hàng có thể cùng lúc đặt một ghế giống nhau. Nếu không có cơ chế đồng bộ phù hợp, tình huống này sẽ dẫn đến **Race Condition**, gây ra việc bán trùng ghế hoặc dữ liệu đặt vé không nhất quán.

Ngoài ra, các hệ thống truyền thống còn gặp nhiều hạn chế khác:

* Khả năng mở rộng hạn chế khi lưu lượng truy cập tăng cao.
* Frontend và backend phụ thuộc chặt chẽ vào nhau.
* Thời gian phản hồi chậm khi xử lý số lượng lớn yêu cầu đặt vé.
* Quy trình xác nhận vé và gửi email còn mang tính thủ công.
* Khó duy trì tính nhất quán dữ liệu giữa các thành phần của hệ thống.
* Hạn chế trong việc mở rộng tính năng trong tương lai.

Những vấn đề này ảnh hưởng trực tiếp đến trải nghiệm của khách hàng và làm tăng độ phức tạp trong vận hành đối với quản trị viên.

---

### Giải pháp đề xuất

Để khắc phục các thách thức trên, dự án đề xuất xây dựng một **Cloud-Native Cinema Booking System** hoàn toàn trên nền tảng Amazon Web Services.

Thay vì triển khai toàn bộ ứng dụng trên một máy chủ duy nhất, hệ thống sử dụng **decoupled architecture**, tách biệt frontend và backend nhằm tăng khả năng bảo trì và mở rộng.

Giải pháp bao gồm các dịch vụ AWS phối hợp với nhau như sau:

* **Amazon S3** lưu trữ và phân phối website frontend tĩnh.
* **Amazon EC2** chạy **RESTful API** được xây dựng bằng Node.js và Express.
* **Amazon RDS PostgreSQL** lưu trữ dữ liệu giao dịch như người dùng, phim, suất chiếu và vé.
* **Amazon DynamoDB** quản lý cơ chế khóa ghế tạm thời để ngăn chặn xung đột đặt vé.
* **Amazon SES** tự động gửi email xác thực OTP và vé điện tử cho khách hàng.

Điểm nổi bật của giải pháp là cơ chế **real-time seat locking** sử dụng **ConditionExpression** và **Time-to-Live (TTL)** của DynamoDB. Khi khách hàng chọn một ghế, ghế đó sẽ được khóa tạm thời trong 5 phút. Nếu quá trình thanh toán không hoàn tất trong khoảng thời gian này, DynamoDB sẽ tự động giải phóng khóa để khách hàng khác có thể đặt ghế.

Thiết kế này giúp giảm đáng kể tình trạng bán trùng ghế trong khi vẫn đảm bảo hệ thống phản hồi nhanh ngay cả khi có nhiều người dùng truy cập đồng thời.

---

### Lợi ích kỳ vọng

So với mô hình triển khai truyền thống, kiến trúc Cloud-Native mang lại nhiều lợi ích đáng kể.

### Lợi ích kỹ thuật

* Ngăn chặn xung đột đặt ghế trong các giao dịch đồng thời.
* Tăng khả năng mở rộng bằng cách tách biệt frontend và backend.
* Nâng cao độ tin cậy của hệ thống nhờ sử dụng **managed AWS services**.
* Giảm độ phức tạp trong vận hành thông qua việc tách biệt các thành phần dịch vụ.
* Hỗ trợ tự động gửi email xác nhận và vé điện tử.
* Dễ dàng mở rộng thêm tính năng trong tương lai mà không cần thay đổi lớn về kiến trúc.

### Lợi ích nghiệp vụ

* Cải thiện trải nghiệm khách hàng trong quá trình đặt vé.
* Tăng độ tin cậy của hệ thống và tỷ lệ giao dịch thành công.
* Giảm khối lượng công việc thủ công của quản trị viên.
* Nâng cao khả năng bảo trì và vận hành hệ thống.
* Tạo nền tảng Cloud hiện đại phù hợp cho việc phát triển lâu dài.

---

## 3. Kiến trúc giải pháp

Hệ thống HCMUT Cinema được xây dựng theo kiến trúc **Cloud-Native Architecture**, trong đó lớp giao diện, xử lý nghiệp vụ, lưu trữ dữ liệu và dịch vụ thông báo được tách thành các thành phần độc lập triển khai trên AWS.

Thay vì sử dụng một máy chủ duy nhất để xử lý toàn bộ chức năng, mỗi thành phần sẽ đảm nhận một nhiệm vụ riêng trong hệ thống. Kiến trúc này giúp tăng khả năng mở rộng, đơn giản hóa việc bảo trì và cho phép từng dịch vụ phát triển độc lập.

Luồng hoạt động tổng quát của hệ thống được mô tả như sau:

```text
Customer Browser
        │
        ▼
 Amazon S3 Static Website
        │
        ▼
 Amazon EC2 (Node.js REST API)
        │
 ┌──────┼───────────────┐
 ▼      ▼               ▼
Amazon RDS      DynamoDB      Amazon SES
(PostgreSQL)   Seat Locks     OTP & E-Ticket
```

### Các AWS Services sử dụng

<table><thead><tr><th>AWS Service</th><th>Mục đích sử dụng</th></tr></thead><tbody><tr><td>Amazon S3</td><td>Lưu trữ và phân phối website frontend cho khách hàng và quản trị viên.</td></tr><tr><td>Amazon EC2</td><td>Chạy ứng dụng backend Node.js và cung cấp RESTful APIs.</td></tr><tr><td>Amazon RDS PostgreSQL</td><td>Lưu trữ dữ liệu giao dịch lâu dài như người dùng, phim, rạp, lịch chiếu và vé.</td></tr><tr><td>Amazon DynamoDB</td><td>Quản lý khóa ghế tạm thời bằng ConditionExpression và TTL để ngăn chặn Race Condition.</td></tr><tr><td>Amazon SES</td><td>Tự động gửi email OTP xác thực và vé điện tử sau khi thanh toán thành công.</td></tr></tbody></table>

### Thiết kế các thành phần

#### Amazon S3

Amazon S3 chịu trách nhiệm lưu trữ và phân phối ứng dụng frontend. Vì frontend chủ yếu bao gồm các tệp HTML, CSS và JavaScript, S3 là giải pháp hosting đơn giản, tin cậy, chi phí thấp và có độ sẵn sàng cao.

#### Amazon EC2

Amazon EC2 lưu trữ backend được phát triển bằng Node.js và Express. Backend xử lý toàn bộ logic nghiệp vụ, giao tiếp với cơ sở dữ liệu, xác thực yêu cầu từ người dùng và tương tác với các dịch vụ AWS thông qua AWS SDK.

#### Amazon RDS PostgreSQL

Amazon RDS lưu trữ toàn bộ dữ liệu giao dịch quan trọng yêu cầu tính nhất quán cao và tuân thủ ACID, bao gồm tài khoản người dùng, thông tin phim, phòng chiếu, lịch chiếu, đặt vé và thanh toán.

#### Amazon DynamoDB

DynamoDB được sử dụng để triển khai cơ chế **real-time seat locking**. Bằng cách kết hợp **ConditionExpression** với **Time-to-Live (TTL)**, hệ thống đảm bảo không có hai khách hàng nào có thể đặt cùng một ghế tại cùng một thời điểm, đồng thời tự động giải phóng các khóa đã hết hạn.

#### Amazon SES

Amazon Simple Email Service (SES) chịu trách nhiệm gửi email xác thực **One-Time Password (OTP)** trong quá trình thanh toán và gửi vé điện tử chứa **QR Code** sau khi đặt vé thành công.

## 4. Triển khai kỹ thuật

### Các giai đoạn phát triển

#### Giai đoạn 1

* Phân tích yêu cầu hệ thống
* Nghiên cứu các hệ thống đặt vé rạp chiếu phim hiện có
* Xác định phạm vi dự án

#### Giai đoạn 2

* Thiết kế kiến trúc AWS
* Thiết kế cơ sở dữ liệu
* Lập kế hoạch RESTful APIs

#### Giai đoạn 3

* Phát triển backend
* Phát triển frontend
* Tích hợp các dịch vụ AWS

#### Giai đoạn 4

* Kiểm thử hệ thống
* Kiểm thử concurrency
* Triển khai trên AWS
* Hoàn thiện tài liệu dự án

### Yêu cầu kỹ thuật

Ngôn ngữ lập trình

* JavaScript
* HTML
* CSS

Framework

* Node.js
* Express.js

Cơ sở dữ liệu

* PostgreSQL
* DynamoDB

Nền tảng Cloud

* Amazon Web Services

Công cụ phát triển

* AWS CLI
* Visual Studio Code
* GitHub

---

## 5. Kế hoạch thực hiện và các mốc quan trọng

<table><thead><tr><th>Giai đoạn</th><th>Nội dung thực hiện</th></tr></thead><tbody><tr><td>Week 1</td><td>Nghiên cứu AWS services và yêu cầu hệ thống</td></tr><tr><td>Week 2</td><td>Thiết kế kiến trúc và cơ sở dữ liệu</td></tr><tr><td>Week 3</td><td>Triển khai backend APIs</td></tr><tr><td>Week 4</td><td>Phát triển giao diện frontend</td></tr><tr><td>Week 5</td><td>Tích hợp các AWS services</td></tr><tr><td>Week 6</td><td>Kiểm thử và sửa lỗi</td></tr><tr><td>Week 7</td><td>Triển khai và tối ưu hệ thống</td></tr><tr><td>Week 8</td><td>Hoàn thiện tài liệu và chuẩn bị trình bày</td></tr></tbody></table>

---

## 6. Dự toán chi phí

Dự án được phát triển chủ yếu phục vụ mục đích học tập và nghiên cứu, ưu tiên sử dụng **AWS Free Tier** whenever possible.

### Chi phí hạ tầng dự kiến

<table><thead><tr><th>Service</th><th style="text-align:right">Chi phí dự kiến</th></tr></thead><tbody><tr><td>Amazon EC2</td><td style="text-align:right">Free Tier</td></tr><tr><td>Amazon S3</td><td style="text-align:right">Free Tier</td></tr><tr><td>Amazon RDS</td><td style="text-align:right">Free Tier</td></tr><tr><td>Amazon DynamoDB</td><td style="text-align:right">Free Tier</td></tr><tr><td>Amazon SES</td><td style="text-align:right">Chi phí gửi email tối thiểu</td></tr><tr><td>Data Transfer</td><td style="text-align:right">Free Tier</td></tr></tbody></table>

Chi phí vận hành ước tính:

**Khoảng 0–5 USD/tháng** tùy thuộc vào mức sử dụng thực tế.

---

## 7. Đánh giá rủi ro

### Các rủi ro tiềm ẩn

#### Đặt vé đồng thời với số lượng lớn

Nhiều người dùng có thể cùng lúc đặt một ghế giống nhau.

**Giải pháp**

Sử dụng cơ chế ghi có điều kiện của DynamoDB kết hợp với khóa ghế bằng TTL.

---

#### Phát sinh chi phí AWS ngoài dự kiến

Các tài nguyên có thể vẫn tiếp tục chạy sau khi kiểm thử.

**Giải pháp**

Xóa các tài nguyên không sử dụng và theo dõi **AWS Billing Dashboard** thường xuyên.

---

#### Lỗi gửi email OTP

Email xác thực có thể không được gửi thành công.

**Giải pháp**

Thực hiện cơ chế gửi lại email và kiểm tra tính hợp lệ của địa chỉ email trước khi thanh toán.

---

#### Sự cố cơ sở dữ liệu

Cơ sở dữ liệu có thể gặp downtime ngoài dự kiến.

**Giải pháp**

Duy trì cơ chế sao lưu dữ liệu định kỳ và quy trình khôi phục dữ liệu khi cần thiết.

---

## 8. Kết quả kỳ vọng

### Kết quả kỹ thuật

Sau khi hoàn thành dự án, hệ thống kỳ vọng sẽ:

* Triển khai thành công trên AWS.
* Hỗ trợ đặt vé xem phim trực tuyến.
* Ngăn chặn việc bán trùng ghế.
* Tự động gửi email OTP xác thực.
* Tạo vé điện tử chứa QR Code.
* Minh họa việc sử dụng thực tế nhiều AWS services trong một hệ thống hoàn chỉnh.

### Kết quả học tập

Thông qua dự án này, các thành viên trong nhóm sẽ tích lũy được kinh nghiệm về:

* Triển khai ứng dụng trên AWS Cloud
* Thiết kế kiến trúc Cloud-Native
* Phát triển RESTful APIs
* Thiết kế cơ sở dữ liệu
* Khái niệm Microservices
* Làm việc nhóm trong dự án phần mềm
* Quy trình triển khai và vận hành ứng dụng
