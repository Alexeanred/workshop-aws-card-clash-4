---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
![arch](/workshop-aws-card-clash-4/images/kientruc.png) 

## Ý tưởng kiến trúc: 
* **User sẽ phân loại ảnh các dòng xe hơi vinfast thông qua kiến trúc sau:**
    * Web Server instance là 1 ec2 instance chạy streamlit application làm giao diện người dùng, ta sẽ lấy amazon sagamaker endpoint để dùng làm model phân loại ảnh.
    * amazon EFS đóng vai trò là nơi lưu trữ file dùng chung giữa các ec2 instances chạy streamlit nếu ta tạo thêm.
    * amazon S3 trong kiến trúc này đóng vai trò lưu trữ ảnh người dùng đã upload lên streamlit để phục vụ việc phân tích data sau này.
    * Database instance này sẽ được cài đặt trên một ec2 instance, ta sẽ cài đặt mysql database trên linux.
    * Ta sẽ cấu hình thêm một amazon cloudwatch log group để quản lý log của streamlit application.
    * Đồng thời, ta cũng sẽ tạo VPC endpoint cho S3 va cloudwatch để bảo mật hơn việc ghi log hay ghi data qua đường mạng backbone của aws.
    * AWS backup đóng vai trò sao lưu và phục hồi các thành phần trong kiến trúc này như ec2 instance, db instance, efs, s3.
    * Đồng thời ta cũng thực hành truy cập ec2 instance qua Session manager của System Manager để test việc tạo thành công VPC endpoint cho S3 và cloudwatch.
## Giới thiệu dịch vụ:

* **Amazon EC2 (Elastic Compute Cloud)**: 
    * Amazon EC2 cung cấp hạ tầng máy chủ ảo linh hoạt, giúp bạn dễ dàng tạo các instances (máy chủ ảo) để chạy các ứng dụng và dịch vụ cần thiết. 
    * EC2 instances sẽ được sử dụng để chạy ứng dụng Streamlit nhằm cung cấp giao diện người dùng cho việc phân loại ảnh. Một instance khác sẽ chạy MySQL database, quản lý và lưu trữ thông tin ảnh.

* **Amazon SageMaker**:
    * Amazon SageMaker là dịch vụ học máy, cho phép bạn xây dựng, huấn luyện và triển khai các mô hình học máy trên quy mô lớn mà không cần quản lý cơ sở hạ tầng.
    * Bạn sẽ dùng SageMaker để triển khai endpoint cho mô hình phân loại ảnh, giúp EC2 instance có thể gọi endpoint này để phân loại ảnh các dòng xe VinFast dựa trên ảnh người dùng tải lên.
* **Amazon EFS (Elastic File System)**:
    * Amazon EFS là một hệ thống lưu trữ file có thể mở rộng, chia sẻ, và được truy cập từ nhiều EC2 instances cùng một lúc. EFS tự động điều chỉnh để lưu trữ nhiều file hơn khi nhu cầu tăng.
    * EFS sẽ đóng vai trò lưu trữ các file dùng chung, như mã nguồn hoặc log file của ứng dụng Streamlit, và cho phép nhiều EC2 instances truy cập đồng thời, giúp duy trì nhất quán khi mở rộng hệ thống.
* **Amazon S3 (Simple Storage Service)**
    * Amazon S3 là dịch vụ lưu trữ đối tượng với khả năng mở rộng lớn, độ bền cao và bảo mật tốt, cho phép lưu trữ và truy xuất dữ liệu bất cứ khi nào cần.
    * S3 sẽ được sử dụng để lưu trữ các ảnh mà người dùng tải lên từ ứng dụng Streamlit, cho phép quản lý và phân tích dữ liệu hình ảnh lâu dài.
* **Amazon CloudWatch Logs**
    * Amazon CloudWatch Logs giúp bạn giám sát và lưu trữ log từ các dịch vụ AWS và ứng dụng chạy trên AWS.
    * CloudWatch Logs sẽ được sử dụng để thu thập và theo dõi log của ứng dụng Streamlit, giúp bạn dễ dàng giám sát hoạt động và phát hiện các vấn đề tiềm ẩn.
* **Amazon VPC Endpoint**
    * VPC Endpoint cho phép các tài nguyên trong VPC của bạn kết nối với các dịch vụ AWS mà không cần truy cập qua Internet, giúp tăng cường bảo mật và tối ưu hóa kết nối.
    * Bạn sẽ tạo VPC Endpoints cho S3 và CloudWatch, giúp EC2 instances có thể truy cập các dịch vụ này mà không cần Internet Gateway, đảm bảo dữ liệu được truyền qua mạng nội bộ của AWS.
* **AWS Backup**
    * AWS Backup cung cấp dịch vụ sao lưu và khôi phục toàn diện, tự động cho các tài nguyên AWS, đảm bảo dữ liệu và cấu hình của bạn luôn được sao lưu an toàn.
    * AWS Backup sẽ giúp bạn sao lưu các thành phần quan trọng trong kiến trúc, bao gồm EC2 instance, MySQL database trên EC2, EFS và các đối tượng trong S3. Điều này giúp dễ dàng khôi phục dữ liệu trong trường hợp sự cố xảy ra.
* **AWS Systems Manager - Session Manager**
    * Session Manager là một phần của AWS Systems Manager, cho phép bạn truy cập các EC2 instances một cách an toàn và không cần kết nối SSH hoặc mở cổng vào internet. Đây là một công cụ bảo mật cao, thuận tiện cho việc quản lý và giám sát các instances mà không cần phải cấu hình key pairs hoặc public IP.
    * Trong kiến trúc, Session Manager sẽ được sử dụng để truy cập các EC2 instances mà không cần mạng công cộng, đặc biệt hữu ích khi bạn đã triển khai VPC Endpoints cho S3 và CloudWatch.