---
title : "Cấu hình VPC endpoint"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
## Giới thiệu
- Thông thường khi giao tiếp giữa non-vpc và vpc service là qua internet.

- Như ta cần gán role cho ec2 instance, và ec2 đi được internet (có igw) để truy cập những thứ phía trong endpoint resource qua internet.

- Cách giao tiếp: qua igw -> endpoint service. Nhược điểm về security, cost.

- VPC gateway endpoint: hỗ trợ S3 và dynamodb

- Amazon S3 có cả interface endpoint và gateway endpoint.

- Gateway là free, interface là cost.

### S3 gateway endpoint
* Vào **VPC**:
* Chọn mục **Endpoints**:
* Chọn **create endpoint**.
* Name tag: ```s3-endpoint```.
* Chọn **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.s3**.
* Chọn: **Type: gateway**.
* Chọn **VPC default**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.1_.png) 
* Route tables: Chọn route table của subnet chứa ec2 instance.
* Policy: Full access.
* Chọn **Create endpoint**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.2_.png) 
* Đợi một chút, vpc endpoint sẽ được tạo. 
* Ta vào route table của subnet, ta thấy được 1 route tự động tao.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.3.png) 

### Cloudwatch gateway endpoint
* Đầu tiên, ta cần tạo **security group** cho VPC endpoint.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.5.png) 

* Chọn **Endpoints**: 
* Chọn **create endpoint**.
* Name tag: **cloudwatch-endpoint**.
* Chọn **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.logs**.
* Chọn **VPC default**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.12.png)
* Chọn subnets: **us-east-1d**, subnet của ec2 instance.
* Chọn sg: **vpc endpoint sg** vừa tạo.
* Chọn **Full access** cho Policy.
* Chọn **Create endpoint**.
* Interface endpoint khi tạo sẽ lâu hơn Gateway endpoint.

![sg](/workshop-aws-card-clash-4/images/5.fwd/5.13.png)
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.4.png) 

