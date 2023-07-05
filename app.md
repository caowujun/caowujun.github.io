## 环境准备

项目使用 JAVA 开发，需要 JDK 1.8 运行环境，数据库使用的是 Mysql，需要安装 Mysql。

## 数据库

```sql
drop database if exists app_manager;
drop user if exists 'app_manager'@'%'; 
create database app_manager default character set utf8mb4 collate utf8mb4_unicode_ci;
use app_manager;
create user 'app_manager'@'%' identified by 'app_manager';
grant all privileges on app_manager.* to 'app_manager'@'%';
flush privileges;
```

