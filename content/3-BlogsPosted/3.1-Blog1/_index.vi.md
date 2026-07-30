---
title: "Blog 1"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tìm hiểu Race Condition và cách Amazon DynamoDB giải quyết bài toán đặt ghế đồng thời

Bài viết này giới thiệu một trong những vấn đề phổ biến nhất của các hệ thống đặt vé trực tuyến: **ngăn nhiều người dùng đặt cùng một ghế tại cùng một thời điểm**.

Thông qua dự án **HCMUT Cinema**, bài viết giải thích khái niệm **Race Condition**, phân tích nguyên nhân xảy ra trong môi trường có nhiều truy cập đồng thời và trình bày cách **Amazon DynamoDB** được sử dụng để giải quyết bài toán này một cách hiệu quả và có khả năng mở rộng.

## Nội dung chính

### Race Condition là gì?

Race Condition xảy ra khi nhiều người dùng hoặc nhiều tiến trình cùng truy cập và thay đổi một tài nguyên tại cùng một thời điểm.

Ví dụ, nếu hai khách hàng cùng chọn ghế **A10** trong cùng một thời điểm, cả hai yêu cầu đều có thể thành công nếu hệ thống chỉ kiểm tra tình trạng ghế trước khi cập nhật dữ liệu. Điều này có thể dẫn đến việc một ghế được bán cho nhiều khách hàng khác nhau.

### Vì sao không chỉ sử dụng cơ sở dữ liệu truyền thống?

Các hệ quản trị cơ sở dữ liệu quan hệ như PostgreSQL hỗ trợ Transaction và cơ chế Lock nhằm đảm bảo tính nhất quán của dữ liệu.

Tuy nhiên, khi số lượng truy cập tăng cao, việc khóa dữ liệu liên tục có thể làm giảm hiệu năng và hạn chế khả năng mở rộng của hệ thống. Vì vậy, nhiều ứng dụng cloud-native hiện nay kết hợp cơ sở dữ liệu quan hệ với các dịch vụ NoSQL để xử lý hiệu quả hơn các yêu cầu đồng thời.

### Sử dụng Amazon DynamoDB để khóa ghế

Giải pháp được đề xuất sử dụng **Amazon DynamoDB** để quản lý cơ chế khóa ghế tạm thời.

Mỗi ghế được định danh bằng một khóa duy nhất gồm:

- Mã phim (Movie ID)
- Mã suất chiếu (Showtime ID)
- Số ghế (Seat Number)

Khi khách hàng chọn ghế:

- Backend gửi yêu cầu đến DynamoDB.
- DynamoDB sử dụng **ConditionExpression** để kiểm tra ghế đã được khóa hay chưa.
- Nếu chưa tồn tại khóa, hệ thống sẽ tạo khóa mới.
- Nếu khóa đã tồn tại, yêu cầu sẽ bị từ chối ngay lập tức.

Do thao tác ghi dữ liệu là **Atomic Operation**, hệ thống đảm bảo không có hai người dùng cùng giữ một ghế tại cùng thời điểm.

### Tự động mở khóa bằng TTL

Một vấn đề khác là khách hàng chọn ghế nhưng không hoàn tất thanh toán.

Để giải quyết vấn đề này, hệ thống sử dụng **Time-to-Live (TTL)** của DynamoDB.

Mỗi Seat Lock sẽ tự động hết hạn sau khoảng năm phút. Khi hết thời gian, DynamoDB sẽ tự động xóa bản ghi mà không cần xây dựng thêm dịch vụ nền hay tác vụ định kỳ.

Cơ chế này giúp:

- Tự động giải phóng ghế.
- Cho phép khách hàng khác tiếp tục đặt ghế.
- Duy trì khả năng phục vụ ổn định ngay cả khi lưu lượng truy cập lớn.

### Kiến trúc AWS

Ngoài DynamoDB, dự án còn sử dụng nhiều dịch vụ AWS khác:

- **Amazon S3** – Lưu trữ và triển khai giao diện web tĩnh.
- **Amazon EC2** – Chạy Backend sử dụng Node.js và Express.
- **Amazon RDS PostgreSQL** – Lưu trữ dữ liệu giao dịch.
- **Amazon DynamoDB** – Thực hiện cơ chế khóa ghế theo thời gian thực.
- **Amazon SES** – Gửi mã OTP xác thực và vé điện tử qua email.

Các dịch vụ này phối hợp tạo thành một kiến trúc cloud-native có khả năng mở rộng, dễ bảo trì và đáp ứng tốt các yêu cầu của hệ thống đặt vé trực tuyến.

## Hình ảnh bài viết

![Blog 1](/images/Blogs/Blog1-1.png)

![Blog 1](/images/Blogs/Blog1-2.png)

## Liên kết bài viết

Bài viết được đăng trong cộng đồng Facebook **AWS Study Group** như một hoạt động chia sẻ kiến thức trong chương trình thực tập.

> **Facebook Post:** *[(Tìm hiểu Race Condition và cách Amazon DynamoDB giải quyết bài toán đặt ghế đồng thời)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224962828268677&hoisted_section_header_type=recently_seen&__cft__[0]=AZaFKQ8ESzH6KnoRHd6s91kKW-a8aVyJEUB__zBvPzUOmnZZprSgF7t8FVPiLj2uU1tVgJTYWsySdAKKDDhUPJwaNvVUGcq39FUNHzdiV56ZLkTLz48J5qtFwX4_Uf3kugJRqWUWPy4LOiWregnazW7k&__tn__=%2CO%2CP-R)*

## Đánh giá

Việc xây dựng và chia sẻ bài viết này là cơ hội để tổng hợp kiến thức về một bài toán thực tế thường gặp trong các hệ thống cloud-native có lượng truy cập lớn.

Thông qua ví dụ từ dự án **HCMUT Cinema**, bài viết minh họa cách **Amazon DynamoDB** kết hợp với **ConditionExpression** và **Time-to-Live (TTL)** để ngăn chặn Race Condition, đồng thời duy trì khả năng mở rộng và tính ổn định của hệ thống đặt vé trực tuyến.

Bên cạnh đó, hoạt động chia sẻ kiến thức trên cộng đồng AWS cũng góp phần nâng cao kỹ năng trình bày kỹ thuật, tổng hợp tài liệu và lan tỏa những kinh nghiệm thực tế đến những người đang tìm hiểu về Cloud Computing.