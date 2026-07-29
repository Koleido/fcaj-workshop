---
title: "Các bài Blog đã đăng"
date: 2026-07-20
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The information dưới đây chỉ mang tính tham khảo.
{{% /notice %}} -->

Trong quá trình tham gia chương trình **First Cloud AI Journey**, nhóm chúng tôi đã tích cực chia sẻ những kiến thức và kinh nghiệm thu được thông qua việc đăng tải ba bài viết kỹ thuật trên cộng đồng **AWS Study Group**.

Mỗi thành viên trong nhóm phụ trách một bài viết, với nội dung xoay quanh quá trình xây dựng dự án **HCMUT Cinema** trên nền tảng Amazon Web Services (AWS). Các bài blog không chỉ giới thiệu các dịch vụ AWS mà còn chia sẻ những kinh nghiệm triển khai thực tế, tư duy thiết kế kiến trúc Cloud-Native cũng như các giải pháp giải quyết những bài toán thường gặp trong quá trình phát triển phần mềm.

Các bài viết đã đăng được tóm tắt như sau.

---

### [Blog 1 – Tìm hiểu Race Condition và cách Amazon DynamoDB giải quyết bài toán đặt ghế đồng thời](3.1-Blog1/)

Bài viết giới thiệu khái niệm **Race Condition** trong các hệ thống đặt vé trực tuyến và trình bày cách **Amazon DynamoDB** sử dụng **ConditionExpression** kết hợp với **Time-to-Live (TTL)** để ngăn chặn tình trạng nhiều người dùng đặt cùng một ghế tại cùng một thời điểm. Đồng thời, bài viết cũng giới thiệu kiến trúc AWS được áp dụng trong dự án HCMUT Cinema.

---

### [Blog 2 – Xây dựng hệ thống đặt vé phim Cloud-Native với Amazon S3, EC2, RDS và DynamoDB](3.2-Blog2/)

Bài viết trình bày tổng quan kiến trúc Cloud-Native của dự án HCMUT Cinema, giải thích vai trò của từng dịch vụ AWS như **Amazon S3**, **Amazon EC2**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB** và **Amazon SES** trong việc xây dựng một hệ thống đặt vé trực tuyến có khả năng mở rộng, dễ bảo trì và phù hợp với các ứng dụng hiện đại.

---

### [Blog 3 – Giải quyết Race Condition trong hệ thống đặt vé với Amazon DynamoDB](3.3-Blog3/)

Bài viết phân tích chi tiết hơn về cơ chế khóa ghế theo thời gian thực bằng **Amazon DynamoDB**, tập trung vào việc sử dụng **ConditionExpression** và **Time-to-Live (TTL)** để xử lý các yêu cầu đặt vé đồng thời. Qua đó, bài viết chia sẻ những kinh nghiệm thực tế trong việc áp dụng các dịch vụ AWS để giải quyết các bài toán phổ biến trong phát triển phần mềm.