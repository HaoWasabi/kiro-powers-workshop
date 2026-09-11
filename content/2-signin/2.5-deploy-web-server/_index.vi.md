---
title: "Triển khai máy chủ web"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>2.5. </strong>"
---

Cài đặt node version manager (nvm) bằng cách nhập nội dung sau:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.1.png?featherlight=false&width=90pc)

Chạy ba dòng lệnh sau trong terminal của bạn để bắt đầu sử dụng nvm mà không cần phải đóng và mở lại terminal:
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

Sử dụng nvm để cài đặt Node.js bằng cách nhập nội dung sau vào dòng lệnh.

```bash
nvm install 20
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.2.png?featherlight=false&width=90pc)

Thực hiện clone repository code ứng dụng

```bash
git clone https://github.com/First-Cloud-Journey/000004-EC2.git
```

Đến thư mục của bài lab 000004-EC2

```bash
cd 000004-EC2
```

NPM là viết tắt của Node package manager là một công cụ tạo và quản lý các thư viện lập trình Javascript cho Node.js. Sử dụng npm init khởi tạo project sẽ tạo ra file package.json mẫu.

```bash
npm init
```
Bạn nên nhấn Enter liên tục để tạo giá trị mặc định

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.3.png?featherlight=false&width=90pc)

Cài đặt **pm2** trong Global, PM2 được sử dụng để quản lý và giám sát các ứng dụng Node.js đang chạy. Nó cho phép các ứng dụng chạy dưới nền.

```bash
npm install -g pm2
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.4.png?featherlight=false&width=90pc)

Tiếp theo, chúng ta định nghĩa lại câu script để chạy ứng dụng, chúng ta sẽ dùng **vim** để mở file pakage.json, trong phần scripts ở key **start**, gán cho nó value sau, điều này sẽ giúp ứng dụng của chúng ta chạy nền:

```bash
pm2 start app.js
```
Nhập lệnh sau đây để truy cập vào nội dung file package.json
```bash
nano package.json
```
Chỉnh sửa nội dung file như ảnh bên dưới:
![Image](/images/2-preparation/2.5-deploy-web-server/2.5.5.png?featherlight=false&width=90pc)

Tiếp tục dùng vim để vào file .env, sau đó nhập vào nội dung sau để thiết lập kết nối tới database.

```bash
DB_HOST='fcj-management-db-instance.c9yyuau4yxcz.eu-west-3.rds.amazonaws.com
DB_NAME='awsfcjuser'
DB_USER='admin'
DB_PASS='123Vodanhphai'
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.6.png?featherlight=false&width=90pc)

Tiến hành khởi chạy ứng dụng:

```bash
npm start
```

Lệnh **`pm2 status`** trong PM2 được sử dụng để hiển thị trạng thái hiện tại của tất cả các ứng dụng đang được quản lý bởi PM2. Khi bạn chạy lệnh này, chúng ta sẽ nhận được thông tin tổng quan về từng ứng dụng.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.7.png?featherlight=false&width=90pc)

Tiếp theo, chúng ta cần lấy được public DNS của instance để có thể truy cập được ứng dụng từ trình duyệt.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.8.png?featherlight=false&width=90pc)

Ứng dụng của chúng ta đã chạy ổn định.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.9.png?featherlight=false&width=90pc)

Tiếp theo chúng ta dùng câu lệnh **`pm2 startup`** để tiến hành cấu hình PM2 tự động khởi động lại các ứng dụng khi máy chủ khởi động lại.
Nó sẽ yêu cầu thiết lập Startup Script, hãy copy/paste command đó và chạy.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.10.png?featherlight=false&width=90pc)

Để đảm bảo các ứng dụng đang chạy được lưu lại và khởi động lại khi server khởi động lại, chúng ta cần chạy lệnh **`pm2 save`**. Lệnh này sẽ lưu trạng thái hiện tại của các tiến trình vào danh sách khởi động.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.11.png?featherlight=false&width=90pc)


