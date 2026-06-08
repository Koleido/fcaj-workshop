---
title: "Worklog Tuần 1"
date: 2026-06-08
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 1

Trong tuần đầu tiên, mục tiêu chính của tôi là làm quen với chương trình thực tập, tìm hiểu tổng quan về AWS, đồng thời cài đặt và chuẩn bị môi trường làm việc cần thiết cho các tuần tiếp theo. Đây là giai đoạn nền tảng nên tôi tập trung vào việc hiểu quy trình, nắm các khái niệm cơ bản và thực hành những thao tác đầu tiên trên AWS.

### Nội dung thực hiện trong tuần 1

#### 1. Làm quen với chương trình thực tập và các thành viên trong nhóm
Ở đầu tuần, tôi được giới thiệu về chương trình First Cloud AI Journey, tìm hiểu sơ bộ về định hướng thực tập, cách làm việc và các yêu cầu cần hoàn thành trong thời gian tham gia. Tôi cũng dành thời gian làm quen với các thành viên khác để thuận tiện cho việc trao đổi và hỗ trợ nhau trong quá trình học tập.

#### 2. Tìm hiểu tổng quan về AWS
Tôi bắt đầu học AWS từ những khái niệm cơ bản nhất như AWS là gì, tại sao cloud computing lại quan trọng, và các nhóm dịch vụ chính của AWS. Trong giai đoạn này, tôi tập trung nhận biết các mảng dịch vụ phổ biến như:
- Compute
- Storage
- Networking
- Database
- Security
- Monitoring

Việc học ở mức tổng quan giúp tôi không bị rối khi sau này tiếp cận từng dịch vụ cụ thể.

#### 3. Tạo tài khoản AWS Free Tier
Sau khi tìm hiểu sơ lược về AWS, tôi tiến hành tạo tài khoản AWS Free Tier để có môi trường thực hành. Đây là bước rất quan trọng vì hầu hết các bài thực hành sau này đều cần truy cập trực tiếp vào AWS Console. Trong quá trình tạo tài khoản, tôi chú ý đến các thông tin xác thực, phương thức thanh toán và những giới hạn của Free Tier để tránh phát sinh chi phí không mong muốn.

### AWS Management Console

Trong quá trình thực hành, tôi đã đăng nhập vào AWS Management Console để làm quen với giao diện quản trị và quan sát các dịch vụ cơ bản.

![AWS Console Home](/images/Week1/AwsConsoleHome.png)

*AWS Management Console after successful login.*

#### 4. Làm quen với AWS Management Console và AWS CLI
Tiếp theo, tôi tìm hiểu hai cách thao tác chính với AWS:
- **AWS Management Console**: giao diện web trực quan để thao tác thủ công.
- **AWS CLI**: công cụ dòng lệnh giúp thao tác nhanh và thuận tiện hơn trong nhiều tình huống.

Tôi thực hiện cài đặt và cấu hình AWS CLI trên máy tính, bao gồm việc thiết lập Access Key, Secret Key và Default Region. Sau đó tôi thử một số lệnh cơ bản để kiểm tra cấu hình và làm quen với cú pháp.

### AWS CLI Configuration

Tôi đã cài đặt và kiểm tra AWS CLI trên máy tính cá nhân để làm quen với thao tác dòng lệnh.

![AWS CLI](/images/Week1/AwsCli.png)

*Kiểm tra phiên bản AWS CLI và cấu hình.*

#### 5. Tìm hiểu EC2 và thực hành khởi tạo instance
Ở cuối tuần, tôi bắt đầu học về Amazon EC2, một trong những dịch vụ quan trọng nhất của AWS. Tôi tìm hiểu các khái niệm cơ bản như:
- Instance types
- AMI
- EBS
- Elastic IP
- SSH connection

Sau đó, tôi thực hành tạo một EC2 instance, thử kết nối vào máy ảo bằng SSH và tìm hiểu cách gắn thêm EBS volume. Đây là phần giúp tôi hiểu rõ hơn cách AWS vận hành một máy chủ ảo trên cloud.

### Kết quả đạt được

Sau tuần đầu tiên, tôi đã đạt được một số kết quả ban đầu như sau:

- Hiểu được AWS là gì và nắm được các nhóm dịch vụ cơ bản của AWS.
- Tạo và cấu hình thành công tài khoản AWS Free Tier.
- Làm quen với giao diện AWS Management Console.
- Cài đặt và cấu hình AWS CLI trên máy tính cá nhân.
- Thực hiện được một số thao tác cơ bản bằng AWS CLI.
- Bước đầu hiểu về EC2, SSH, AMI, EBS và Elastic IP.
- Tự mình thực hành tạo và kết nối đến một EC2 instance.

### Khó khăn gặp phải

Trong tuần đầu tiên, tôi gặp một số khó khăn nhất định:
- Chưa quen với cách tổ chức dịch vụ của AWS nên ban đầu hơi khó hình dung.
- Một số thuật ngữ như AMI, EBS, Elastic IP hay Security Group còn khá mới.
- Việc cấu hình AWS CLI cần cẩn thận, nếu nhập sai thông tin có thể không kết nối được.
- Khi kết nối SSH vào EC2, tôi cần kiểm tra lại key pair, quyền truy cập và cấu hình mạng để tránh lỗi.

Tuy nhiên, nhờ tìm hiểu tài liệu và thực hành nhiều lần, tôi dần hiểu rõ hơn cách sử dụng các công cụ này.

### Bài học rút ra

Qua tuần 1, tôi nhận thấy việc học cloud computing cần bắt đầu từ nền tảng thật chắc. Thay vì cố học quá nhiều dịch vụ cùng lúc, tôi nên nắm rõ cách một dịch vụ hoạt động, cách truy cập nó trên Console, và cách thao tác tương ứng bằng CLI. Đây là bước khởi đầu cần thiết để có thể học tốt các phần thực hành phức tạp hơn ở những tuần sau.

### Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi dự định sẽ:
- Ôn lại các khái niệm cơ bản đã học ở tuần 1.
- Tiếp tục thực hành với EC2 để thao tác thành thạo hơn.
- Tìm hiểu sâu hơn về S3, VPC và các thành phần liên quan.
- Ghi chú lại các lệnh, quy trình và lỗi thường gặp để thuận tiện cho việc làm báo cáo sau này.