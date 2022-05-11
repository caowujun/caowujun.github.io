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
