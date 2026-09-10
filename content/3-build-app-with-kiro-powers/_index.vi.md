---
title: "Xây dựng ứng dụng với Kiro Powers"
date: "2024-01-01"
weight: 3
chapter: false
pre: "<strong>3. </strong>"
---

#### AMIs và Launch Template

**ℹ️ Information**: AMIs (Amazon Machine Images) lưu trữ các thông tin như hệ điều hành, ứng dụng, và cấu hình của EC2 instance mà chúng được tạo từ đó. Việc tạo AMI đảm bảo rằng khi khởi tạo máy chủ mới, tất cả các instance đều giống nhau và có thể hoạt động ngay lập tức.

Launch template là một công cụ mà chúng ta dùng để cấu hình việc khởi tạo các EC2 instance mới thông qua AMI được chỉ định, loại instance, cấu hình mạng, và các tùy chọn bảo mật. Khi cần khởi tạo một hoặc nhiều máy chủ giống nhau, chúng ta chỉ cần sử dụng launch template đã được cấu hình sẵn.

#### Thiết lập Launch Templates

#### Tạo Amazon Machine Images (AMIs) từ EC2

Ở trong phần giao diện quản lý **EC2**, ở bảng điều hướng bên trái:

- Chọn **Instances**
- Chọn instance **FCJ-Management**
- Chọn **Actions**
- Chọn **Image and templates**
- Ấn **Create image**

![3.1](/images/3-create-launch-template/3.1.png)

Trong bảng cấu hình **Create AMI**, điền các thông tin sau:

- Image name: `FCJ-Management-AMI`
- Image description: `AMI for FCJ-Management`
- Ấn **Create Image**

![3.2](/images/3-create-launch-template/3.2.png)

Sau khi tạo AMI, kiểm tra AMI vừa tạo:

- Chọn **AMIs** trong bảng điều hướng bên trái
- Tìm và chọn **FCJ-Management-AMI**

![3.3](/images/3-create-launch-template/3.3.png)

**⚠️ Warning**: Quá trình khởi tạo AMI sẽ mất khoảng 3 phút. Sau quá trình khởi tạo, **Status** của AMI sẽ chuyển sang trạng thái **Available**.

![3.4](/images/3-create-launch-template/3.4.png)

**💡 Pro Tip**: Chúng ta vừa hoàn thành việc tạo một Image để lưu cấu hình của EC2 instance. AMI này sẽ giúp đảm bảo tính nhất quán khi triển khai nhiều instance.

#### Tạo Launch Templates

Ở giao diện quản lý **EC2**, ở bảng điều hướng bên trái:

- Chọn **Launch Templates**
- Chọn **Create launch template**

![3.5](/images/3-create-launch-template/3.5.png)

Trong bảng **Create launch template**, điền các thông tin sau:

- Ở phần **Launch template name and description**:
  - Launch template name: `FCJ-Management-template`
  - Template version description: `Template for FCJ Management`

![3.6](/images/3-create-launch-template/3.6.png)

- Ở phần **Application and OS Image (Amazon Machine Image)**:
  - Chọn **My AMIs**
  - Chọn **Owned by me**
  - Chọn AMI đã tạo **FCJ-Management-AMI**

![3.7](/images/3-create-launch-template/3.7.png)

- Ở phần **Instance type**:
  - Chọn loại Instance **t2.micro**
- Ở phần **Key pair (login)**:
  - Chọn **Key pair name** có tên là **fcj-key**

![3.8](/images/3-create-launch-template/3.8.png)

- Ở phần **Network settings**:
  - Chọn subnet public **AutoScaling-Lab-public-ap-southeast-1a**
  - Chọn **Select existing security group**
  - Chọn security group **FCJ-Management-SG**
  - Cuối cùng, chọn **Create launch template**

![3.9](/images/3-create-launch-template/3.9.png)

**🔒 Security Note**: Việc chọn đúng security group là rất quan trọng để đảm bảo các quy tắc truy cập phù hợp cho instance của bạn. Hãy đảm bảo security group chỉ mở các cổng cần thiết.

#### Kết quả

Kiểm tra Launch Template vừa tạo:

- Chọn **FCJ-Management-template**

![3.10](/images/3-create-launch-template/3.10.png)

- Ở đây chúng ta có thể xem lại cấu hình Launch Template đã tạo

![3.11](/images/3-create-launch-template/3.11.png)

**💡 Pro Tip**: Launch Template vừa tạo có thể được sử dụng để khởi tạo các EC2 instance riêng lẻ hoặc trong Auto Scaling Group. Bạn cũng có thể tạo nhiều phiên bản của cùng một template để quản lý các thay đổi theo thời gian.

#### Tùy chỉnh nâng cao cho Launch Template

Ngoài các cấu hình cơ bản, Launch Template còn cho phép bạn tùy chỉnh nhiều thông số nâng cao để tối ưu hóa hiệu suất và chi phí cho các EC2 instance trong Auto Scaling Group.

##### Sử dụng User Data

User Data là một tính năng mạnh mẽ cho phép bạn chạy script khi instance khởi động.
