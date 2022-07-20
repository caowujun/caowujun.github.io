# linux 常用命令

_made by caowujun,2022.1.19_

---

## 1. vim 常用命令。

更详细的请查看 [vim 常用命令大全（转）](https://www.cnblogs.com/chen-nn/p/11531932.html)

| 命令    | 含义                                   |
| ------- | -------------------------------------- |
| i:      | 进入编辑状态                           |
| :w      | 保存文件但不退出 vi                    |
| :w file | 将修改另外保存到 file 中，不退出 vi    |
| :w!     | 强制保存，不推出 vi                    |
| :wq     | 保存文件并退出 vi                      |
| :wq!    | 强制保存文件，并退出 vi                |
| :q:     | 不保存文件，退出 vi                    |
| :q!     | 不保存文件，强制退出 vi                |
| :e!     | 放弃所有修改，从上次保存文件开始再编辑 |

## 2. 防火墙的常用命令

查看防火墙某个端口是否开放

```bash
firewall-cmd --query-port=80/tcp
```

开放防火墙端口 80

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

关闭 80 端口

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

配置立即生效

```bash
firewall-cmd --reload
```

查看防火墙状态

```bash
systemctl status firewalld
```

关闭防火墙

```bash
systemctl stop firewalld
```

打开防火墙

```bash
systemctl start firewalld
```

开放一段端口

```bash
firewall-cmd --zone=public --add-port=8121-8124/tcp --permanent
```

查看开放的端口列表

```bash
 firewall-cmd --zone=public --list-ports
```

## 3. redis

Centos8 于 2021 年年底停止了服务，大家再在使用 yum 源安装时候，出现下面错误“错误：Failed to download metadata for repo ‘AppStream’: Cannot prepare internal mirrorlist: No URLs in mirrorlist”
参考：
https://blog.csdn.net/qq_575775600/article/details/125274121
https://baijiahao.baidu.com/s?id=1722728002073366376&wfr=spider&for=pc

下载 redis

```bash
wget https://download.redis.io/releases/redis-6.2.6.tar.gz
```

解压 redis

```bash
tar xzf redis-6.2.6.tar.gz
```

移动 redis 目录，一般都会将 redis 目录放置到 /usr/local/redis 目录

```bash
mv redis-6.2.6 /usr/local/redis
```

进入 redis 安装目录，执行 make 命令编译 redis

```bash
cd /etc/yum.repos.d/
```

执行 make 命令

```bash
make
```

如果执行 make 报错：command not find

```bash
yum -y install gcc automake autoconf libtool make
```

如果执行上面命令报错：Failed to download metadata for repo 'AppStream': Cannot prepare internal mirrorlist

```bash
cd /etc/yum.repos.d/

sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|
```

安装 redis，并指定安装目录

```bash
make install PREFIX=/usr/local/redis
```

启动 redis

```baxh
./bin/redis-server redis.conf
```

判断是否开启

```bash
ps -ef | grep redis
```

通过 redis-cli 测试 redis 是否可用，在 redis 安装目录执行下面命令

```bash
./bin/redis-cli
```

我们通过下面命令随便 set 一个字符串类型的值，key 是 test，value 是 hello

```bash
set test hello
```

通过下面命令 get 出 test 这个 key 的 value 值

```bash
get test
```

关闭运行中的 Redis 服务,redis-cli 进入控制台后输入命令 shutdown

```bash
./bin/redis-cli

shundown
```
