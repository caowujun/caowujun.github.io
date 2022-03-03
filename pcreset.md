# **1、oracle安装**
orcl的管理密码用orcl，然后就口令管理的system，sys的密码都用sys。

    注意的是：经过测试，发现只要装了服务端，就可以连本地orcl库和服务器的库，不需要装client端，plsql可以访问。 

# **2、JDK1.8安装** 
        测试：      java –version          javac –version
1. JDK1.8一路默认安装。
2. 环境变量新增JAVA_HOME

         C:\Program Files\Java\jdk1.8.0_151
         
    path新增，注意Windows10应该是分2行增加，而不是这2个写在一行里。

        %JAVA_HOME%\bin                   %JAVA_HOME%\jre\bin

 # **3、MVN安装**
         测试：      mvn –version       
1. 解压缩下载的mvn压缩包。
2. 修改setting.xml里的local配置，  

        <localRepository>D:\java\repository</localRepository>
        镜像：
        <mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云公共仓库</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
        https://maven.aliyun.com/mvn/guide
        
3. 环境变量新增MAVEN_HOME，

        D:\java\apache-maven-3.6.0
        
    path新增

         %MAVEN_HOME%\bin\
# **4、IDEA安装**
        如果是打开以前的程序，把.idea文件夹删除。
安装成功后，安装插件lombok，easycode
# **5、NODEJS安装** 
一路默认安装，现在的nodejs自带npm。

        测试：      node -v,npm –v

另外，npm的默认下载插件保存的路径在C盘，为了防止C盘过大，一般重新设置。

        npm config set prefix "E:\nodejs\node_global"
        npm config set cache "E:\nodejs\node_cache"
环境变量的**用户**变量path，修改为

        E:\nodejs\node_global
环境变量的系统变量，增加NODE_PATH

        E:\nodejs\node_global\node_modules

npm默认速度较慢，一般设置连接淘宝镜像。

        npm install -g cnpm --registry=https://registry.npm.taobao.org

### 更详细的参见

[node.js图文安装][https://github.com/P0164442/md/blob/main/Node.js图文安装.md]

# **5、VUE安装** 

[VUE图文安装][https://github.com/P0164442/md/blob/main/vue项目从零开始.md]
