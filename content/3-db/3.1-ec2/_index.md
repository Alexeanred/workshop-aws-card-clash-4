---
title : "EC2 instance configuration"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1. </b> "
---

### Create ec2 instance
* Name: ```mysql-db```.
* Select AMI: **Amazon Linux 2023 AMI**.
* Select instance type: **t2.micro**.
![sg](/workshop-aws-card-clash-4/images/3.connect/3.1.png)
* Select key pair if you want.
* Select default VPC if you want simple setup or can use custom VPC you created.
* Select subnet in AZ: us-east-1d.
* Security Group inbound rules:
* Because web server instance needs to connect to MySQL, we open port 3306 and source 0.0.0.0/0.
* Since we need to ssh into ec2 instance, we open port 22 and source 0.0.0.0/0.
![ec22](/workshop-aws-card-clash-4/images/3.connect/3.2.png)
* Select **Launch instance**.