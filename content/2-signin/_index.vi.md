---
title: "Các bước chuẩn bị"
date: "2024-01-01"
weight: 2
chapter: false
pre: "<strong>2. </strong>"
---

#### Tổng quan

Trong bài thực hành này, chúng ta cần chuẩn bị một số dịch vụ để có thể tiến hành triển khai ứng dụng FCJ Management sử dụng Amazon EC2 Auto Scaling cùng với Elastic Load Balancing. 

**ℹ️ Information**: Kiến trúc triển khai của ứng dụng FCJ Management sẽ tận dụng các dịch vụ quản lý của AWS để đảm bảo tính sẵn sàng cao và khả năng mở rộng linh hoạt.

![Create VPC](/images/2-preparation/diagram0006.png)

#### Nội dung

1. [Thiết lập hạ tầng mạng](2.1-setup-network/)
2. [Khởi tạo EC2 instance](2.2-launch-ec2-instance/)
3. [Khởi tạo Database instance với Amazon RDS](2.3-launch-db-instance/)
4. [Cài đặt dữ liệu cho database](2.4-add-data-to-db/)
5. [Triển khai máy chủ web](2.5-deploy-web-server/)
6. [Chuẩn bị metric cho Predictive scaling](2.6-prepare-metrics-for-predictive-scaling/)

**💡 Pro Tip**: Việc chuẩn bị đầy đủ các thành phần cơ sở hạ tầng trước khi triển khai ứng dụng sẽ giúp quá trình triển khai diễn ra suôn sẻ và giảm thiểu các vấn đề có thể phát sinh.

#### Các dịch vụ AWS sử dụng trong bài thực hành

Trong bài thực hành này, chúng ta sẽ sử dụng các dịch vụ AWS sau:

1. **Amazon VPC (Virtual Private Cloud)**: Tạo môi trường mạng ảo riêng biệt để triển khai các tài nguyên AWS.

2. **Amazon EC2 (Elastic Compute Cloud)**: Cung cấp máy chủ ảo để chạy ứng dụng FCJ Management.

3. **Amazon RDS (Relational Database Service)**: Dịch vụ cơ sở dữ liệu quan hệ được quản lý để lưu trữ dữ liệu ứng dụng.

4. **Amazon EC2 Auto Scaling**: Tự động điều chỉnh số lượng EC2 instances dựa trên nhu cầu thực tế.

5. **Elastic Load Balancing (ELB)**: Phân phối lưu lượng truy cập đến giữa nhiều EC2 instances.

6. **Amazon CloudWatch**: Giám sát tài nguyên và ứng dụng AWS, thu thập và theo dõi các metrics.

7. **AWS Systems Manager**: Quản lý cấu hình và tự động hóa các tác vụ trên EC2 instances.

#### Yêu cầu kỹ thuật

Để hoàn thành bài thực hành này, bạn cần:

1. **Tài khoản AWS**: Có quyền truy cập vào AWS Management Console với đầy đủ quyền để tạo và quản lý các dịch vụ được liệt kê ở trên.

2. **Kiến thức cơ bản**: Hiểu biết cơ bản về EC2, VPC, và các dịch vụ AWS khác.

3. **Trình duyệt web**: Chrome, Firefox, hoặc Edge phiên bản mới nhất.

4. **Công cụ kết nối SSH** (tùy chọn): Như PuTTY (Windows) hoặc Terminal (macOS/Linux) để kết nối với EC2 instances.

5. **Kiên nhẫn và tỉ mỉ**: Một số bước trong bài thực hành có thể mất thời gian để hoàn thành, đặc biệt là khi chờ các tài nguyên được khởi tạo.

**🔒 Security Note**: Trong môi trường thực tế, bạn nên tuân thủ nguyên tắc đặc quyền tối thiểu (principle of least privilege) khi cấp quyền cho người dùng AWS IAM.

