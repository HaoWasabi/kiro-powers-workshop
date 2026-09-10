---
title: "Chuẩn bị các metric cho Predictive scaling"
date: "2024-01-01"
weight: 6
chapter: false
pre: "<strong>2.6. </strong>"
---

#### Chuẩn bị dữ liệu cho Predictive scaling

Bởi vì Predictive scaling cần phải có một lượng dữ liệu trong vòng hơn 2 ngày để có thể đưa ra được các dự đoán vào các ngày tiếp theo, mà ở đây chúng ta lại không có các dữ liệu đó cho nên là chúng ta sẽ cần phải chuẩn bị để giải lập một môi trường như thế.

#### Các bước chuẩn bị

Đầu tên là chúng ta sẽ tạo một folder mới với tên là `metric-preparation` và chuyển vào trong thư mục này

```bash
mkdir metric-preparation && cd metric-preparation
```

Sau đó là tải kịch bản để chuẩn bị các dữ liệu

```bash
curl -o prepare-metric-data.sh https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/prepare-metric-data.sh
```

![2.6.1.png](/images/2-preparation/2.6-prepare-metric-data/2.6.1.png)

Sau khi tải xong thì vào trong để thay đổi phần câu lệnh trong kịch bản này lại một xíu
```bash
vim prepare-metric-data.sh
```
Chỉnh sửa biến thời gian thành:
```bash
time=$(date -d "$((5*i)) minutes ago")
```

![2.6.2.png](/images/2-preparation/2.6-prepare-metric-data/2.6.2.png)

Sau khi chỉnh sửa xong thì giờ chúng ta tiến hành tải các dữ liệu chưa qua xử lý, đó là lý do vì sao mà chúng ta cần tải kịch bản xử lý dữ liệu này trước. Trước tiên là metric cho các instances.

```bash
curl -o metric-cpu.json https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/metric-cpu.json
```

![2.6.3.png](/images/2-preparation/2.6-prepare-metric-data/2.6.3.png)

Tiếp theo là dữ liệu cho CPU

```bash
curl -o metric-cpu.json https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/metric-cpu.json
```

![2.6.4.png](/images/2-preparation/2.6-prepare-metric-data/2.6.4.png)

Tiến hành sửa đổi lần lượt 2 loại dữ liệu này, đầu tiên là cho CPU trước

```bash
bash prepare-metric-data.sh metric-cpu.json FCJ-Management-ASG && cat metric-cpu.json
```

![2.6.5.png](/images/2-preparation/2.6-prepare-metric-data/2.6.5.png)

Tiếp theo là cho instances

```bash
bash prepare-metric-data.sh metric-instances.json FCJ-Management-ASG && cat metric-instances.json
```

![2.6.6.png](/images/2-preparation/2.6-prepare-metric-data/2.6.6.png)

{{% notice note %}}
Ở 2 lệnh ở trên đều xuất hiện tham số **FCJ-Management-ASG** thì nó chính là tên của Auto Scaling Group mà chúng ta sẽ tạo về sau, nên về sau thì bạn cần sẽ phải tạo ASG với cùng tên như thế. Còn không thì bạn nên thay một cái tên khác từ bây giờ.
{{% /notice %}}

#### Tải dữ liệu lên CloudWatch

Trong Amazon Linux 2023, và dùng đúng AMI thì AWS CLI đã được cài đặt sẵn ở bên trong, lúc này thì chúng ta chỉ cần lấy ra để cấu hình lại các crediential là được. Nên nhớ là bạn phải có một IAM User đủ quyền để tải dữ liệu lên CloudWatch hoặc ít nhất là đủ quyền để làm bài workshop này.

Vào trang IAM, vào thông tin IAM User và ấy Access Key Id và Serect Access Key, nếu chưa có thì tạo mới.

```bash
aws configure
```

Và tiến hành cấu hình

![2.6.7.png](/images/2-preparation/2.6-prepare-metric-data/2.6.7.png)

Sau đó là tải 2 file dữ liệu mà chúng ta đã chuẩn bị trước đó lên trên CloudWatch

```bash
aws cloudwatch put-metric-data --namespace 'FCJ Management Custom Metrics' --metric-data file://metric-cpu.json
```

```bash
aws cloudwatch put-metric-data --namespace 'FCJ Management Custom Metrics' --metric-data file://metric-instances.json
```

![2.6.8.png](/images/2-preparation/2.6-prepare-metric-data/2.6.8.png)

#### Kiểm tra

Cuối cùng thì chúng ta sẽ vào trong CloudWatch để kiểm tra kết quả

- Tìm **CloudWatch**
- Click để vào trong CloudWatch Console

![2.6.9.png](/images/2-preparation/2.6-prepare-metric-data/2.6.9.png)

Trong giao diện Console của **CloudWatch**

- Chọn **All metrics**
- Chọn **FCJ Management Custom Metrics**

![2.6.10.png](/images/2-preparation/2.6-prepare-metric-data/2.6.10.png)

Chọn tiếp **AutoScalingGroupName**

![2.6.11.png](/images/2-preparation/2.6-prepare-metric-data/2.6.11.png)

Chọn tiếp 2 thông số như trên hình, chờ một khoảng thời gian để nhận được kết quả.

![2.6.12.png](/images/2-preparation/2.6-prepare-metric-data/2.6.12.png)

{{% notice note %}}
Chúng ta sẽ phải chờ khoảng **30 phút** hoặc hơn để cho CloudWatch xử lý xong. Thay vì chờ thì chúng ta nên làm tiếp các phần tiếp theo.
{{% /notice %}}

#### Hiểu về dữ liệu đã tải lên

Dữ liệu mà chúng ta đã tải lên CloudWatch bao gồm hai loại metric quan trọng:

1. **CPU Utilization**: Đây là dữ liệu về mức sử dụng CPU của Auto Scaling Group theo thời gian. Metric này giúp AWS Predictive Scaling hiểu được mô hình sử dụng tài nguyên của ứng dụng và dự đoán nhu cầu trong tương lai.

2. **Instance Count**: Đây là dữ liệu về số lượng instance đã được sử dụng trong quá khứ. Metric này giúp hệ thống hiểu được cách Auto Scaling Group đã phản ứng với các thay đổi về tải trong quá khứ.

Hai loại dữ liệu này cung cấp cho AWS Predictive Scaling thông tin cần thiết để:

- Phân tích mô hình sử dụng tài nguyên theo thời gian
- Dự đoán nhu cầu tài nguyên trong tương lai
- Tự động điều chỉnh capacity trước khi nhu cầu thực sự tăng cao

#### Lợi ích của Predictive Scaling

Việc sử dụng Predictive Scaling mang lại nhiều lợi ích so với chỉ sử dụng Dynamic Scaling thông thường:

1. **Chủ động thay vì phản ứng**: Predictive Scaling tăng/giảm capacity trước khi nhu cầu thực sự thay đổi, giúp tránh tình trạng ứng dụng bị quá tải.

2. **Tối ưu chi phí**: Bằng cách dự đoán chính xác nhu cầu, bạn chỉ sử dụng đúng số lượng tài nguyên cần thiết vào đúng thời điểm.

3. **Cải thiện trải nghiệm người dùng**: Người dùng không phải đợi hệ thống scale up khi tải tăng cao đột ngột.

4. **Xử lý tốt các mô hình tải có tính chu kỳ**: Đặc biệt hiệu quả cho các ứng dụng có mô hình sử dụng theo giờ, theo ngày hoặc theo mùa.

Trong phần tiếp theo, chúng ta sẽ cấu hình Auto Scaling Group để sử dụng các metric này cho Predictive Scaling.
