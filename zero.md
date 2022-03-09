# 新电脑重装
_made by caowujun,2020.11.11_

---
## 1. oracle安装
orcl的管理密码用orcl，然后就口令管理的system，sys的密码都用sys。

**注意的是：经过测试，发现只要装了服务端，就可以连本地orcl库和服务器的库，不需要装client端，plsql可以访问。** 

## 2. JDK1.8安装

 
环境变量修正增加JAVA_HOME  
``` 
C:\Program Files\Java\jdk1.8.0_151
```

path新增，注意Windows10应该是分2行增加，而不是这2个写在一行里。
``` 
%JAVA_HOME%\bin   
%JAVA_HOME%\jre\bin
```
测试：      
```bash
java –version          
javac –version
```

## 3. MVN安装
测试:
```bash
 mvn –version   
```
修改setting.xml里的local配置 ,仓库地址：
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
参考文档 [阿里云Maven中央仓库](https://maven.aliyun.com/mvn/guide)
        
环境变量新增MAVEN_HOME，
```
D:\java\apache-maven-3.6.0
``` 
path新增
```
%MAVEN_HOME%\bin\
```
## 3. IDEA安装
安装成功后，安装插件lombok，easycode
 
 
