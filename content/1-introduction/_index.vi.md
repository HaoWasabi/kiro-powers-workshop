---
title: "Giới thiệu"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>1. </strong>"
---

#### Giới thiệu 
Trong bài thực hành này, chúng ta sẽ tiến hành triển khai ứng dụng sử dụng Auto Scaling Group để đảm bảo khả năng mở rộng linh hoạt theo nhu cầu của người dùng. Chúng ta cũng sẽ triển khai Load Balancer để cân bằng tải và phân phối yêu cầu truy cập từ người dùng đến Application Tier.

ℹ️ **Information**: Hãy đảm bảo bạn đã tham khảo tài liệu [Triển khai Ứng dụng FCJ Management trên Máy ảo Windows/AmazonLinux](https://000004.awsstudygroup.com/) và hiểu cách triển khai ứng dụng trên máy ảo. Chúng ta sẽ sử dụng máy ảo **FCJ Management** đã triển khai để thực hiện việc triển khai và mở rộng trong Auto Scaling Group.

#### Auto Scaling Group
1. Tại sao cần sử dụng Auto scaling group?
   
   Khi ứng dụng của chúng ta đưa vào hoạt động, lượng người truy cập sẽ thay đổi theo thời gian, do đó chúng ta cần thường xuyên thay đổi (scaling) lượng instance nhằm nâng cao tính sẵn sàng và tiết kiệm chi phí. Để tự động hóa và linh hoạt trong công việc scaling, chúng ta sẽ có giải pháp là Auto Scaling Group.

2. Sơ lược về Auto Scaling Group
   
   ℹ️ **Information**: **Amazon EC2 Auto Scaling Group (ASG)** giúp tự động điều chỉnh số lượng EC2 instances theo nhu cầu của ứng dụng. ASG có thể tự động mở rộng (scale out) khi lưu lượng tăng, hoặc thu nhỏ (scale in) khi lưu lượng giảm, giúp tối ưu hóa tài nguyên và giảm chi phí. Nó cũng giúp đảm bảo tính sẵn sàng cao bằng cách phân phối instances qua nhiều Availability Zones để duy trì hoạt động liên tục ngay cả khi một phần của hệ thống gặp sự cố.

3. Các loại Scaling trong ASG
   
   Trong nội dung này, chúng ta sẽ tìm hiểu về các loại Scaling sau đây:
   - **Manual Scaling**: Người dùng tự tay điều chỉnh số lượng EC2 instances trong Auto Scaling Group dựa trên yêu cầu. Đây là phương pháp thủ công, không tự động dựa trên chỉ số cụ thể.
   
   - **Dynamic Scaling**: Tự động điều chỉnh số lượng instances dựa trên các chỉ số thời gian thực như CPU utilization, network traffic, hoặc custom metrics từ Amazon CloudWatch. Dynamic scaling có 3 chính sách chính:
     - **Target Tracking Scaling**: Duy trì một giá trị mục tiêu cho metric cụ thể
     - **Step Scaling**: Điều chỉnh dựa trên ngưỡng metric với các bước tăng/giảm khác nhau
     - **Simple Scaling**: Điều chỉnh dựa trên một ngưỡng đơn giản
   
   - **Scheduled Scaling**: Cho phép chúng ta cấu hình các thời gian cụ thể để tự động mở rộng hoặc thu nhỏ instances, ví dụ như tăng số lượng instances vào giờ cao điểm hoặc giảm xuống ngoài giờ làm việc. Phù hợp cho các trường hợp mà chúng ta đã biết trước mô hình lưu lượng truy cập.
   
   - **Predictive Scaling**: Sử dụng machine learning để dự đoán hoạt động bằng cách phân tích load data trong lịch sử để tìm các mẫu hàng ngày hoặc hàng tuần trong luồng lưu lượng truy cập. Nó sử dụng thông tin này để dự báo nhu cầu công suất trong tương lai để Amazon EC2 Auto Scaling có thể chủ động tăng công suất của Auto Scaling Group để phù hợp với dự kiến.

💡 **Pro Tip**: Kết hợp Predictive Scaling với Dynamic Scaling để đạt hiệu quả tối ưu - Predictive Scaling chuẩn bị trước cho các mẫu lưu lượng đã biết, trong khi Dynamic Scaling xử lý các biến động không lường trước được.

#### Launch Template

ℹ️ **Information**: **Launch Template** là một cấu hình chứa các thông số cần thiết để khởi chạy EC2 instances. Nó lưu trữ các chi tiết như loại instance, AMI (Amazon Machine Image), key pair, network settings, security groups, và các thông tin khác về cấu hình của EC2. Nhằm đơn giản hóa việc tạo instance, hỗ trợ trong việc tự động tạo mới các instance trong ASG.

🔒 **Security Note**: Launch Templates hỗ trợ quản lý phiên bản, cho phép bạn duy trì nhiều phiên bản cấu hình khác nhau và kiểm soát chặt chẽ các thay đổi, giúp tăng cường bảo mật và tuân thủ.

#### Elastic Load Balancer

ℹ️ **Information**: **Elastic Load Balancer (ELB)** là một dịch vụ giúp phân phối đều tải công việc (traffic) đến nhiều máy chủ hoặc instances để đảm bảo hệ thống hoạt động ổn định và tránh quá tải cho bất kỳ một máy chủ nào. Nó giúp tối ưu hiệu suất, tăng tính sẵn sàng và đảm bảo rằng nếu một máy chủ gặp sự cố, lưu lượng sẽ được chuyển hướng tới các máy chủ khác mà không ảnh hưởng đến người dùng.

AWS cung cấp ba loại Load Balancer:
- **Application Load Balancer (ALB)**: Tối ưu cho HTTP/HTTPS traffic, hoạt động ở tầng ứng dụng (Layer 7)
- **Network Load Balancer (NLB)**: Xử lý traffic ở tầng vận chuyển (Layer 4), phù hợp cho các ứng dụng yêu cầu hiệu suất cực cao
- **Gateway Load Balancer (GWLB)**: Dùng để triển khai và quản lý các thiết bị mạng ảo

#### Target Group

ℹ️ **Information**: **Target Group** là một thành phần của Elastic Load Balancer (ELB), dùng để xác định và quản lý các EC2 instances, IP addresses, Lambda functions, hoặc các container mà Load Balancer sẽ phân phối lưu lượng truy cập đến. Target Group cũng định nghĩa các health check để đảm bảo traffic chỉ được gửi đến các targets khỏe mạnh.

⚠️ **Warning**: Cấu hình health check không phù hợp có thể dẫn đến việc loại bỏ các instances khỏe mạnh hoặc giữ lại các instances có vấn đề trong Target Group. Hãy đảm bảo thiết lập các ngưỡng timeout, interval và threshold phù hợp với đặc điểm ứng dụng của bạn.
