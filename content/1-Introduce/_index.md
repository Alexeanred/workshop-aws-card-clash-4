---
title : "Introduction"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
![arch](/workshop-aws-card-clash-4/images/kientruc.png)

## Architectural idea:
* **Users will classify images of Vinfast car models through the following architecture:**
* Web Server instance is an ec2 instance running streamlit application as the user interface, we will use amazon sagamaker endpoint to use as an image classification model.
* amazon EFS acts as a shared file storage place between ec2 instances running streamlit if we create more.
* amazon S3 in this architecture acts as a storage place for images that users have uploaded to streamlit to serve data analysis later.
* This database instance will be installed on an ec2 instance, we will install mysql database on linux.
* We will configure an additional amazon cloudwatch log group to manage the logs of the streamlit application.

* At the same time, we will also create a VPC endpoint for S3 and cloudwatch to make logging or data recording via the aws backbone network more secure.

* AWS backup plays a role in backing up and restoring components in this architecture such as ec2 instance, db instance, efs, s3.

* At the same time, we also practice accessing ec2 instance via Session manager of System Manager to test the successful creation of VPC endpoint for S3 and cloudwatch.
## Service introduction:

* **Amazon EC2 (Elastic Compute Cloud)**:
* Amazon EC2 provides a flexible virtual server infrastructure, helping you easily create instances (virtual servers) to run the necessary applications and services.

* EC2 instances will be used to run the Streamlit application to provide a user interface for image classification. Another instance will run the MySQL database, managing and storing image information.

* **Amazon SageMaker**:

* Amazon SageMaker is a machine learning service that lets you build, train, and deploy machine learning models at scale without managing infrastructure.

* You will use SageMaker to deploy an endpoint for the image classification model, allowing EC2 instances to call this endpoint to classify images of VinFast cars based on user-uploaded images.

* **Amazon EFS (Elastic File System)**:

* Amazon EFS is a scalable, shared file storage system that can be accessed by multiple EC2 instances at the same time. EFS automatically scales to store more files as demand increases.

* EFS will serve as a storage for shared files, such as the source code or log files of a Streamlit application, and allow multiple EC2 instances to access them concurrently, helping to maintain consistency as the system scales.
* **Amazon S3 (Simple Storage Service)**
* Amazon S3 is a highly scalable, durable, and secure object storage service that allows you to store and retrieve data whenever you need it.
* S3 will be used to store images uploaded by users from the Streamlit application, allowing for long-term management and analysis of image data.
* **Amazon CloudWatch Logs**
* Amazon CloudWatch Logs helps you monitor and store logs from AWS services and applications running on AWS.
* CloudWatch Logs will be used to collect and track logs from the Streamlit application, making it easy to monitor operations and detect potential issues.
* **Amazon VPC Endpoint**
* VPC Endpoint allows resources in your VPC to connect to AWS services without accessing them over the Internet, enhancing security and optimizing connectivity.
* You will create VPC Endpoints for S3 and CloudWatch, allowing EC2 instances to access these services without an Internet Gateway, ensuring that data is transmitted over the AWS internal network.
* **AWS Backup**
* AWS Backup provides comprehensive, automated backup and recovery services for AWS resources, ensuring that your data and configurations are always securely backed up.
* AWS Backup will help you back up important components in the architecture, including EC2 instances, MySQL databases on EC2, EFS, and objects in S3. This makes it easy to restore data in the event of a failure.
* **AWS Systems Manager - Session Manager**
* Session Manager is part of AWS Systems Manager, allowing you to access EC2 instances securely and without the need for an SSH connection or opening a port to the internet. This is a highly secure tool, convenient for managing and monitoring instances without having to configure key pairs or public IPs.
* In the architecture, Session Manager will be used to access EC2 instances without the need for a public network, especially useful when you have deployed VPC Endpoints for S3 and CloudWatch.