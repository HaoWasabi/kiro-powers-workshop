---
title: "Tạo Target Group"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>4.1. </strong>"
---

#### Tạo Target Group

**ℹ️ Information**: Target Group là thành phần quan trọng trong kiến trúc Load Balancer của AWS, giúp định nghĩa các đích đến mà Load Balancer sẽ phân phối lưu lượng truy cập. Target Group có thể chứa các EC2 instances, IP addresses, Lambda functions hoặc các container.

Ở phần giao diện quản lý EC2, ở bảng điều hướng bên trái, hãy kéo xuống phần **Load Balancing**:

- Chọn **Target Groups**
- Chọn **Create target group**

![4.1.1](/images/4-setup-load-balancer/4.1.1.png)

Xuất hiện bảng **Specify group details**, hãy cấu hình như sau:

- Ở phần **Basic configuration**:
  - Choose a target type: **Instances**
  - Target group name: `FCJ-Management-TG`

![4.1.2](/images/4-setup-load-balancer/4.1.2.png)

- Tiếp tục trong phần **Basic configuration**:
  - Protocol: Port: **HTTP**, **5000**
  - IP address type: **IPv4**
  - VPC: **AutoScaling-Lab**
  - Protocol version: **HTTP1**

![4.1.3](/images/4-setup-load-balancer/4.1.3.png)

- Nhấn **Next**

![4.1.4](/images/4-setup-load-balancer/4.1.4.png)

**💡 Pro Tip**: Đảm bảo rằng port bạn chọn (5000) khớp với port mà ứng dụng của bạn đang lắng nghe trên EC2 instance. Điều này đảm bảo lưu lượng truy cập được định tuyến chính xác.

Tiếp theo chúng ta tiến hành **Register targets**:

- Ở phần **Available instances**:
  - Chọn instance **FCJ-Management**
  - Ports for the selected instances: **5000**
  - Chọn **Include as pending below**

![4.1.5](/images/4-setup-load-balancer/4.1.5.png)

- Ở phần **Review targets**:
  - Kiểm tra target đã được đăng ký
  - Chọn **Create target group**

![4.1.6](/images/4-setup-load-balancer/4.1.6.png)

**⚠️ Warning**: Sau khi tạo Target Group, trạng thái health check của các targets có thể hiển thị là "unhealthy" trong vài phút đầu tiên. Đây là hành vi bình thường khi hệ thống đang thực hiện kiểm tra sức khỏe ban đầu.

#### Kết quả

Chúng ta đã hoàn thành việc tạo Target Group. Chọn Target Group **FCJ-Management-TG** vừa khởi tạo để xem thông tin chi tiết.

![4.1.7](/images/4-setup-load-balancer/4.1.7.png)

**🔒 Security Note**: Target Group là một phần quan trọng trong việc thiết lập bảo mật cho ứng dụng của bạn. Khi kết hợp với Application Load Balancer, bạn có thể triển khai các quy tắc bảo mật như WAF (Web Application Firewall) để bảo vệ ứng dụng khỏi các mối đe dọa web phổ biến.

#### Hiểu về Target Group

Target Group là một thành phần quan trọng trong kiến trúc Elastic Load Balancing của AWS. Nó đóng vai trò là điểm đích cho các yêu cầu được gửi đến Load Balancer và định nghĩa cách thức kiểm tra sức khỏe của các mục tiêu.

**ℹ️ Information**: Target Group cho phép bạn nhóm các EC2 instance, IP address, Lambda function hoặc các container lại với nhau để Load Balancer có thể định tuyến lưu lượng đến chúng một cách hiệu quả.

#### Các loại Target Group

AWS hỗ trợ nhiều loại Target Group khác nhau tùy thuộc vào nhu cầu của ứng dụng:

1. **Instance-based Target Groups**: Định tuyến lưu lượng đến các EC2 instance (như chúng ta đã cấu hình).
2. **IP-based Target Groups**: Định tuyến lưu lượng đến các địa chỉ IP cụ thể, hữu ích khi làm việc với các container hoặc on-premises servers.
3. **Lambda-based Target Groups**: Định tuyến yêu cầu đến AWS Lambda functions.
4. **ALB-based Target Groups**: Định tuyến lưu lượng đến một Application Load Balancer khác.

#### Health Checks

Health Check là một tính năng quan trọng của Target Group, giúp đảm bảo lưu lượng chỉ được gửi đến các mục tiêu khỏe mạnh:

- **Path**: Đường dẫn mà Load Balancer sẽ gửi yêu cầu kiểm tra sức khỏe (mặc định là /).
- **Port**: Cổng mà Load Balancer sẽ sử dụng để kiểm tra sức khỏe.
- **Threshold**: Số lần kiểm tra liên tiếp thành công/thất bại để xác định trạng thái của mục tiêu.
- **Interval**: Khoảng thời gian giữa các lần kiểm tra sức khỏe.
- **Timeout**: Thời gian chờ phản hồi từ mục tiêu.

**💡 Pro Tip**: Cấu hình health check phù hợp là rất quan trọng để đảm bảo tính sẵn sàng cao của ứng dụng. Nên chọn một endpoint cụ thể trong ứng dụng của bạn để kiểm tra sức khỏe thay vì sử dụng trang chủ, vì endpoint này nên kiểm tra các thành phần quan trọng của ứng dụng như kết nối cơ sở dữ liệu.

#### Tích hợp với Auto Scaling Group

Target Group có thể được tích hợp với Auto Scaling Group để tự động đăng ký và hủy đăng ký các instance khi chúng được tạo hoặc kết thúc. Điều này đảm bảo rằng lưu lượng truy cập luôn được định tuyến đến các instance khỏe mạnh và sẵn sàng.

Trong phần tiếp theo, chúng ta sẽ tạo Application Load Balancer và kết nối nó với Target Group này để hoàn thiện cấu hình cân bằng tải cho ứng dụng của chúng ta.
