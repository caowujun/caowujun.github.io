

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [下面是mysql安装](#下面是mysql安装)
  - [1.安装虚拟机](#1安装虚拟机)
  - [2.执行yum更新](#2执行yum更新)
  - [3.下载wget](#3下载wget)
  - [4.下载mysql源安装包](#4下载mysql源安装包)
  - [5.安装mysql](#5安装mysql)
  - [5.验证安装](#5验证安装)
  - [6.执行安装](#6执行安装)
  - [7.启动MySQL](#7启动mysql)
  - [8.查看启动状态](#8查看启动状态)
  - [9.设置开机自启动](#9设置开机自启动)
  - [10.安装vim](#10安装vim)
  - [11.查看mysql临时密码](#11查看mysql临时密码)
  - [12.输入账号和临时密码登录](#12输入账号和临时密码登录)
  - [13.修改MySQL密码](#13修改mysql密码)
  - [14.设置root账号可远程访问，远程访问MySQL](#14设置root账号可远程访问远程访问mysql)
      - [14.1.进入mysql数据库](#141进入mysql数据库)
    - [14.2.添加远程访问密码](#142添加远程访问密码)
    - [14.3.刷新修改](#143刷新修改)
    - [14.4.退出mysql](#144退出mysql)
  - [15.开启防火墙（root下）](#15开启防火墙root下)
  - [16.使用工具链接](#16使用工具链接)
- [下面是redmine安装](#下面是redmine安装)
  - [1.创建mysql，redmine账号](#1创建mysqlredmine账号)
  - [2.安装docker](#2安装docker)
  - [3.下载redmine镜像](#3下载redmine镜像)
  - [4.配置redmine](#4配置redmine)
  - [5.几种常用命令](#5几种常用命令)
  - [6.测试](#6测试)

<!-- /code_chunk_output -->

#下面是mysql安装
---
##1.安装虚拟机

##2.执行yum更新
```bash
yum -y update
```

##3.下载wget
```bash
yum install wget -y
```

##4.下载mysql源安装包
```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

##5.安装mysql
```bash
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

##5.验证安装
```bash
yum repolist enabled | grep "mysql.*-community.*"
```
结果如图：
![mysql验证](/images/redmine/1.png)

##6.执行安装
注意：如果不执行yum module disable mysql，则安装的时候会报错
*<font color=red>Error: Unable to find a match: mysql-community-server</font>*
```bash
yum module disable mysql
yum install mysql-community-server
```

##7.启动MySQL
```bash
systemctl start mysqld
```

##8.查看启动状态
```bash
systemctl status mysqld
```

结果如图：
![mysql启动状态](/images/redmine/2.png)

##9.设置开机自启动
备注：daemon-reload: 重新加载某个服务的配置文件，如果新安装了一个服务，归属于 systemctl 管理，要是新服务的服务程序配置文件生效，需重新加载。
```bash
systemctl enable mysqld
systemctl daemon-reload
```
##10.安装vim
```bash
yum install -y vim
```

##11.查看mysql临时密码
mysql安装完成之后，在/var/log/mysqld.log文件中有一个默认临时密码，用户名是root。查看密码：
```bash
grep 'temporary password' /var/log/mysqld.log
```
备注：也可以通过vim命令，直接去log文件查看临时密码。

##12.输入账号和临时密码登录

```bash
mysql -uroot -p
```
结果如图：
![mysql登录](/images/redmine/3.png)

##13.修改MySQL密码 
*<font color=red>注意：mysql的语法，后面要加“;”</font>*
```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Win2003@';
```

##14.设置root账号可远程访问，远程访问MySQL
*<font color=red>默认root的账号只能localhost本地访问的，如需要远程访问，还需要如下设置</font>*
####14.1.进入mysql数据库
如果执行了13步，默认应该是在mysql下，如果之前退出了，则执行下面命令重新进入，密码是13步修改的新密码。
```bash
mysql -uroot -p
```
###14.2.添加远程访问密码
```bash
GRANT ALL ON *.* TO root@'%' IDENTIFIED BY 'Win2003@' WITH GRANT OPTION;
```
###14.3.刷新修改
```bash
flush privileges;
```
###14.4.退出mysql
```bash
exit;
```
##15.开启防火墙（root下）
```bash
firewall-cmd --permanent --zone=public --add-port=3306/tcp
firewall-cmd --reload
```
##16.使用工具链接
结果如图：
![mysql链接](/images/redmine/4.png)

---

备注：
创建新用户

    create user localuser identified by '666';
    注：'localuser' 即为新用户的用户名，        '666' 即为新用户的登录密码

赋予某个库操作权限

    grant all privileges on students.* to 'localuser'@'%';
    注：'students' 即为指定的数据库，        '%' 即表示无论此用户以哪个IP操作都可以。

然后刷新启用

    flush privileges;
#下面是redmine安装
---
##1.创建mysql，redmine账号

首先登录
```bash
mysql -uroot -p
```
然后分别执行
```bash 
CREATE DATABASE IF NOT EXISTS `redmine` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_unicode_ci`; 
GRANT ALL ON redmine.* TO 'redmine'@'localhost' IDENTIFIED BY 'Win2003@';
GRANT ALL ON *.* TO redmine@'%' IDENTIFIED BY 'Win2003@' WITH GRANT OPTION;
flush privileges;
exit;
```


备注：如果创建某个用户只能访问某个数据库，则执行
```bash
GRANT ALL ON dbname.* TO 'test'@'localhost' IDENTIFIED BY 'youpsd';
GRANT ALL ON dbname.* TO test@'%' IDENTIFIED BY 'youpsd' WITH GRANT OPTION;
flush privileges;
exit;
```
 其中dbname是schema的名字，test是创建的数据库账号，youpsd是密码
如果之前给root账号密码给别人了，现在不想给了，在这里修改root密码
```bash
update mysql.user set authentication_string=password('Win2004@') where user='root';
flush privileges;
exit;
```

##2.安装docker
```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
systemctl start docker 
docker run hello-world
docker version
```
##3.下载redmine镜像
```bash
docker pull sameersbn/redmine 
```
##4.配置redmine
生成文件夹
```bash
mkdir -p /home/redmine/data
mkdir -p /var/log/redmine
```
然后执行run命令，注意最后一行sameersbn/redmine前是有一个空格的，不能和前面的log路径连起来
```bash
docker run --name=redmine -it \
  --restart always\
  --publish=3000:80 \
  --env='DB_ADAPTER=mysql2' \
  --env='DB_HOST=192.168.109.128' --env='DB_NAME=redmine' \
  --env='DB_USER=redmine' --env='DB_PASS=Win2003@' \
  --env='SMTP_DOMAIN=www.163.com'  --env='SMTP_HOST=smtp.163.com'  --env='SMTP_PORT=25' --env='SMTP_USER=redminesmtp@163.com' --env='SMTP_PASS=NZHOSHWRKWQZTXMG'  \
  --volume=/srv/docker/redmine/redmine:/home/redmine/data \
  --volume=/srv/docker/redmine/redmine-logs:/var/log/redmine/ \
  sameersbn/redmine
```

##5.几种常用命令

查询所有容器
```bash
docker ps -a
```
删除容器
```bash
docker rm -f 容器id
```
 重启后，重新启动容器
```bash
sudo docker ps -a
sudo docker start 5e9c6015f069
```

报错
*CentOS 8中安装Docker出现和Podman冲突 problem with installed package podman-1.4.2-5.module_el8.1.0+237+63e26edc.x86_64*

查看是否安装 Podman
```bash
rpm -q podman
```
如果显示
```bash
podman-1.4.2-5.module_el8.1.0+237+63e26edc.x86_64
```
执行删除Podman
```bash
dnf remove podman
```

如果docker没问题，但是redmine不能访问，有可能是端口80没开放，也有可能是容器没有启动。
启动容器
```bash
docker start redmine
```
如果还有问题，则删除容器重新跑一遍。

##6.测试
http://192.168.109.128:3000

 




 