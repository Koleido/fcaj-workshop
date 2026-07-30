---
title: "Tổng quan dự án"
date: 2026-07-28
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

# Giới thiệu

**HCMUT Cinema** là hệ thống Đặt Vé Phim Thời Gian Thực (Real-time Cinema Booking System). Hệ thống được thiết kế theo kiến trúc Cloud-Native, tận dụng tối đa các dịch vụ đám mây của AWS để đảm bảo tính sẵn sàng cao (High Availability), khả năng mở rộng linh hoạt (Scalability) và bảo mật dữ liệu.

Trong bài thực hành (workshop) này, bạn sẽ được hướng dẫn từng bước cấu hình và triển khai 5 dịch vụ cốt lõi của AWS bao gồm: **Amazon S3, Amazon EC2, Amazon RDS, Amazon DynamoDB, và Amazon SES**. 

# Sơ đồ kiến trúc (Architecture Overview)

Dưới đây là sơ đồ kiến trúc tổng thể của hệ thống, minh họa cách các thành phần và các dịch vụ AWS tương tác với nhau trong hệ sinh thái đám mây:

![Sơ đồ Kiến trúc Tổng thể](/images/5-Workshop/5.1-Architecture-Overview/architecture.png)

Hệ thống tuân thủ nguyên tắc **Decoupled (Phân tách lỏng lẻo)** để tối ưu hóa hiệu suất, cụ thể:

* **Amazon S3:** Host giao diện web tĩnh (Frontend). Hoạt động hoàn toàn độc lập với Server, giúp giảm thiểu tải cho hệ thống Backend và tăng tốc độ tải trang.
* **Amazon EC2:** Máy chủ ảo chạy logic Backend Node.js, đóng vai trò là API Server và điều phối kết nối WebSocket (Socket.io) cho các tác vụ thời gian thực.
* **Amazon RDS (PostgreSQL):** Cơ sở dữ liệu quan hệ lưu trữ dữ liệu bền vững (thông tin phim, rạp, suất chiếu, người dùng, hóa đơn).
* **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL tốc độ cao được sử dụng chuyên biệt để xử lý tính năng "Khóa ghế thời gian thực". Tận dụng tính năng **TTL (Time-To-Live)** để tự động giải phóng ghế nêú khách hàng không thanh toán trong vòng 5 phút mà không cần Backend phải chạy Cron Job phức tạp.
* **Amazon SES (Simple Email Service):** Dịch vụ tự động gửi mail chứa vé điện tử và mã OTP xác thực, đảm bảo tỷ lệ vào Inbox cao và bảo mật tuyệt đối thông qua AWS SDK.
