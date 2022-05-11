# java 学习

_made by caowujun,2022.1.19_

---

## 1. druid 数据库加密

springboot 下,pom 文件

```xml
<dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.4</version>
</dependency>
```

yml

```yml
spring:
  application:
    name: boot
  main:
    allow-bean-definition-overriding: true
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://ip:3306/db?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8&useSSL=false
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: XOnIiT/L9aIj1BoFgcGgIxqUmnC2AZlkc/WfCBVPdDLGVVbW81STArqBUTeDaGBQKF4YdfMXCTLjwuDIozt9Dw==
    druid:
      filter:
        config:
          enabled: true
      connection-properties: config.decrypt=true;config.decrypt.key=MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAIkR+Y73QCtq2P8jlpxUWbYiAdowGPgkABXfLO79Bwc0Babv2urmjbFGMoDZKfTR5QlS19ZUcDFu1YoJ0o533mMCAwEAAQ==
```

密钥生成命令，跳转到 druid 的 jar 包下面，打开 cmd

```bash
 java -cp druid-1.2.4.jar com.alibaba.druid.filter.config.ConfigTools Win2021@ >miyao.txt
```
