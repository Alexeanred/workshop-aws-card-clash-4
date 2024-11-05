---
title : "Cấu hình Cloudwatch log ec2 instance"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---
### Tạo log groups
* Vào cloudwatch, chọn mục **Log groups**.
![sg](/ws1/images/4.s3/4.1_.png) 
* Ở log group name: ```streamlit-app-log-group```
* Chọn nút **Create**
![sg](/ws1/images/4.s3/4.2_.png) 

### Cài đặt cloudwatch agent trên web server ec2 instance
* Ở web server instance, chạy các lệnh sau:

```
sudo yum install amazon-cloudwatch-agent -y
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
sudo systemctl status amazon-cloudwatch-agent
cat /var/log/amazon/amazon-cloudwatch-agent/amazon-cloudwatch-agent.log
```
* Sẽ có một config wizard cho ta các lựa chọn để cấu hình agent, cần cấu hình đúng để agent hoạt động được.
```
On which OS are you planning to use the agent? -> Linux
Are you using EC2 or On-Premises hosts? -> EC2
Which user are you planning to run the agent? -> root
Do you want to turn on StatsD daemon? -> yes
Which port do you want StatsD daemon to listen to? -> 8125
What is the collect interval for StatsD daemon? -> 10s
What is the aggregation interval for metrics collected by StatsD daemon? -> 60s
Do you want to monitor metrics from CollectD? -> no
Do you want to monitor any host metrics? e.g. CPU, memory, etc. -> no
Do you have any existing CloudWatch Log Agent configuration file to import for migration? -> no
Do you want to monitor any log files? -> yes
Log file path: /home/ec2-user/test2.log
Log group name: streamlit-app-log-group
Log group class: STANDARD
Log stream name: [{instance_id}]
Log Group Retention in days: -1
Do you want to specify any additional log files to monitor? -> no
Do you want the CloudWatch agent to also retrieve X-ray traces? -> no
Do you want to store the config in the SSM parameter store? -> no
```
* Ở trên là một số cấu hình qua các câu hỏi mình đã chọn , bạn có thể tham khảo.
* Sau khi cấu hình xong thì file config sẽ như thế này.
```
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/home/ec2-user/test2.log",
            "log_group_name": "streamlit-app-log-group",  # Update to your actual log group name
            "log_stream_name": "{instance_id}",  # This will auto-fill the EC2 instance ID
            "timestamp_format": "%Y-%m-%d %H:%M:%S"
          }
        ]
      }
    }
  }
}

```
* Sau khi cấu hình xong, ta chạy thêm các lệnh để hoàn thành việc cấu hình cloudwatch agent.
* Khi cấu hình thành công, ta sẽ thấy được log đã được đẩy vào log group.
![sg](/ws1/images/4.s3/4.3_.png) 