---
title : "Tạo EFS"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

* Đầu tiên, ta tạo **security group** cho efs trước:
    * Inbound rules: Type: **NFS** Port: **2049** Source: **sg của ec2 instance** ta dùng.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.1.png) 

* Tiếp theo, ta vào **Elastic File System**:
* Chọn **Create file system**:
* Đặt tên: **fs-streamlit-app**.
* Chọn **default vpc**.
* Chọn **Create**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.2.png) 
* Vào mục **Network**:
* Chọn **Manage**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.3.png) 
* Ở các AZ trong region:
    * Xóa **default sg**, chọn **efs-sg** đã tạo trước đó.
    * Nếu muốn thêm AZ thì ta chọn **Add mount target**.
    * Chọn **Save**.
![sg](/workshop-aws-card-clash-4/images/7.efs/7.4.png) 

* Chọn **Attach**.

Copy: ```sudo mount -t efs -o tls fs-0b4552f241db29dae:/ efs```
![sg](/workshop-aws-card-clash-4/images/7.efs/7.5.png) 

* Vào ec2 instance ta muốn mount, dán vào để mount efs.

* kiểm tra bằng lệnh ```df -h```.

* Bạn có thể thực hành thêm việc chia sẻ files giữa các ec2 instances. 
* Trong lúc thực hành thì mình đã lỡ tay dùng lệnh mv file app.py và file log vào efs và không tìm thấy lun và mình phải cấu hình lại =)).

