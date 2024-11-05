---
title : "AWS backup"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

* If we want to create a test backup, we create an on-demand backup.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.10.png)
* If we want to create a Backup plan, we create a backup plan:
* Select **Create backup plan**.
* Select **Build a new plan**.
* Name: **backup-plan-for-streamlit-app**.
* **Schedule** section:
* Name: **daily-backup**.
* Backup Vault: **default**.
* Backup frequency: **daily**.
* Select start time: **10:00 Asia Bangkok**.
* Start within: **1 hour**
* Complete within: **2hours**
* Select **Create plan**.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.9.png)
* After finishing, select **Assign resources**:
* We will see an error with the S3 bucket: **The following buckets do not have versioning enabled: vinfast-car-images-150903. To assign buckets to a backup plan, turn on versioning for the bucket in S3 or select a different bucket with versioning enabled**.

* So we have to **turn on bucket versioning** first.

* Go to S3 bucket -> Select Properties -> Bucket Versioning -> Enable -> Save changes.

* Go back to assign s3 resources.

* However, because the AWSBackupServiceRolePolicyForBackup policy does not have a policy to access the S3 bucket, we have to create 2 new policies to backup and Restore S3.

![sg](/workshop-aws-card-clash-4/images/8.backup/8.2.png) ![sg](/workshop-aws-card-clash-4/images/8.backup/8.11.png) ### Assign resource: #### EFS ![sg](/workshop-aws-card-clash-4/images/8.backup/8.7.png) #### EBS ![sg](/workshop-aws-card-clash-4/images/8.backup/8. 6.png) ![sg](/workshop-aws-card-clash-4/images/8.backup/8.5.png) #### S3 ![sg](/workshop-aws-card-clash-4/images/8.backup/8.8.png) #### EC2 isntance ![sg](/workshop-aws-card-clash-4/images/8.backup/8.4.png) * We have successfully created backed up plan.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.3.png) ![sg](/workshop-aws-card-clash-4/images/8.backup/8.12.png)