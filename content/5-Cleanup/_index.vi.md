---
title: "Dọn dẹp tài nguyên"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>5. </strong>"
---

### Dọn dẹp tài nguyên

Sau khi thực hành xong bài workshop chúng ta tiến hành bước dọn dẹp tài nguyên

#### Xóa Auto Scaling Group

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, lướt xuống và chọn **Auto Scaling Groups**

- Chọn Auto Scaling Groups **FCJ-Management-ASG**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Delete**

![8.1](/images/8-Cleanup/8.1.png)

#### Xóa Load Balancer:

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, lướt xuống và chọn **Load Balancer**

- Chọn Load Balancer **FCJ-Management-LB**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Delete load balancer**

![8.2](/images/8-Cleanup/8.2.png)

#### Xóa Target Group:

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, lướt xuống và chọn **Target Group**

- Chọn Target Group **FCJ-Management-TG**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Delete**

![8.3](/images/8-Cleanup/8.3.png)

#### Xóa Launch Template:

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, lướt xuống và chọn **Launch Templates**

- Chọn Launch Templates **CJ-Management-TG**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Delete template**

![8.4](/images/8-Cleanup/8.4.png)

#### Xóa AMI:

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, lướt xuống và chọn **AMIs**

- Chọn AMI **FCJ-Management-AMI**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Deregister AMI**.

![8.5](/images/8-Cleanup/8.5.png)

#### Terminate EC2 instance

Ở giao diện quản lý EC2, ở thanh điều hướng bên trái, chọn **Instance**

- Chọn **FCJ-Management** instance
- Nhấn vào nút **Instance state** ở góc trên bên phải màn hình
- Chọn **Terminate (delete) instance**

![8.6](/images/8-Cleanup/8.6.png)

#### Xóa RDS Database

- Truy cập **RDS**
- Trên thanh điều hướng bên trái, chọn **Databases** instance
- Chọn database instance **fcj-management-db-instance** liên quan tới bài lab.
- Nhấn vào **Modify**.

![8.7](/images/8-Cleanup/8.7.png)

Ở phần Modify DB Instance, chúng ta kéo xuống dưới cùng

- Nhấp bỏ **Enable deletion protection**
- Nhấn **Continue**

![8.8](/images/8-Cleanup/8.8.png)

Tiếp tục ở phần Schedule modifications

- Chọn **Apply immediately**
- Nhấn **Modify DB instance**

![8.9](/images/8-Cleanup/8.9.png)

Tiến hành xoá DB instance

- Chọn database instance **fcj-management-db-instance**
- Nhấn vào nút **Actions** ở góc trên bên phải màn hình
- Chọn **Delete**

![8.10](/images/8-Cleanup/8.10.png)

- Chọn **I acknowledge that upon instance deletion, automated, including system snapshots and point -in-time recovery, will no longer be available**
- Điền **delete me**
- Nhấn **Delete**

![8.11](/images/8-Cleanup/8.11.png)

#### Xóa Subnet Group

- Chọn Subnet groups
- Chọn subnet group **fcj-management-subnet-group**
- Chọn **Delete**

![8.12](/images/8-Cleanup/8.12.png)
