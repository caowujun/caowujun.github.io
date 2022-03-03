        Springboot+vue+nginx简单部署
# **1、准备好vue**
运行

    npm run build

获取到dist文件夹，将该文件夹拷贝到Nginx的html文件夹下面
# **2、修改Nginx的nginx.conf**
设置静态文件目录

    location / {
           root   html/dist;
           index  index.html index.htm;
           try_files $uri /index.html; #简略理解是:尝试去找文件，具体意思baidu
       }
设置访问路径， /boot   代表所有的带boot路径的请求都会转发到proxy_pass所写的访问路径上

    location /boot { 
	                proxy_set_header Host $http_host;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header REMOTE-HOST $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		            proxy_pass http://localhost:5105/;
		            proxy_http_version 1.1;
	}
其他

    listen       8055;
    server_name  localhost;
# **3、测试**

1. dist目录拷贝到

        E:\java\nginx-1.18.0\html
2. cmd E:\java\nginx-1.18.0 文件夹
3. 执行 
   
        start nginx，如果修改了配置文件，执行 nginx -s reload

4. 访问
   
        http://localhost:8055


通过访问http://localhost:8055/boot实际就相当于访问http://localhost:5105/


# **4、 常用命令**
    nginx -h 查看帮助信息
    nginx -v 查看Nginx的版本号
    nginx -V 显示Nginx的版本号和编译信息
    start nginx 启动Nginx
    nginx -s stop 快速停止和关闭Nginx
    nginx -s quit 正常停止或关闭Nginx
    nginx -s reload 配置文件修改重新加载