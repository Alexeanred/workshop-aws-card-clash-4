---
title : "Configure ec2 instance"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

* After getting the sagemaker endpoint, we will configure the ec2 instance.

* If you have followed AWSCardClash2, I have reused the same configurations as in that exercise.

### Create ec2 instance
* Name: ```ec2-streamlit```.
* Select AMI: **Amazon Linux 2023 AMI**.
* Select instance type: **t2.micro**.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.10.png)
* Select key pair if you want.
* Select default VPC if you want a simple setup or you can use the custom VPC you created.
* Select subnet in AZ: us-east-1d.
* Security Group inbound rules:
* Because streamlit application runs on port 8080, we open port 8080 and source 0.0.0.0/0.
* Because we need to ssh into ec2 instance, we open port 22 and source 0.0.0.0/0.
![ec22](/workshop-aws-card-clash-4/images/2.prerequisite/2.11.png)
* In Advanced details, in IAM instance profile: select the role I created.
![ec23](/workshop-aws-card-clash-4/images/2.prerequisite/2.12.png)
* Select **Launch instance**.