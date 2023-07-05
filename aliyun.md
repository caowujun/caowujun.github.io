
yum  update
更换源
https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b114JXWIf

 
###  安装其他 


yum install java-17-openjdk
 

yum的maven，nodejs，版本都好低


发现node版本过低
# 查看当前版本
node -v
# 清理本地包缓存
npm cache clean -f
# 安装
npm i -g n
# 查看n是否安装成功
n -V


n lts // 长期支持版


node -v
如果还是旧的，关掉shell，打开新的就好了

npm install -g pnpm


  



## 配置Apache服务 

### 1、运行以下命令，安装Apache服务。 

参照：https://help.aliyun.com/document_detail/253434.html?spm=a2c4g.223746.0.0.3154637fD0lg1C
```bash
 yum install -y httpd
```

## 2、依次运行以下命令，启动Apache服务，并将服务设置为开机自启动。
启动Apache服务：

```bash
 systemctl start httpd
```

将Apache服务设置为开机自启动：

```bash
 systemctl enable httpd
```
### 3、运行以下命令，查看Apache服务的运行状态。

```bash
 systemctl status httpd
```

## 安装docker
### 1、安装
Install the yum-utils package (which provides the yum-config-manager utility) and set up the repository.


```bash
 yum install -y yum-utils
 yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
第二句报错

Invalid configuration value: failovermethod=priority in /etc/yum.repos.d/CentOS-epel.repo; 配置：ID 为 “failovermethod” 的 OptionBinding 不存在

```bash
vim /etc/yum.repos.d/CentOS-epel.repo
```
打开这个文件，然后注释
failovermethod=priority  

:wq!   强制保存即可

参照：https://docs.docker.com/engine/install/centos/

Install Docker Engine, containerd, and Docker Compose

```bash
 yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 2、启动
```bash
 systemctl start docker
```

### 3、测试
```bash
 docker run hello-world
```
登录IP:80

### 4、常见命令

开启
```bash
 systemctl start docker
```

重启
```bash
 systemctl restart docker
```

停止
```bash
 systemctl stop docker
```

开机启动
```bash
 systemctl enable docker
```


## 防火墙（阿里云不需要防火墙）

启动服务：
```bash
systemctl start firewalld.service
```
停止服务：
```bash
 systemctl stop firewalld.service
```
重启服务：
```bash
 systemctl restart firewalld.service
```
服务状态：
```bash
 systemctl status firewalld.service
```

开机启动
```bash
 systemctl enable firewalld.service
```

查看开机项列表
```bash
 systemctl list-unit-files
```

查看防火墙某个端口是否开放

```bash
firewall-cmd --query-port=5432/tcp
```

开放防火墙端口 80

```bash
firewall-cmd --zone=public --add-port=5432/tcp --permanent
```

关闭 80 端口

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

配置立即生效

```bash
firewall-cmd --reload
```

开放一段端口

```bash
firewall-cmd --zone=public --add-port=8121-8124/tcp --permanent
```

查看开放的端口列表

```bash
 firewall-cmd --zone=public --list-ports
```
netstat -ntulp
iptables -t nat -I PREROUTING -p tcp --dport 5432 -m state --state NEW -j ACCEPT

## 生成密码

```bash
htpasswd -nbm redmine redminepostgres
redmine:$apr1$hrI62q/P$dJgHJ5WyMQtv4xSVjmwUE.
```

## 安装数据库Postgres

参照： https://developer.aliyun.com/article/1240243

查询
```bash
docker search postgres
```

下载
```bash
docker pull postgres
```

安装
```bash
docker volume create postgre-data
```
volumes(卷挂载) ：存储在主机文件系统的一部分中，该文件系统由Docker管理 （在Linux上是“ /var/lib/docker/volumes /”）。 非Docker进程不应修改文件系统的这一部分。 卷是在Docker中持久存储数据的最佳方法。

docker自动在外部创建文件夹自动挂载容器内部指定的文件夹内容(Dockerfile VOLUME指令的作用)

  
容器
```bash
  docker run -id --restart=always --name=postgresql -v postgre-data:/var/lib/postgresql/data -p 5432:5432 -e POSTGRES_PASSWORD=Win2003@ -e LANG=C.UTF-8 postgres
```

 


查看端口是否通
```bash
netstat -ntulp
```

查看启动日志
```bash
 docker logs postgres
```

语法
参照：https://www.runoob.com/docker/docker-run-command.html

* POSTGRES_PASSWORD 设定PostgreSQL的超级用户的密码，这里设定为123456，PostgreSQL容器的超级用户用户名为postgres
* LANG 设定语言环境为C.UTF-8以支持中文
* --restart=always  # 表示容器退出时，docker会总是自动重启这个容器


## 安装redmine

1. 下载

```bash
docker pull sameersbn/postgresql
docker pull sameersbn/redmine
```

mkdir -p /apps/redmine/postgresql
mkdir -p /apps/redmine/redmine
mkdir -p /apps/redmine/redmine-logs

2. Launch the redmine container

  网上大部分都是
    docker run -id --name=postgresql-redmine  --restart=always   --publish=5433:5432  --env='DB_NAME=redmine_production'   --env='DB_USER=redmine' --env='DB_PASS=Win2004@'   --volume=/apps/redmine/postgresql:/var/lib/postgresql   sameersbn/postgresql
会报各种错误，比如
PG::InsufficientPrivilege: ERROR: permission denied for schema public
initdb: error: program "postgres" is needed by initdb but was not found in the same directory as "/usr/lib/postgresql/15/bin/initdb"
 
 最后还是下面解决

 ```bash

 docker run --name=sameersbn_postgresql -e TZ="Asia/Shanghai" -id --publish=5433:5432   --env='POSTGRES_USER=redmine' --env='POSTGRES_PASSWORD=Win2003@' --restart=always --volume=/apps/redmine/postgresql:/var/lib/postgresql sameersbn/postgresql

 docker run --name=sameersbn_redmine -e TZ="Asia/Shanghai" -id --link=sameersbn_postgresql:postgresql --publish=3000:80 --env='REDMINE_PORT=3000'     --env='SMTP_DOMAIN=www.163.com'    --env='SMTP_HOST=smtp.163.com'   --env='SMTP_PORT=25'  --env='SMTP_USER=redminesmtp@163.com'   --env='SMTP_PASS=NZHOSHWRKWQZTXMG'  --restart=always --volume=/apps/redmine/redmine:/home/redmine/data --volume=/apps/redmine/redmine-logs:/var/log/redmine  sameersbn/redmine
 ```
————————————————
版权声明：本文为CSDN博主「武晓兵」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/wuxiaobingandbob/article/details/113482198

 
代理redmine转发

