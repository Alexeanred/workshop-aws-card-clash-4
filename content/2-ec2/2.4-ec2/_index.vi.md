---
title : "Cấu hình ec2 instance"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

* Sau khi có được sagemaker endpoint, ta sẽ đi cấu hình ec2 instance.
* Nếu bạn có theo dõi AWSCardClash2 thì mình đã tận dụng lại những cấu hình giống với bài thực hành đó.

### Tạo ec2 instance
* Name: ```ec2-streamlit```.
* Chọn AMI: **Amazon Linux 2023 AMI**.
* Chọn instance type: **t2.micro**.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.10.png) 
* Chọn key pair nếu muốn.
* Chọn default VPC nếu muốn setup đơn giản hoặc có thể dùng custom VPC bạn đã tạo.
* Chọn subnet trong AZ: us-east-1d.
* Security Group inbound rules:
    * Do streamlit application chạy port 8080, ta mở port 8080 và source 0.0.0.0/0.
    * Do cần ssh vào ec2 instance, ta mở port 22 và source 0.0.0.0/0.
![ec22](/workshop-aws-card-clash-4/images/2.prerequisite/2.11.png)
* Ở Advanced details, tại mục IAM instance profile: chọn role mình đã tạo.
![ec23](/workshop-aws-card-clash-4/images/2.prerequisite/2.12.png)
* Chọn **Launch instance**.



