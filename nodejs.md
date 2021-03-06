# node.js 图文安装

_made by caowujun,2020.11.11_

---

## 1. 下载

    https://nodejs.org/zh-cn/

![install node.js](images/nodejs/1.png)

## 2. 按顺序默认安装

![install node.js](images/nodejs/2.png)
![install node.js](images/nodejs/3.png)
![install node.js](images/nodejs/4.png)
![install node.js](images/nodejs/5.png)
![install node.js](images/nodejs/6.png)
![install node.js](images/nodejs/7.png)
![install node.js](images/nodejs/8.png)
![install node.js](images/nodejs/9.png)

## 3. 测试

    打开命令窗口，输入node -v,npm -v ,如图

![install node.js](images/nodejs/10-1.png)

## 4.进阶

node.js 安装自带的 NPM 下载速度慢，并且默认下载目录在 C 盘，需要我们处理一下。

### 4.1 在硬盘中新建 2 个文件夹如下

    E:\nodejs\node_cache
    E:\nodejs\node_global

### 4.2 在命令窗口执行以下 2 个命令

```bash
npm config set prefix "E:\nodejs\node_global"
npm config set cache "E:\nodejs\node_cache"
```

![install node.js](images/nodejs/13-1.png)

### 4.3 打开环境变量配置窗口

![install node.js](/images/nodejs/14-1.png)

#### 4.3.1 增加系统变量 NODE_PATH，值 E:\nodejs\node_global\node_modules

![install node.js](images/nodejs/15.png)

#### 4.3.2 修改用户变量 PATH，将 C:\Users\用户名\AppData\Roaming\npm 的值改为 E:\nodejs\node_global

修改前

![install node.js](images/nodejs/16-1.png)

修改后

![install node.js](images/nodejs/17-1.png)

### 4.4 复制一下代码在命令窗口执行，切换到国内淘宝镜像

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

![install node.js](images/nodejs/11-1.png)
安装后如图所示
![install node.js](images/nodejs/12-1.png)

npm 升级

```bash
npm install -g npm
```
