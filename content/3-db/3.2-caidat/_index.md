---
title : "Install MySQL"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2. </b> "
---

* Go to the ec2 instance to install the mysql database.
```
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-client -y
sudo dnf install mysql-community-server -y
mysql --version
```
* Check mysql installation status.
```
sudo systemctl status mysqld
``` 
* Find temporary password after installation.
```
sudo grep 'temporary password' /var/log/mysqld.log
```
* Log in to MySQL with that password.
```
mysql -u root -p
```
* Change password after successfully logging in:
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Okbaby123@';
```
* Run the following queries to create database and table.
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
* Use this command if you forget the database name after logging in.
```sql
SHOW DATABASES
```

* Fix: ERROR 1130 (HY000): Host '' is not allowed to connect to this MySQL server [duplicate]

```sql
CREATE USER 'root'@'172.31.42.102' IDENTIFIED BY 'Okbaby123@';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.31.42.102' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
