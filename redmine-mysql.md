# 完整版 redmine 图文安装含 mysql

_made by caowujun,2022.01.11_

---

## 1. 安装虚拟机

### 1.1 创建 linux 虚拟机

![创建linux虚拟机](/images/redmine/linux1.png)

### 1.2 选择 linux 版本

![选择linux版本](/images/redmine/linux2.png)

### 1.3 硬盘设置为单文件

![硬盘设置为单文件](/images/redmine/linux3.png)

### 1.4 选择光盘映像，可调节内存大小

![选择光盘映像，可调节内存大小](/images/redmine/linux4.png)

### 1.5 语言默认英语

![语言默认英语](/images/redmine/linux5.png)

### 1.6 启动虚拟机后自动到这一步

![启动虚拟机后自动到这一步](/images/redmine/linux6.png)

### 1.7 最小化安装，启动快，也可以选择界面版

![最小化安装](/images/redmine/linux7.png)

### 1.8 打开网络，设置 hostname

![打开网络，设置hostname](/images/redmine/linux8.png)

### 1.8 设置上海时间

![设置上海时间](/images/redmine/linux9.png)

---

## 2. mysql 安装

### 2.1 执行 yum 更新

```bash
yum -y update
```

### 2.2 下载 wget

```bash
yum install wget -y
```

### 2.3 下载 mysql 源安装包

```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

### 2.4 安装 mysql

```bash
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

### 2.5 验证安装

```bash
yum repolist enabled | grep "mysql.*-community.*"
```

结果如图：

![mysql验证](/images/redmine/1.png)

### 2.6 执行安装

注意：如果不执行 yum module disable mysql，则安装的时候会报错
_<font color=red>Error: Unable to find a match: mysql-community-server</font>_

```bash
yum module disable mysql
yum install mysql-community-server
```

### 2.7 启动 MySQL

```bash
systemctl start mysqld
```

## 2.8 查看启动状态

```bash
systemctl status mysqld
```

结果如图：
![mysql启动状态](/images/redmine/2.png)

### 2.9 设置开机自启动

备注：daemon-reload: 重新加载某个服务的配置文件，如果新安装了一个服务，归属于 systemctl 管理，要是新服务的服务程序配置文件生效，需重新加载。

```bash
systemctl enable mysqld
systemctl daemon-reload
```

### 2.10 安装 vim

```bash
yum install -y vim
```

### 2.11 查看 mysql 临时密码

mysql 安装完成之后，在/var/log/mysqld.log 文件中有一个默认临时密码，用户名是 root。查看密码：

```bash
grep 'temporary password' /var/log/mysqld.log
```

备注：也可以通过 vim 命令，直接去 log 文件查看临时密码。

### 2.12 输入账号和临时密码登录

```bash
mysql -uroot -p
```

结果如图：
![mysql登录](/images/redmine/3.png)

### 2.13 修改 MySQL 密码

_<font color=red>注意：mysql 的语法，后面要加“;”</font>_

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Win2003@';
```

### 2.14 设置 root 账号可远程访问，远程访问 MySQL

_<font color=red>默认 root 的账号只能 localhost 本地访问的，如需要远程访问，还需要如下设置</font>_

### 2.14.1 进入 mysql 数据库

如果执行了 13 步，默认应该是在 mysql 下，如果之前退出了，则执行下面命令重新进入，密码是 13 步修改的新密码。

```bash
mysql -uroot -p
```

### 2.14.2 添加远程访问密码

```bash
GRANT ALL ON *.* TO root@'%' IDENTIFIED BY 'newpwd' WITH GRANT OPTION;
```

### 2.14.3 刷新修改

```bash
flush privileges;
```

### 2.14.4 退出 mysql

```bash
exit;
```

## 2.15 开启防火墙（root 下）

```bash
firewall-cmd --permanent --zone=public --add-port=3306/tcp
firewall-cmd --reload
```

## 2.16. 使用工具链接（dbeaver）

结果如图：
![mysql链接](/images/redmine/4.png)

---

## 2.17. 常用 mysql 命令：

创建新用户('localuser' 即为新用户的用户名， '666' 即为新用户的登录密码)

```
create user localuser identified by '666';
```

赋予某个库操作权限('students' 即为指定的数据库， '%' 即表示无论此用户以哪个 IP 操作都可以。)

```
grant all privileges on students.* to 'localuser'@'%';
```

然后刷新启用

```
flush privileges;
```

如果创建某个用户只能访问某个数据库，则执行(其中 dbname 是 schema 的名字，test 是创建的数据库账号，youpsd 是密码)

```bash
GRANT ALL ON dbname.* TO 'test'@'localhost' IDENTIFIED BY 'youpsd';
GRANT ALL ON dbname.* TO test@'%' IDENTIFIED BY 'youpsd' WITH GRANT OPTION;
flush privileges;
exit;
```

如果之前给 root 账号密码给别人了，现在不想给了，在这里修改 root 密码

```bash
update mysql.user set authentication_string=password('xinmima') where user='root';
flush privileges;
exit;
```

# 3. redmine 安装

## 3.1 创建 mysql，redmine 账号

首先登录

```bash
mysql -uroot -p
```

然后分别执行（创建 redmine 数据库）

```bash
CREATE DATABASE IF NOT EXISTS `redmine` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_unicode_ci`;
GRANT ALL ON redmine.* TO 'redmine'@'localhost' IDENTIFIED BY 'woshimima';
GRANT ALL ON *.* TO redmine@'%' IDENTIFIED BY 'woshimima' WITH GRANT OPTION;
flush privileges;
exit;
```

## 3.2 安装 docker

按顺序执行，下载 docker，并测试 helloworld

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
systemctl start docker
docker run hello-world
docker version
```

## 3.3 下载 redmine 镜像

拉取镜像

```bash
docker pull sameersbn/redmine
```

## 3.4 配置 redmine

生成文件夹

```bash
mkdir -p /home/redmine/data
mkdir -p /var/log/redmine
```

然后执行 run 命令，注意最后一行 sameersbn/redmine 前是有一个空格的，不能和前面的 log 路径连起来

```bash
docker run --name=redmine -it \
  --restart always \
  --publish=3000:80 \
  --env='DB_ADAPTER=mysql2' \
  --env='DB_HOST=192.168.109.128' \
  --env='DB_NAME=redmine' \
  --env='DB_USER=redmine' \
  --env='DB_PASS=mima1' \
  --env='SMTP_DOMAIN=www.163.com'  \
  --env='SMTP_HOST=smtp.163.com'  \
  --env='SMTP_PORT=25' \
  --env='SMTP_USER=redminesmtp@163.com' \
  --env='SMTP_PASS=NZHOSHWRKWQZTXMG'  \
  --volume=/srv/docker/redmine/redmine:/home/redmine/data \
  --volume=/srv/docker/redmine/redmine-logs:/var/log/redmine/ \
  sameersbn/redmine
```

<!-- Win2003@,NZHOSHWRKWQZTXMG -->
# 3.5. 测试

http://192.168.109.128:3000

## 4. redis安装

## 4.1 拉取镜像
```bash
docker pull redis
```

## 4.2 启动容器
```bash
docker run  --name redis  --restart=always -p 6379:6379 -d redis
```

# 5 rabbitMq安装

## 5.1 拉取镜像
```bash
docker pull rabbitmq:3.7-management 
```

## 5.2 启动容器
```bash
docker run --name rabbitmq  --restart=always -d -p 5672:5672 -p 15672:15672
-e RABBITMQ_DEFAULT_USER=spring -e RABBITMQ_DEFAULT_PASS=spring
rabbitmq:3.7-management
```

# 6 redis安装

## 6.1 拉取镜像
```bash
docker pull redis
```


## 6.2 启动容器
```bash
docker run  --name redis  --restart=always -p 6379:6379 -d redis
```

# 7 几种常用命令

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

如果报错
_<font color=red>CentOS 8 中安装 Docker 出现和 Podman 冲突 problem with installed package podman-1.4.2-5.module_el8.1.0+237+63e26edc.x86_64</font>_

1)查看是否安装 Podman

```bash
rpm -q podman
```

2)如果显示

```bash
podman-1.4.2-5.module_el8.1.0+237+63e26edc.x86_64
```

3)执行删除 Podman

```bash
dnf remove podman
```

如果 docker 没问题，但是 redmine 不能访问，有可能是端口 80 没开放，也有可能是容器没有启动。
启动容器

```bash
docker start redmine
```

如果还有问题，则删除容器重新跑一遍。

