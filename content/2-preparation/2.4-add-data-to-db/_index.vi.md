---
title: "Cài đặt dữ liệu cho Database"
date: "2024-01-01"
weight: 4
chapter: false
pre: "<strong>2.4. </strong>"
---

Lấy **Public IP address** của EC2 instance

![Image](/images/2-preparation/2.4-data-for-db/2.4.1.png?featherlight=false&width=90pc)

Sử dụng **MobaXterm** để kết nối SSH vào instance qua port 22.

- Chọn **Session**
- Chọn **SSH**
- **Remote host**, nhập **Public IPv4 address** mới lấy của instance
- **Specify username**, nhập **ec2-user**
- Kiểm tra port 22
- Chọn **Advanced SSH settings**
- Chọn **Use private key** và chọn **keypair** của instance.
- Chọn **OK**

![Image](/images/2-preparation/2.4-data-for-db/2.4.2.png?featherlight=false&width=90pc)

Kết quả sau khi SSH

![Image](/images/2-preparation/2.4-data-for-db/2.4.3.png?featherlight=false&width=90pc)

Chúng ta sử dụng git để clone source code. Trước hết, cài đặt git bằng lệnh sau:

```bash
sudo yum install git
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.4.png?featherlight=false&width=90pc)

Cài đặt MySQL command-line client

```bash
sudo dnf install mariadb105
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.5.png?featherlight=false&width=90pc)

Kiểm tra cài đặt thành công

```
mysql --version
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.6.png?featherlight=false&width=90pc)

Kết nối MySQL command-line client (unencrypted)

- Đối với tham số -h, hãy thay thế DNS name (endpoint) cho DB instance, bạn có thể lấy DNS name ở trong console chi tiết của RDS bạn đã tạo - Đối với tham số -P, hãy thay thế port cho DB instance. (3306)
- Đối với tham số -u, thay bằng master user lúc bạn tạo RDS
- Sau khi chạy lệnh thì nhập vào master user password mà bạn đã đặt khi tạo RDS

```bash
mysql -h fcj-management-db-instance.cdysiiecu90g.ap-southeast-1.rds.amzonaws.com -P 3306 -u admin -p
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.7.png?featherlight=false&width=90pc)

Kết nối DB instance thành công. Tiến hành kiểm tra các database trong instance bằng lệnh sau sẽ in ra danh sách tất cả các cơ sở dữ liệu.

```sql
SHOW DATABASES;
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.8.png?featherlight=false&width=90pc)

Chọn cơ sở dữ liệu để thực hiện các thay đổi đối với nó bằng cách sử dụng USE, hãy dùng initial database lúc bạn tạo RDS .

```sql
USE "tên database";
```

Thực hiện tạo bảng trong database awsuser bằng lệnh CREATE TABLE.

```sql
CREATE TABLE `awsfcjuser`.`user` ( `id` INT NOT NULL AUTO_INCREMENT , `first_name` VARCHAR(45) NOT NULL , `last_name` VARCHAR(45) NOT NULL , `email` VARCHAR(45) NOT NULL , `phone` VARCHAR(45) NOT NULL , `comments` TEXT NOT NULL , `status` VARCHAR(10) NOT NULL DEFAULT 'active' , PRIMARY KEY (`id`)) ENGINE = InnoDB;
```

Chèn thông tin vào trong bảng dữ liệu bằng lệnh **INSERT INTO**

```sql
INSERT INTO `user`
(`id`, `first_name`,  `last_name`,    `email`,                 `phone`,         `comments`, `status`) VALUES
(NULL, 'Amanda',      'Nunes',        'anunes@ufc.com',        '012345 678910', '',          'active'),
(NULL, 'Alexander',   'Volkanovski',  'avolkanovski@ufc.com',  '012345 678910', '',          'active'),
(NULL, 'Khabib',      'Nurmagomedov', 'knurmagomedov@ufc.com', '012345 678910', '',          'active'),
(NULL, 'Kamaru',      'Usman',        'kusman@ufc.com',        '012345 678910', '',          'active'),
(NULL, 'Israel',      'Adesanya',     'iadesanya@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Henry',       'Cejudo',       'hcejudo@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Valentina',   'Shevchenko',   'vshevchenko@ufc.com',   '012345 678910', '',          'active'),
(NULL, 'Tyron',       'Woodley',      'twoodley@ufc.com',      '012345 678910', '',          'active'),
(NULL, 'Rose',        'Namajunas ',   'rnamajunas@ufc.com',    '012345 678910', '',          'active'),
(NULL, 'Tony',        'Ferguson ',    'tferguson@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Jorge',       'Masvidal ',    'jmasvidal@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Nate',        'Diaz ',        'ndiaz@ufc.com',         '012345 678910', '',          'active'),
(NULL, 'Conor',       'McGregor ',    'cmcGregor@ufc.com',     '012345 678910', '',          'active'),
(NULL, 'Cris',        'Cyborg ',      'ccyborg@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Tecia',       'Torres ',      'ttorres@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Ronda',       'Rousey ',      'rrousey@ufc.com',       '012345 678910', '',          'active'),
(NULL, 'Holly',       'Holm ',        'hholm@ufc.com',         '012345 678910', '',          'active'),
(NULL, 'Joanna',      'Jedrzejczyk ', 'jjedrzejczyk@ufc.com',  '012345 678910', '',          'active');
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.9.png?featherlight=false&width=90pc)

Sử dụng lệnh SELECT để hiển thị bảng:

```sql
SELECT * FROM user;
```

![Image](/images/2-preparation/2.4-data-for-db/2.4.10.png?featherlight=false&width=90pc)

Sử dụng **exit** đề rời khỏi. Nếu không thể ngắt kết nối với DB instance, hãy dùng tổ hợp phím Ctrl+C


