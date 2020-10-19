# MySQL

## Installation
### On WLS(utunbu):
#### 1. Install: 
```
sudo apt update
```
```
sudo apt install mysql-server
```
#### 2. Configure:
```
sudo mysql_secure_installation
```
Follow each steps in output until you've down. 

If you cannot see the expected configuration message, try more times. 

To check if your MySQL has been configured correctly, run following command:
```
mysqld --initialize
```
You should expect:
```
mysqld: Can't create directory '/var/lib/mysql/' (Errcode: 17 - File exists)
. . .
2018-04-23T13:48:00.572066Z 0 [ERROR] Aborting
```

#### 3. Testing:
- __Method 1:__
MySQL should be running after configuration. If not, run the below command first:
```
sudo service mysql start
```
Or depending on ubuntu version:
```
sudo systemctl start mysql
```
Check its status to see if it's running:
```
service status mysql
```
Or, again, depending on version:
```
systemctl status mysql.service
```
Should see the output like:
```
● mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: en
   Active: active (running) since Wed 2018-04-23 21:21:25 UTC; 30min ago
 Main PID: 3754 (mysqld)
    Tasks: 28
   Memory: 142.3M
      CPU: 1.994s
   CGroup: /system.slice/mysql.service
           └─3754 /usr/sbin/mysqld
```

- __Method 2:__
```
sudo mysqladmin -p -u root version
```
Should see an output similar to:
```
mysqladmin  Ver 8.42 Distrib 5.7.21, for Linux on x86_64
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version      5.7.21-1ubuntu1
Protocol version    10
Connection      Localhost via UNIX socket
UNIX socket     /var/run/mysqld/mysqld.sock
Uptime:         30 min 54 sec

Threads: 1  Questions: 12  Slow queries: 0  Opens: 115  Flush tables: 1  Open tables: 34  Queries per second avg: 0.006
```

---
###### Reference: Tutorial on [How To Install MySQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04#step-1-%E2%80%94-installing-mysql).
---
