---
title : "VPC endpoint configuration"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
## Introduction
- Normally, the communication between non-vpc and vpc service is over the internet.

- Like we need to assign a role to ec2 instance, and ec2 can go to the internet (with igw) to access things inside the endpoint resource over the internet.

- Communication method: via igw -> endpoint service. Disadvantages in security, cost.

- VPC gateway endpoint: supports S3 and dynamodb

- Amazon S3 has both interface endpoint and gateway endpoint.

- Gateway is free, interface is cost.

### S3 gateway endpoint
* Go to **VPC**:
* Select **Endpoints**:
* Select **create endpoint**.
* Name tag: ```s3-endpoint```.
* Select **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.s3**.
* Select: **Type: gateway**.
* Select **VPC default**.
![sg](/ws1/images/5.fwd/5.1_.png)
* Route tables: Select the route table of the subnet containing the ec2 instance.
* Policy: Full access.
* Select **Create endpoint**.
![sg](/ws1/images/5.fwd/5.2_.png)
* Wait a moment, the vpc endpoint will be created.
* We go to the route table of the subnet, we see a route automatically created.
![sg](/ws1/images/5.fwd/5.3.png)

### Cloudwatch gateway endpoint
* First, we need to create a **security group** for the VPC endpoint.
![sg](/ws1/images/5.fwd/5.5.png)

* Select **Endpoints**:
* Select **create endpoint**.
* Name tag: **cloudwatch-endpoint**.
* Select **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.logs**.
* Select **VPC default**.
![sg](/ws1/images/5.fwd/5.12.png)
* Select subnets: **us-east-1d**, subnet of ec2 instance.
* Select sg: **vpc endpoint sg** just created.
* Select **Full access** for Policy.
* Select **Create endpoint**.
* Interface endpoint will take longer to create than Gateway endpoint.

![sg](/ws1/images/5.fwd/5.13.png) ![sg](/ws1/images/5.fwd/5.4.png)