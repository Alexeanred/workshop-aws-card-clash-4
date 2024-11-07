---
title : "Session Manager"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

- ​​System managers is a **non-vpc service**, can ssh public subnet ec2 instance but if private then need **vpc endpoint configuration**.

- Another way is to use Bastion host but we are using VPC endpoint so continue to configure =))

- To be able to use Session manager in System managers to access private ec2 instance, we need to configure the following 3 vpc endpoints:
    * **ssm endpoint**
    * **ssmmessages endpoint**
    * **ec2messages endpoint**
* We will configure one, the remaining 2 do the same but the service name is different:
* Select **Endpoints**:
* Select **create endpoint**.
* Name tag: **ssm-endpoint**.

* Select **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.ssm**.
* Select **VPC default**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.6.png)
* Select subnets: **us-east-1d**, subnet of ec2 instance.
* Select sg: **vpc endpoint sg** just created.
* Select **Full access** for Policy.
* Select **Create endpoint**.
* Interface endpoint will take longer to create than Gateway endpoint.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.7.png)
* After creating 3 VPC endpoints for System manager, go to System manager:
* Select **Session Manager**:
* Select **Start a session**.
* Select the ec2 instance you want to connect to.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.14.png)
* After accessing the terminal of ec2 instances, we need to remove the internet gateway route from the route table to see if we can communicate with the services via the vpc endpoint.
* Go to **Route table**, select the route table associated with the subnet containing the ec2 instance.
* Select **Edit routes**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.15.png) 
* We see there is an **Internet Gateway** route and we select **remove** and **Save changes**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.16.png)
* When successfully logged in, we run the commands to test the vpc endpoint of s3 and cloudwatch.
* First, we need to log in with the user ec2-user.
* Next, we try to list the bucket in the S3 bucket to see if it works, if it works, we succeed.
* Next, we try to write a log to test if the cloudwatch log is received.
```
sudo su - ec2-user
aws s3 ls s3://vinfast-car-images-150903
echo "Manual log test $(date)" >> /home/ec2-user/test2.log
```
* The objects in the vinfast-images bucket have been listed.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.8.png)
* There is a new event log written to it, the log we just wrote.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.10.png)