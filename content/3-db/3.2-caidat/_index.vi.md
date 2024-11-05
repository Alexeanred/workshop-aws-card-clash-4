---
title : "Cài đặt mysql"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2. </b> "
---

* Vào ec2 instance để cài đặt mysql database.
```
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-client -y
sudo dnf install mysql-community-server -y
mysql --version
```
* Kiểm tra trạng thái cài đặt mysql.
```
sudo systemctl status mysqld
``` 
* Tìm password tạm thời sau khi cài đặt.
```
sudo grep 'temporary password' /var/log/mysqld.log
```
* Đăng nhập vào MySQL với password đó.
```
mysql -u root -p
```
* Đổi password sau khi đã đăng nhập thành công:
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Okbaby123@';
```
* Chạy các query sau để tạo database, table.
```sql
CREATE DATABASE prediction_data;
USE prediction_data;

CREATE TABLE predictions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255),
    prediction TEXT,
    upload_time DATETIME
);
```
* Dùng lệnh này nếu quên tên database sau khi login.
```sql
SHOW DATABASES
```

* Fix: ERROR 1130 (HY000): Host '' is not allowed to connect to this MySQL server [duplicate]

```sql
CREATE USER 'root'@'172.31.42.102' IDENTIFIED BY 'Okbaby123@';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.31.42.102' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
