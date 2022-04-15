# springboot学习
_made by caowujun,2022.3.19_

---  
## 1. Springboot项目创建
按图示依次创建
![新建springboot](/images/springboot/1.png)
![新建springboot](/images/springboot/2.png)
![新建springboot](/images/springboot/3.png)
pom文件增加几个依赖
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

增加swagger config
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
###问题1
```json
Failed to start bean 'documentationPluginsBootstrapper'; nested exception is
```
解决：在swaggerConfig类上加@EnableWebMvc.上面代码上已经修正果的。