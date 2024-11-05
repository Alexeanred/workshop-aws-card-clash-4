---
title : "AWS backup"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

* Nếu ta muốn tạo thử 1 backup, ta tạo 1 on-demand backup.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.10.png) 
* Còn nếu ta muốn tạo 1 kế hoạch Backup, ta tạo 1 backup plan:
    * Chọn **Create backup plan**.
    * Chọn **Build a new plan**.
    * Name: **backup-plan-for-streamlit-app**.
    * Phần **Schedule**:
    * Name: **daily-backup**.
    * Backup Vault: **default**.
    * Backup frequency: **daily**.
    * Chọn start time: **10:00 Asia Bangkok**.
    * Start within: **1 hour**
    * Complete within: **2hours**
    * Chọn **Create plan**.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.9.png) 
* Sau khi xong, chọn **Assign resources**:
* Ta sẽ thấy xảy ra lỗi với S3 bucket: **The following buckets do not have versioning enabled: vinfast-car-images-150903. To assign buckets to a backup plan, turn on versioning for the bucket in S3 or select a different bucket with versioning enabled**.
* Cho nên ta phải **turn on bucket versioning** trước.
* Vào S3 bucket -> Chọn Properties -> Bucket Versioning -> Enable -> Save changes.

* Quay lại để assign s3 resources vào.

* Tuy nhiên, vì AWSBackupServiceRolePolicyForBackup policy ko có policy để access S3 bucket, ta phải tạo 2 policy mới để được backup và Restore S3.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.2.png) 
![sg](/workshop-aws-card-clash-4/images/8.backup/8.11.png) 

### Assign resource:
#### EFS
![sg](/workshop-aws-card-clash-4/images/8.backup/8.7.png) 
#### EBS
![sg](/workshop-aws-card-clash-4/images/8.backup/8.6.png) 
![sg](/workshop-aws-card-clash-4/images/8.backup/8.5.png) 
#### S3
![sg](/workshop-aws-card-clash-4/images/8.backup/8.8.png) 
#### EC2 isntance
![sg](/workshop-aws-card-clash-4/images/8.backup/8.4.png) 

* Ta đã thành công tạo được backup plan.
![sg](/workshop-aws-card-clash-4/images/8.backup/8.3.png) 

![sg](/workshop-aws-card-clash-4/images/8.backup/8.12.png) 
