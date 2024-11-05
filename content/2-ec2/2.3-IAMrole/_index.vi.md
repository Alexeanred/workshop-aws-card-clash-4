---
title : "Cấu hình IAM role cho ec2 instance"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

* Để ec2 instance có thể giao tiếp với các services khác, ta cần tạo IAM role cho ec2 instance này.
* Vào IAM, chọn mục **Roles**.
* Chọn nút **Create role**.
![ec23](/workshop-aws-card-clash-4/images/2.prerequisite/2.13.png)
* Chọn AWS service.
* Chọn Service: EC2.
* Chọn **Next**.
![ec23](/workshop-aws-card-clash-4/images/2.prerequisite/2.14.png)
* Thêm các policies này vào:
* ```AmazonS3FullAccess```
* ```AmazonSageMakerFullAccess```
* ```AmazonSSMManagedInstanceCore```
* ```CloudWatchAgentServerPolicy```
* Đặt tên role: ```ssm-ec2-role```, có thể đặt tên nào bạn muốn.
* Chọn **Create role**.
![ec23](/workshop-aws-card-clash-4/images/2.prerequisite/2.15.png)


