---
title : "Cấu hình ec2 instance"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

### Tạo ec2 instance
* Name: ```mysql-db```.
* Chọn AMI: **Amazon Linux 2023 AMI**.
* Chọn instance type: **t2.micro**.
![sg](/ws1/images/3.connect/3.1.png) 
* Chọn key pair nếu muốn.
* Chọn default VPC nếu muốn setup đơn giản hoặc có thể dùng custom VPC bạn đã tạo.
* Chọn subnet trong AZ: us-east-1d.
* Security Group inbound rules:
    * Do web server instance cần connect với MySQL, ta mở port 3306 và source 0.0.0.0/0.
    * Do cần ssh vào ec2 instance, ta mở port 22 và source 0.0.0.0/0.
![ec22](/ws1/images/3.connect/3.2.png)
* Chọn **Launch instance**.