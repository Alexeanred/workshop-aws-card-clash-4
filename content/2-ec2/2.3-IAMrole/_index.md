---
title : "Configuring IAM role for ec2 instance"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

* To enable ec2 instance to communicate with other services, we need to create an IAM role for this ec2 instance.
* Go to IAM, select **Roles**.
* Select the **Create role** button.
![ec23](/ws1/images/2.prerequisite/2.13.png)
* Select AWS service.
* Select Service: EC2.
* Select **Next**.
![ec23](/ws1/images/2.prerequisite/2.14.png)
* Add these policies:
* ```AmazonS3FullAccess```
* ```AmazonSageMakerFullAccess```
* ```AmazonSSMManagedInstanceCore```
* ```CloudWatchAgentServerPolicy```
* Name the role: ```ssm-ec2-role```, you can name it whatever you want.
* Select **Create role**.
![ec23](/ws1/images/2.prerequisite/2.15.png)