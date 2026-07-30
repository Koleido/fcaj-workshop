---
title: "Blog 3"
date: 2026-07-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Giải quyết Race Condition trong hệ thống đặt vé với Amazon DynamoDB

Trong quá trình phát triển dự án **HCMUT Cinema** trên nền tảng Amazon Web Services (AWS), nhóm chúng tôi đã gặp một bài toán rất phổ biến trong các hệ thống đặt vé trực tuyến: **làm thế nào để nhiều người dùng không thể đặt cùng một ghế tại cùng một thời điểm**.

Bài viết này giới thiệu khái niệm **Race Condition**, phân tích nguyên nhân gây ra vấn đề trong các hệ thống có lượng truy cập đồng thời lớn và trình bày cách **Amazon DynamoDB** được sử dụng để xây dựng cơ chế khóa ghế theo thời gian thực.

## Nội dung chính

### Race Condition là gì?

Race Condition xảy ra khi nhiều người dùng hoặc nhiều tiến trình cùng truy cập và thay đổi một tài nguyên tại gần như cùng một thời điểm.

Ví dụ, nếu chỉ còn duy nhất ghế **A10**, hai khách hàng cùng chọn ghế này trong cùng thời điểm thì cả hai yêu cầu đều có thể thành công nếu hệ thống chỉ kiểm tra trạng thái ghế trước khi cập nhật dữ liệu. Điều này dẫn đến việc một ghế được bán cho nhiều khách hàng khác nhau.

### Khóa ghế bằng Amazon DynamoDB

Để giải quyết vấn đề trên, dự án sử dụng **Amazon DynamoDB** thay vì chỉ dựa vào cơ sở dữ liệu quan hệ.

Mỗi khi người dùng chọn ghế, backend sẽ tạo một bản ghi khóa ghế tạm thời trong DynamoDB. Nếu ghế đã được người khác giữ trước đó, yêu cầu mới sẽ bị từ chối ngay lập tức, đảm bảo chỉ có một người có thể giữ ghế tại cùng một thời điểm.

### Sử dụng ConditionExpression

Cơ chế quan trọng nhất trong giải pháp là **ConditionExpression**.

Hệ thống chỉ tạo bản ghi khóa ghế khi dữ liệu tương ứng chưa tồn tại:

```text
ConditionExpression:
attribute_not_exists(SeatKey)
```

Nhờ tính chất **Atomic Operation** của DynamoDB, dù có nhiều yêu cầu được gửi đến đồng thời thì cũng chỉ có một yêu cầu duy nhất được thực hiện thành công.

### Tự động mở khóa với TTL

Một tình huống khác cần xử lý là người dùng chọn ghế nhưng không hoàn tất thanh toán.

Để giải quyết vấn đề này, dự án sử dụng tính năng **Time-to-Live (TTL)** của DynamoDB.

Mỗi bản ghi khóa ghế sẽ tự động hết hạn sau khoảng năm phút. Khi hết thời gian này, DynamoDB sẽ tự động xóa bản ghi mà không cần xây dựng thêm các tác vụ nền hoặc chương trình dọn dẹp dữ liệu.

Nhờ đó, hệ thống luôn duy trì khả năng phục vụ những khách hàng tiếp theo và hạn chế tình trạng ghế bị giữ quá lâu.

### Kiến trúc tổng thể của hệ thống

Dự án HCMUT Cinema kết hợp nhiều dịch vụ AWS, trong đó mỗi dịch vụ đảm nhận một vai trò riêng:

- **Amazon S3** – Lưu trữ giao diện người dùng.
- **Amazon EC2** – Chạy ứng dụng Backend Node.js.
- **Amazon RDS PostgreSQL** – Lưu trữ dữ liệu nghiệp vụ.
- **Amazon DynamoDB** – Quản lý cơ chế khóa ghế tạm thời.
- **Amazon SES** – Gửi email OTP và vé điện tử.

Việc phân tách chức năng giữa các dịch vụ giúp hệ thống dễ mở rộng, dễ bảo trì và phù hợp với mô hình kiến trúc Cloud-Native.

## Hình minh họa bài viết

![Blog 3](/images/Blogs/Blog3-1.png)

![Blog 3](/images/Blogs/Blog3-2.png)

## Liên kết bài viết

Bài viết gốc được đăng tải trong cộng đồng Facebook **AWS Study Group** như một hoạt động chia sẻ kiến thức trong thời gian tham gia chương trình thực tập.

> **Bài đăng Facebook:** *[(Giải quyết Race Condition trong hệ thống đặt vé với Amazon DynamoDB)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224958578269102&hoisted_section_header_type=recently_seen&__cft__[0]=AZZg1au5zUs8YYSUu_gkvVNkZj0Xfp8LEeKGInZG4R2Vx-iIr8FN9kQJHx7zzPUD_rt9N7nKm47pZd3YkPIjp23fnm65eldG2Lz0MaRRAcJAQd7Yxmf-aYbdm9guWlodWjtBk3uwGfM5FcIuZtZhuuuL&__tn__=%2CO%2CP-R)*

## Cảm nhận

Thông qua việc thực hiện bài viết này, nhóm chúng tôi có cơ hội tìm hiểu sâu hơn về cách các dịch vụ AWS có thể giải quyết những bài toán thực tế trong phát triển phần mềm.

Việc kết hợp **Amazon DynamoDB**, **ConditionExpression** và **Time-to-Live (TTL)** cho thấy xử lý truy cập đồng thời không chỉ là vấn đề của cơ sở dữ liệu mà còn là bài toán về thiết kế kiến trúc hệ thống.

Nhóm hy vọng bài viết sẽ giúp những người mới tìm hiểu AWS có thêm một ví dụ thực tế về cách xây dựng cơ chế khóa tài nguyên hiệu quả trong các hệ thống đặt vé trực tuyến theo hướng Cloud-Native.