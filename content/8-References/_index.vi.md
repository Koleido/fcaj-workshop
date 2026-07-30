---
title: "Tài liệu tham khảo"
date: 2026-07-29
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

Phần này tổng hợp các tài liệu tham khảo và nguồn tài nguyên được sử dụng trong suốt quá trình thực tập, bao gồm mã nguồn dự án, video minh họa và các tài liệu học tập.

## Mã nguồn dự án

Mã nguồn của dự án **HCMUT Cinema** được lưu trữ trên GitHub.

- **Kho mã nguồn:** https://github.com/HuyPT3508/AWS_Final.git

## Video minh họa dự án

Video trình bày các chức năng chính và quá trình triển khai của dự án có thể được xem tại đường dẫn sau.

- **Video Demo (Google Drive):** [Demo Workshop](https://drive.google.com/file/d/1BRA68CK4h_CU5k95bLJXEMu5BOlVSGKD/view)

## Triển khai dự án

Dự án được triển khai với hai giao diện web riêng biệt trên dịch vụ **Amazon S3 Static Website Hosting**.

### Giao diện khách hàng

Giao diện dành cho khách hàng cho phép người dùng:

- Xem danh sách phim đang chiếu
- Xem thông tin chi tiết của phim
- Lựa chọn vị trí ghế ngồi
- Đặt vé xem phim trực tuyến

**Website:** [HCMUT Cinema](http://hcmut-cinema-frontend-huypt.s3-website-ap-southeast-1.amazonaws.com)

### Giao diện quản trị viên

Giao diện quản trị viên được sử dụng để thực hiện các chức năng quản lý, bao gồm:

- Quản lý thông tin phim
- Thêm phim mới
- Chỉnh sửa thông tin phim
- Xóa phim khỏi hệ thống

**Website:** [HCMUT Cinema (Admin)](http://hcmut-cinema-frontend-huypt.s3-website-ap-southeast-1.amazonaws.com/admin.html)

## Tài liệu tham khảo bổ sung

Trong suốt quá trình thực tập, các tài liệu chính thức của AWS dưới đây được sử dụng thường xuyên để nghiên cứu, triển khai, cấu hình và xử lý các vấn đề phát sinh:

- AWS Skill Builder
- AWS Documentation
- AWS Well-Architected Framework
- Amazon EC2 Documentation
- Amazon S3 Documentation
- Amazon RDS Documentation
- Amazon DynamoDB Documentation
- Amazon SES Documentation

Các tài liệu chính thức này cung cấp hướng dẫn về kiến trúc hệ thống, tài liệu kỹ thuật của từng dịch vụ và các thực tiễn tốt nhất (best practices) trong việc thiết kế và triển khai các ứng dụng Cloud-Native trên nền tảng AWS.