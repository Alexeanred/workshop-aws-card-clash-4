---
title : "Create EFS"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

* First, we create **security group** for efs first:
* Inbound rules: Type: **NFS** Port: **2049** Source: **sg of ec2 instance** we use.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.1.png)

* Next, we go to **Elastic File System**:
* Select **Create file system**:
* Name: **fs-streamlit-app**.
* Select **default vpc**.
* Select **Create**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.2.png)
* Go to **Network**:
* Select **Manage**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.3.png)
* In the AZs in the region:
* Delete **default sg**, select **efs-sg** created before.
* If you want to add AZ, select **Add mount target**.
* Select **Save**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.4.png)

* Select **Attach**.

Copy: ```sudo mount -t efs -o tls fs-0b4552f241db29dae:/ efs```
![sg](/workshop-aws-card-clash-4/images/7.efs/7.5.png)

* Go to the ec2 instance you want to mount, paste it to mount efs.

* Check with the command ```df -h```.

* You can practice sharing files between ec2 instances.
* While practicing, I accidentally used the command mv file app.py and log file into efs and couldn't find it and I had to reconfigure =)).