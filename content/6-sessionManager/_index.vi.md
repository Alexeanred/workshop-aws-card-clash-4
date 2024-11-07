---
title : "Session Manager"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

- System managers là 1 **non-vpc service**, có thể ssh public subnet ec2 instance nhưng nếu private thì cần **cấu hình vpc endpoint**.

- Còn một cách khác là ta dùng Bastion host nhưng ta đang dùng VPC endpoint nên cấu hình tiếp thôi =))

- Để có thể dùng Session manager trong System managers truy cập vào private ec2 instance, ta cần cấu hình 3 vpc endpoints sau:
    * **ssm endpoint**
    * **ssmmessages endpoint**
    * **ec2messages endpoint**
* Ta sẽ cấu hình 1 cái, 2 cái còn lại làm tương tự chỉ khác service name:
* Chọn **Endpoints**: 
* Chọn **create endpoint**.
* Name tag: **ssm-endpoint**.
* Chọn **AWS services**.
* Search: **Service Name = com.amazonaws.us-east-1.ssm**.
* Chọn **VPC default**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.6.png) 
* Chọn subnets: **us-east-1d**, subnet của ec2 instance.
* Chọn sg: **vpc endpoint sg** vừa tạo.
* Chọn **Full access** cho Policy.
* Chọn **Create endpoint**.
* Interface endpoint khi tạo sẽ lâu hơn Gateway endpoint.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.7.png) 
* Sau khi tạo xong 3 VPC endpoints cho System manager, ta vào System manager:
* Chọn mục **Session Manager**:
* Chọn **Start a session**.
* Chọn ec2 instance mà muốn connect vào. 
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.14.png) 
* Sau khi vào được terminal của ec2 instances, ta cần bỏ route internet gateway ở route table để thử xem có giao tiếp với các dịch vụ qua vpc endpoint được không.
* Vào **Route table**, chọn route table liên kết với subnet chứa ec2 instance.
* Chọn **Edit routes**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.15.png) 
* Ta thấy có route **nternet Gateway** và ta chọn **remove** và **Save changes**.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.16.png) 
* Khi vào thành công, ta chạy các câu lệnh để test vpc endpoint của s3 và cloudwatch.
* Đầu tiên, ta cần vào với user là ec2-user.
* Tiếp theo ta thử list bucket ở S3 bucket thử xem được không, nếu được thì ta thành công.
* Tiếp theo ta thử ghi log để test xem cloudwatch log có nhận được không.
```
sudo su - ec2-user
aws s3 ls s3://vinfast-car-images-150903
echo "Manual log test $(date)" >> /home/ec2-user/test2.log
```
* Đã list ra được các object ở bucket vinfast-images.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.8.png) 
* Đã có event log mới ghi vào đó là log ta vừa ghi.
![sg](/workshop-aws-card-clash-4/images/5.fwd/5.10.png) 


