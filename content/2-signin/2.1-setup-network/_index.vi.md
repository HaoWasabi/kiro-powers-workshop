---
title: "Đăng nhập Kiro IDE"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>2.1. </strong>"
---

#### Tạo VPC

Truy cập vào [AWS Management Console](https://aws.amazon.com/premiumsupport/knowledge-center/sign-in-console/)

- Tìm **VPC**
- Chọn **VPC**

![Image](/images/2-preparation/2.1-network/2.1.1.png?featherlight=false&width=90pc)

Trong giao diện **VPC**

- Chọn **Create VPC**

![Image](/images/2-preparation/2.1-network/2.1.2.png?featherlight=false&width=90pc)

Trong giao diện **Create VPC**

- Chọn **VPC and more**
- **Name**, nhập tên VPC của bạn. Trong bài lab này, ta đặt tên là **`AutoScaling-Lab`**
- **IPv4 CIDR block**, nhập **`10.0.0.0/16`**

![Image](/images/2-preparation/2.1-network/2.1.3.png?featherlight=false&width=90pc)

Chọn theo hướng dẫn

- Số lượng AZ là **3**
- Số lượng public subnet là **3**
- Số lượng private subnet là **3**
- Nat gateways chọn **None**

![Image](/images/2-preparation/2.1-network/2.1.4.png?featherlight=false&width=90pc)

Chọn theo hướng dẫn

- VPC endpoints chọn **None**
- Chọn **Create VPC**

![Image](/images/2-preparation/2.1-network/2.1.5.png?featherlight=false&width=90pc)

#### Cấu hình Auto-assign Public IP cho Public Subnets

**ℹ️ Information**: Để các EC2 instances trong public subnet có thể tự động nhận địa chỉ IP public khi khởi tạo, chúng ta cần bật tính năng auto-assign public IPv4 address.

- Chọn **Subnets**
- Chọn **public subnet**
- Chọn **Edit subnet settings**

![Image](/images/2-preparation/2.1-network/2.1.6.png?featherlight=false&width=90pc)

Chọn **Enable auto-assign public IPv4 address**. Sau đó Chọn **Save**

![Image](/images/2-preparation/2.1-network/2.1.7.png?featherlight=false&width=90pc)

Kiểm tra đã cấp phát thành công.

![Image](/images/2-preparation/2.1-network/2.1.8.png?featherlight=false&width=90pc)

**💡 Pro Tip**: Lặp lại các bước trên cho tất cả public subnet còn lại để đảm bảo tính nhất quán trong cấu hình mạng.

#### Tạo Security Group cho ứng dụng FCJ Management

**ℹ️ Information**: Security Group hoạt động như tường lửa ảo để kiểm soát lưu lượng truy cập vào và ra khỏi các tài nguyên AWS của bạn.

- Trong giao diện VPC, chọn **Security groups**
- Chọn **Create security group**

![Image](/images/2-preparation/2.1-network/2.1.9.png?featherlight=false&width=90pc)

Thực hiện cấu hình **Security Group**

- **Security group name**, nhập **`FCJ-Management-SG`**
- **Description**, nhập **`Security Group for FCJ Management`**
- **VPC**, chọn VPC vừa tạo: **AutoScaling-Lab**

![Image](/images/2-preparation/2.1-network/2.1.10.png?featherlight=false&width=90pc)

Cấu hình **Inbound rules**

- Đầu tiên cấu hình **SSH** port 22 với **Source: MyIP** để có thể truy cập an toàn vào instance
- Tiếp theo là **HTTP** port 80
- **Custom TCP** port **5000** dành cho ứng dụng **FCJ Management**
- **HTTPS** port 443

![Image](/images/2-preparation/2.1-network/2.1.11.png?featherlight=false&width=90pc)

**🔒 Security Note**: Việc giới hạn truy cập SSH chỉ từ địa chỉ IP của bạn (MyIP) là một biện pháp bảo mật quan trọng để giảm thiểu rủi ro xâm nhập trái phép.

Kiểm tra **Outbound rules** và chọn **Create security group**

![Image](/images/2-preparation/2.1-network/2.1.12.png?featherlight=false&width=90pc)

#### Tạo Security Group cho Database Instance

**ℹ️ Information**: Để tuân thủ nguyên tắc phân quyền tối thiểu (principle of least privilege), chúng ta nên tạo Security Group riêng cho Database instance.

Cấu hình **Security Group**:

- **Security Group name**, nhập **`FCJ-Mangement-DB-SG`**
- **Description**, nhập **`Security Group for DB instance`**
- Chọn VPC vừa tạo

Cấu hình **Inbound rules**:

- Chọn **Add rule**
- Chọn **MYSQL/Aurora** port 3306
- Sau đó chọn Source là **FCJ-Management-SG**

![Image](/images/2-preparation/2.1-network/2.1.13.png?featherlight=false&width=90pc)

**🔒 Security Note**: Bằng cách chỉ cho phép kết nối từ Security Group của ứng dụng (FCJ-Management-SG), chúng ta đảm bảo rằng chỉ các EC2 instances trong Security Group đó mới có thể kết nối đến database, tăng cường bảo mật cho dữ liệu.

Kiểm tra lại **Outbound rules** và cuối cùng bấm **Create security group**

![Image](/images/2-preparation/2.1-network/2.1.14.png?featherlight=false&width=90pc)

#### Tổng kết

Trong phần này, chúng ta đã hoàn thành việc thiết lập hạ tầng mạng cơ bản cho ứng dụng FCJ Management, bao gồm:

1. **VPC và Subnet**: Tạo môi trường mạng ảo riêng biệt với các subnet phân bố ở nhiều Availability Zone
2. **Internet Gateway**: Cho phép kết nối từ VPC ra internet
3. **Route Table**: Cấu hình định tuyến cho các subnet
4. **Security Groups**: Thiết lập các quy tắc bảo mật cho EC2 instances và RDS database

**💡 Pro Tip**: Việc phân chia subnet giữa các Availability Zone khác nhau giúp tăng tính sẵn sàng cao cho ứng dụng, đảm bảo hệ thống vẫn hoạt động ngay cả khi một AZ gặp sự cố.

Trong bước tiếp theo, chúng ta sẽ tiến hành khởi tạo EC2 instance để triển khai ứng dụng FCJ Management.
