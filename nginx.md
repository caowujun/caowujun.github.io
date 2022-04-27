# nignx 学习

_made by caowujun,2022.1.19_

---

## 1. Springboot+vue+nginx 简单部署

准备好 vue,运行

```bash
npm run build
```

获取到 dist 文件夹，将该文件夹拷贝到 Nginx 的 html 文件夹下面

## 2. 修改 Nginx 的 nginx.conf

设置静态文件目录

```xml
    location / {
           root   html/dist;
           index  index.html index.htm;
           try_files $uri /index.html; #简略理解是:尝试去找文件，具体意思baidu
       }
```

设置访问路径， /boot 代表所有的带 boot 路径的请求都会转发到 proxy_pass 所写的访问路径上

```
    location /boot {
	            proxy_set_header Host $http_host;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header REMOTE-HOST $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		    proxy_pass http://localhost:5105/;
		    proxy_http_version 1.1;
	}
```

其他

    listen       8055;
    server_name  localhost;

## 3. 测试

1. dist 目录拷贝到 E:\java\nginx-1.18.0\html
2. cmd 到 E:\java\nginx-1.18.0 文件夹
3. 执行

```bash
start nginx
```

如果修改了配置文件，执行

```bash
nginx -s reload
```

4. 访问 http://localhost:8055

通过访问 http://localhost:8055/boot 实际就相当于访问 http://localhost:5105/

## 4. 常用命令

```bash
nginx -h //查看帮助信息
nginx -v //查看Nginx的版本号
nginx -V //显示Nginx的版本号和编译信息
start nginx //启动Nginx
nginx -s stop //快速停止和关闭Nginx
nginx -s quit //正常停止或关闭Nginx
nginx -s reload //配置文件修改重新加载
```
