---
title: "Blog 2"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Xây dựng hệ thống đặt vé xem phim Cloud-Native với Amazon S3, EC2, RDS và DynamoDB

Bài viết này giới thiệu kiến trúc tổng thể của dự án **HCMUT Cinema**, một hệ thống quản lý và đặt vé xem phim được phát triển theo mô hình **Cloud-Native** trên nền tảng **Amazon Web Services (AWS)**.

Thông qua dự án thực tế, bài viết trình bày cách kết hợp nhiều dịch vụ AWS để xây dựng một hệ thống có khả năng mở rộng, dễ bảo trì và đáp ứng các yêu cầu của một ứng dụng hiện đại.

## Nội dung chính

### Tổng quan dự án

Một hệ thống bán vé xem phim trực tuyến cần xử lý nhiều nghiệp vụ khác nhau như:

- Quản lý phim.
- Đặt vé.
- Chọn ghế.
- Thanh toán.
- Gửi email xác nhận.
- Quản trị hệ thống.

Nếu triển khai toàn bộ trên một máy chủ duy nhất, hệ thống sẽ gặp nhiều khó khăn khi lưu lượng truy cập tăng cao. Vì vậy, dự án lựa chọn mô hình **Cloud-Native**, trong đó các thành phần của hệ thống được tách biệt và triển khai trên các dịch vụ AWS phù hợp.

### Amazon S3 – Lưu trữ giao diện người dùng

Phần giao diện (Frontend) được triển khai trên **Amazon S3**, cung cấp giải pháp lưu trữ website tĩnh với chi phí thấp và độ sẵn sàng cao.

Amazon S3 đảm nhiệm:

- Lưu trữ các tệp HTML, CSS và JavaScript.
- Cung cấp giao diện cho người dùng và quản trị viên.
- Triển khai website tĩnh một cách đơn giản và hiệu quả.

### Amazon EC2 – Xử lý nghiệp vụ

Backend được triển khai trên **Amazon EC2** sử dụng **Node.js** và **Express**.

Máy chủ Backend chịu trách nhiệm xử lý toàn bộ nghiệp vụ của hệ thống như:

- Xác thực người dùng.
- Đặt vé.
- Thanh toán.
- Kết nối cơ sở dữ liệu.
- Gửi email thông báo.

EC2 đóng vai trò là trung tâm xử lý của toàn bộ hệ thống.

### Amazon RDS PostgreSQL – Cơ sở dữ liệu giao dịch

Các dữ liệu nghiệp vụ quan trọng được lưu trữ trên **Amazon RDS PostgreSQL**, bao gồm:

- Người dùng.
- Phim.
- Rạp chiếu.
- Suất chiếu.
- Vé.
- Thông tin thanh toán.

PostgreSQL đảm bảo tính nhất quán của dữ liệu và hỗ trợ các giao dịch theo chuẩn ACID, phù hợp với các nghiệp vụ yêu cầu độ tin cậy cao.

### Amazon DynamoDB – Khóa ghế tạm thời

**Amazon DynamoDB** được sử dụng để triển khai cơ chế khóa ghế theo thời gian thực.

Khi khách hàng chọn ghế:

- Hệ thống tạo một khóa tạm thời.
- Ghế được giữ trong khoảng năm phút.
- Khóa sẽ tự động hết hạn nếu khách hàng không hoàn tất thanh toán.

Giải pháp này giúp hạn chế tình trạng nhiều người cùng đặt một ghế trong cùng thời điểm.

### Amazon SES – Dịch vụ gửi email

Sau khi giao dịch thành công, **Amazon Simple Email Service (SES)** sẽ tự động gửi:

- Email xác thực OTP.
- Vé điện tử.
- Mã QR xác nhận.

Toàn bộ quá trình gửi thông báo được tự động hóa, góp phần nâng cao trải nghiệm người dùng.

### Lợi ích của kiến trúc Cloud-Native

Việc áp dụng kiến trúc Cloud-Native mang lại nhiều lợi ích như:

- Tách biệt hoàn toàn Frontend và Backend.
- Dễ dàng mở rộng từng thành phần của hệ thống.
- Lựa chọn cơ sở dữ liệu phù hợp với từng loại dữ liệu.
- Tận dụng hiệu quả các dịch vụ Managed Services của AWS.
- Thuận tiện cho việc bảo trì và phát triển các tính năng mới.

## Hình ảnh bài viết

![Blog 2](/images/Blogs/Blog2-1.png)

![Blog 2](/images/Blogs/Blog2-2.png)

## Liên kết bài viết

Bài viết được đăng trong cộng đồng Facebook **AWS Study Group** như một hoạt động chia sẻ kiến thức trong chương trình thực tập.

> **Facebook Post:** *[(Xây dựng hệ thống đặt vé xem phim Cloud-Native với Amazon S3, EC2, RDS và DynamoDB)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224973238267636&hoisted_section_header_type=recently_seen&__cft__[0]=AZYk_Ku1LBPhTfWojUpXBqOTOKNMBOkd-thieWVEqABkcQfS_mCvm3BmbYJWPt8FT6511oD6O7gXr-ROprbXQLW_cQCpwz-Muem-xLM1DQ3YPbq4hFUHG-Y1ppwQHDPYkx254rZtCHSqvnuMjvKAk4y5&__tn__=%2CO%2CP-R)*

## Đánh giá

Việc xây dựng và chia sẻ bài viết này là cơ hội để tổng hợp kiến thức về cách thiết kế một hệ thống **Cloud-Native** trên nền tảng AWS thông qua một dự án thực tế.

Thông qua dự án **HCMUT Cinema**, bài viết minh họa vai trò của các dịch vụ như **Amazon S3**, **Amazon EC2**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB** và **Amazon SES**, đồng thời cho thấy cách các dịch vụ này phối hợp với nhau để tạo nên một hệ thống có khả năng mở rộng, dễ bảo trì và đáp ứng tốt các yêu cầu của ứng dụng hiện đại.

Bên cạnh đó, hoạt động chia sẻ bài viết trên cộng đồng AWS cũng góp phần nâng cao kỹ năng tổng hợp kiến thức, trình bày nội dung kỹ thuật và lan tỏa những kinh nghiệm thực tế đến các bạn sinh viên đang tìm hiểu về Cloud Computing và phát triển ứng dụng Cloud-Native.