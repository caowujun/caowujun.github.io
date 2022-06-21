# springboot 学习

_made by caowujun,2022.3.19_

---

## 1. Springboot 项目创建

按图示依次创建
![新建springboot](/images/springboot/1.png)
![新建springboot](/images/springboot/2.png)
![新建springboot](/images/springboot/3.png)
<font color=red>更正:不要加入 lombok，一直提示错误。</font>
pom 文件增加几个依赖

```xml
<!-- mybatisplus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!-- 数据库加密 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.4</version>
        </dependency>
<!-- swagger -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>
```

增加 swagger config

```java

@Configuration
@EnableOpenApi
@EnableWebMvc
public class Swagger3Config implements WebMvcConfigurer {
    /**
     * 是否开启swagger配置，生产环境需关闭
     */
    @Value("${swagger.enabled}")
    private boolean enable;
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .select() // 指定需要发布到Swagger的接口目录，不支持通配符
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Swagger3接口文档")
                .description("SpringBoot后台接口")
//                .contact(new Contact("Ray。", "http://www.ruiyeclub.cn", "ruiyeclub@foxmail.com"))
                .version("1.0")
                .build();
    }


    @SafeVarargs
    private final <T> Set<T> newHashSet(T... ts) {
        if (ts.length > 0) {
            return new LinkedHashSet<>(Arrays.asList(ts));
        }
        return null;
    }

    private List<Response> getGlobalResonseMessage()
    {
        List<Response> responseList=new ArrayList<>();
        responseList.add(new ResponseBuilder().code("404").description("找不到资源").build());
        return responseList;
    }
}
```

###问题 1

```json
Failed to start bean 'documentationPluginsBootstrapper'; nested exception is
```

解决：在 swaggerConfig 类上加@EnableWebMvc.上面代码上已经修正果的。

## 2. druid 数据库加密

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
 java -cp druid-1.2.4.jar com.alibaba.druid.filter.config.ConfigTools Win2003@ >miyao.txt
```
