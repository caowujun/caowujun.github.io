配置Apache服务

1.运行以下命令，安装Apache服务。

yum install -y httpd

2.依次运行以下命令，启动Apache服务，并将服务设置为开机自启动。
启动Apache服务：
systemctl start httpd
将Apache服务设置为开机自启动：
systemctl enable httpd
3.运行以下命令，查看Apache服务的运行状态。
systemctl status httpd


Install Docker Engine
https://docs.docker.com/engine/install/centos/
Install the yum-utils package (which provides the yum-config-manager utility) and set up the repository.

 sudo yum install -y yum-utils
 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


1.Install Docker Engine, containerd, and Docker Compose:
 sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
2. sudo systemctl start docker
3. sudo docker run hello-world
#启动
systemctl start docker
#重启
systemctl restart docker
#停止
systemctl stop docker


redmine
生成密码
[root@iZ2zeac1fuwmzym7kacbw3Z opt]# htpasswd -nbm redmine redminepostgres
redmine:$apr1$hrI62q/P$dJgHJ5WyMQtv4xSVjmwUE.

sudo docker pull sameersbn/redmine:latest
  
Step 1. Launch a postgresql container
sudo docker run --name=postgresql -d \
  --env='DB_NAME=redmine' \
  --env='DB_USER=redmine' --env='DB_PASS=$apr1$hrI62q/P$dJgHJ5WyMQtv4xSVjmwUE.' \
  --volume=/srv/docker/redmine/postgresql:/var/lib/postgresql \
  sameersbn/postgresql:9.6-4
Step 2. Launch the redmine container