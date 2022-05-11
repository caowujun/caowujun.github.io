# 新电脑重装

_made by caowujun,2020.11.11_

---

## 1. oracle 安装

orcl 的管理密码用 orcl，然后就口令管理的 system，sys 的密码都用 sys。

**注意的是：经过测试，发现只要装了服务端，就可以连本地 orcl 库和服务器的库，不需要装 client 端，plsql 可以访问。**

## 2. JDK1.8 安装

环境变量修正增加 JAVA_HOME

```
C:\Program Files\Java\jdk1.8.0_151
```

path 新增，注意 Windows10 应该是分 2 行增加，而不是这 2 个写在一行里。

```
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

测试：

```bash
java –version
javac –version
```

## 3. MVN 安装

测试:

```bash
 mvn –version
```

修改 setting.xml 里的 local 配置 ,仓库地址：

```xml
<localRepository>D:\java\repository</localRepository>
```

可增加阿里镜像：

```xml
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云公共仓库</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

参考文档 [阿里云 Maven 中央仓库](https://maven.aliyun.com/mvn/guide)

环境变量新增 MAVEN_HOME，

```
D:\java\apache-maven-3.6.0
```

path 新增

```
%MAVEN_HOME%\bin\
```

## 3. IDEA 安装

安装成功后，安装插件 lombok，easycode
