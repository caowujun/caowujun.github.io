# springboot 学习

_made by caowujun,2022.3.19_

---

[toc]

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

- 问题

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

## 3.mybatis-plus 自动生成代码

[https://blog.csdn.net/qq_42682745/article/details/124829925](参考资料1)
[https://blog.csdn.net/qq_42682745/article/details/124906331](参考资料2)

easy code 目前的版本不够新，默认返回的 R 还是上个版本的，研究了下 mybatis-plus 自带的 3.5.1 及以上版本可用的自动生成。
适用版本：mybatis-plus-generator 3.5.1 及其以上版本，对历史版本不兼容！
另外自带的 dto 生成代码有问题，它生成 dto 的路径是/entityname/dto.java。
所以我重写了 FreemarkerTemplateEngineExtend，见 3.2

### 3.1 总图

![自动创建代码](/images/springboot/4.png)

- generate 文件夹是自动生成目标路径
- MybatisPlusCodeAutoGenerator 自动生成代码
- resources/templates ftl 文件是自动生成代码用的模板，供用户自定义修改，如果用默认的这里就不用加了。
- .vm 为 velocity 引擎的，.ftl 为 freemarker

### 3.2 各部分代码

扩展 FreemarkerTemplateEngineExtend，兼顾 transfer 代码

```java
 package com.example.bootmaven.autoGenerateCode;

import com.baomidou.mybatisplus.generator.config.OutputFile;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

import java.io.File;
import java.util.Map;

/**
 * @author robin
 * @date 2022年06月27日 13:28
 * @description
 */
public class FreemarkerTemplateEngineExtend extends FreemarkerTemplateEngine {


    /*
     * @author robin
     * @description mybatis-plus自动生成自定义模板class，支持多个，下面就是有vo和 vo转换类
     * @date 2022/6/29 8:09
     * @param customFile
     * @param tableInfo
     * @param objectMap
     */
    @Override
    protected void outputCustomFile(Map<String, String> customFile, TableInfo tableInfo, Map<String, Object> objectMap) {
//        super.outputCustomFile(customFile, tableInfo, objectMap);
        String entityName = tableInfo.getEntityName();
        String otherPath = this.getPathInfo(OutputFile.other);

        customFile.forEach((key, value) -> {
            String fileName;
            if (key.contains("VO")) {
                fileName = String.format(otherPath + File.separator + "%sVO.java", entityName);
            } else {
                fileName = String.format(otherPath + File.separator + "votransfer" + File.separator + "%sTransfer.java", entityName);
            }
            this.outputFile(new File(fileName), objectMap, value);
        });
    }
}


```

自动生成代码

```java
package com.example.bootmaven.autoGenerateCode;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.generator.FastAutoGenerator;
import com.baomidou.mybatisplus.generator.config.OutputFile;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.example.bootmaven.BaseController;

import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

//import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

/**
 * @author robin
 * @date 2022年06月21日 11:15
 */
public class MybatisPlusCodeAutoGenerator {

    /*
     * @author robin
     * @description 自动生成main函数
     * @date 2022/6/29 8:11
     * @param args
     */
    public static void main(String[] args) {

        String projectPath = System.getProperty("user.dir");//D:\workstation\boot-maven
        FastAutoGenerator.create("jdbc:mysql://192.168.109.128:3306/robin?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8&useSSL=false",
                "root", "Win2003@")
                .globalConfig(builder -> {
                    builder.author("robin") // 设置作者
                            .enableSwagger() // 开启 swagger 模式
                            .fileOverride() // 覆盖已生成文件
                            .outputDir(projectPath + "/generator"); // 指定输出目录
                })
                .templateConfig(builder -> {
                    builder.controller("/templates/controller.java").entity("/templates/entity.java");
                })
                .packageConfig(builder -> {
                    builder.parent("com.example.bootmaven") // 设置父包名
                            .mapper("dao")
                            .other("vo")
                            .pathInfo(Collections.singletonMap(OutputFile.mapperXml, projectPath + "/generator/resources/xml")); // 设置mapperXml生成路径
                })
                .strategyConfig(builder -> {
                    builder.enableSkipView()
                            .addInclude("customconfig", "enumtypes", "menu", "money", "noticeboard", "role", "rolemenu", "roleuser", "runninglog", "fire", "sysuser")
                            .entityBuilder()
                            .enableTableFieldAnnotation()//开启生成实体时生成字段注解
                            .naming(NamingStrategy.underline_to_camel)//数据库表映射到实体的命名策略
                            .enableLombok()//开启 lombok 模型
                            .controllerBuilder()
                            .superClass(BaseController.class)//设置父类
                            .enableRestStyle()//开启生成@RestController 控制器
                            .mapperBuilder()
                            .superClass(BaseMapper.class)//设置父类
                            .enableMapperAnnotation()//开启 @Mapper 注解
                            .enableBaseResultMap()//启用 BaseResultMap 生成
                            .enableBaseColumnList()//启用 BaseColumnList
                            .formatMapperFileName("%sDao")//转换 mapper 类文件名称
                            .formatXmlFileName("%sXml"); // 转换 xml 文件名称
                })
                .injectionConfig(builder -> {
                    // DTO，.vm为velocity引擎的，.ftl为freemarker
                    Map<String, String> customFile = new HashMap<>();
                    customFile.put("VO.java", "/templates/entityVO.java.ftl");
                    customFile.put("Transfer.java", "/templates/entityTransfer.java.ftl");
                    //注入数据
                    Map<String, Object> customDataMap = new HashMap<>();
                    customDataMap.put("rootPackage", "com.example.bootmaven");

                    builder.customFile(customFile).customMap(customDataMap);

                })
                .templateEngine(new FreemarkerTemplateEngineExtend()) // 使用Freemarker引擎模板，默认的是Velocity引擎模板
                .execute();
    }
}



```

<font color=red>注意</font>

- customDataMap.put("rootPackage", "com.example.bootmaven");
  可以注入自己想要的数据，比如包名等。

- 这里要注意,前面不需要加什么前缀，默认是 resources 路径下的，后面不加 ftl

```java
  builder.controller( "/templates/controller.java");
```

但是 dto 的时候，必须加 ftl

```java
customFile.put("DTO.java","/templates/entityDTO.java.ftl");
```

我的 2 个模板

- controller

```java
package ${package.Controller};

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.springframework.web.bind.annotation.RequestMapping;
import ${package.Entity}.${entity};
import ${package.Service}.${table.serviceName};
import com.example.bootmaven.tools.response.R;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.*;
import javax.annotation.Resource;
import java.util.List;
import com.example.bootmaven.vo.votransfer.${entity}Transfer;
import com.example.bootmaven.vo.${entity}VO;


<#if restControllerStyle>
<#else>
import org.springframework.stereotype.Controller;
</#if>
<#if superControllerClassPackage??>
import ${superControllerClassPackage};
</#if>

/**
* <p>
* ${table.comment!} 前端控制器
* </p>
*
* @author ${author}
* @since ${date}
*/
<#if restControllerStyle>
@RestController
<#else>
@Controller
</#if>
@RequestMapping("<#if package.ModuleName?? && package.ModuleName != "">/${package.ModuleName}</#if>/<#if controllerMappingHyphenStyle??>${controllerMappingHyphen}<#else>${table.entityPath}</#if>")
<#if kotlin>
class ${table.controllerName}<#if superControllerClass??> : ${superControllerClass}()</#if>
<#else>
<#if superControllerClass??>
public class ${table.controllerName} extends ${superControllerClass} {
<#else>
@Api(tags="${table.controllerName}",value="${table.comment} 前端控制器")
public class ${table.controllerName} {
</#if>

/**
* 服务对象
*/
@Resource
private ${table.serviceName} i${entity}Service;
@Resource
private ${entity}Transfer transfer;

/**
* 分页查询所有数据
*
* @param page 分页对象
* @param entity 查询实体
* @return 所有数据
*/
@GetMapping(value = "page.do")
@ApiOperation(value = "查询${entity}数据列表，带分页", notes = "查询${entity}数据列表，带分页")
public R<Page<${entity}>> page(Page<${entity}> page, ${entity}  entity)
{
try{
return success(this.i${entity}Service.page(page, new QueryWrapper<${entity}>(entity)));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 查询所有数据
*
* @param entity 查询实体
* @return 所有数据
*/
@GetMapping(value = "listAll.do")
@ApiOperation(value = "查询${entity}数据列表,不分页", notes = "查询${entity}数据列表,不分页")
public R<List<${entity}>> listAll(${entity}  entity)
{
try{
return success(this.i${entity}Service.list(new QueryWrapper<${entity}>(entity)));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 通过主键查询单条数据
*
* @param id 主键
* @return 单条数据
*/
@GetMapping("getById.do")
@ApiOperation(value = "根据id获取${entity}数据", notes = "id")
public R<${entity}> getById(String id)
{
try{
return success(this.i${entity}Service.getById(id));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 新增数据
*
* @param entity 实体对象
* @return 新增结果
*/
@PostMapping(value = "insert.do")
@ApiOperation(value = "${entity}数据新增", notes = "${entity}数据新增")
public R<Boolean> save(@RequestBody ${entity} entity)
{
try{
return success(this.i${entity}Service.save(entity));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 修改数据
*
* @param entity 实体对象
* @return 修改结果
*/
@PostMapping(value = "update.do")
@ApiOperation(value = "${entity}数据更新", notes = "{entity}数据更新")
public R<Boolean> update(@RequestBody ${entity} entity) {
try{
return success(this.i${entity}Service.updateById(entity));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 删除数据
*
* @param id 主键
* @return 删除结果
*/
@PostMapping(value = "removeById.do")
@ApiOperation(value = "根据id删除${entity}数据", notes = "根据id删除${entity}数据")
public R<Boolean> removeById(@RequestParam("id") String id) {
try
{
return success(this.i${entity}Service.removeById(id));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}

/**
* 批量删除数据
*
* @param idList 主键集合
* @return 删除结果
*/
@PostMapping(value = "removeByIds.do")
@ApiOperation(value = "批量删除${entity}数据", notes = "批量删除${entity}数据")
public R<Boolean> removeByIds(@RequestParam("idList") List<Long> idList) {
try
{
return success(this.i${entity}Service.removeByIds(idList));
} catch (Exception ex) {
return failed(ex.getMessage());
}
}
}
</#if>

```

- vo

```java
package com.example.bootmaven.vo;

<#list table.importPackages as pkg>
    import ${pkg};
</#list>
<#if swagger>
    import io.swagger.annotations.ApiModel;
</#if>
<#if entityLombokModel>
    import lombok.Data;
    <#if chainModel>
        import lombok.experimental.Accessors;
    </#if>
</#if>

/**
* <p>
    * ${table.comment!}
    * </p>
*
* @author ${author}
* @since ${date}
*/
<#if entityLombokModel>
    @Data
    <#if chainModel>
        @Accessors(chain = true)
    </#if>
</#if>
<#if table.convert>
<#--@TableName("${schemaName}${table.name}")-->
</#if>
<#if swagger>
    @ApiModel(value = "${entity}对象", description = "${table.comment!}")
</#if>
<#if superEntityClass??>
    public class ${entity}VO extends ${superEntityClass}<#if activeRecord><${entity}></#if> {
<#elseif activeRecord>
    public class ${entity}VO extends Model<${entity}> {
<#elseif entitySerialVersionUID>
    public class ${entity}VO implements Serializable {
<#else>
    public class ${entity}VO {
</#if>
<#if entitySerialVersionUID>

    private static final long serialVersionUID = 1L;
</#if>
<#-- ----------  BEGIN 字段循环遍历  ---------->
<#list table.fields as field>
    <#if field.keyFlag>
        <#assign keyPropertyName="${field.propertyName}"/>
    </#if>

    <#if field.comment!?length gt 0>
        <#if swagger>
            @ApiModelProperty("${field.comment}")
        <#else>
            /**
            * ${field.comment}
            */
        </#if>
    </#if>
    <#if field.keyFlag>
    <#-- 主键 -->
        <#if field.keyIdentityFlag>
            @TableId(value = "${field.annotationColumnName}", type = IdType.AUTO)
        <#elseif idType??>
            @TableId(value = "${field.annotationColumnName}", type = IdType.${idType})
        <#elseif field.convert>
            @TableId("${field.annotationColumnName}")
        </#if>
    <#-- 普通字段 -->
    <#elseif field.fill??>
    <#-- -----   存在字段填充设置   ----->
        <#if field.convert>
            @TableField(value = "${field.annotationColumnName}", fill = FieldFill.${field.fill})
        <#else>
            @TableField(fill = FieldFill.${field.fill})
        </#if>
    <#elseif field.convert>
        @TableField("${field.annotationColumnName}")
    </#if>
<#-- 乐观锁注解 -->
    <#if field.versionField>
        @Version
    </#if>
<#-- 逻辑删除注解 -->
    <#if field.logicDeleteField>
        @TableLogic
    </#if>
    private ${field.propertyType} ${field.propertyName};
</#list>
<#------------  END 字段循环遍历  ---------->

<#if !entityLombokModel>
    <#list table.fields as field>
        <#if field.propertyType == "boolean">
            <#assign getprefix="is"/>
        <#else>
            <#assign getprefix="get"/>
        </#if>
        public ${field.propertyType} ${getprefix}${field.capitalName}() {
        return ${field.propertyName};
        }

        <#if chainModel>
            public ${entity} set${field.capitalName}(${field.propertyType} ${field.propertyName}) {
        <#else>
            public void set${field.capitalName}(${field.propertyType} ${field.propertyName}) {
        </#if>
        this.${field.propertyName} = ${field.propertyName};
        <#if chainModel>
            return this;
        </#if>
        }
    </#list>
</#if>

<#if entityColumnConstant>
    <#list table.fields as field>
        public static final String ${field.name?upper_case} = "${field.name}";

    </#list>
</#if>
<#if activeRecord>
    @Override
    public Serializable pkVal() {
    <#if keyPropertyName??>
        return this.${keyPropertyName};
    <#else>
        return null;
    </#if>
    }

</#if>
<#if !entityLombokModel>
    @Override
    public String toString() {
    return "${entity}{" +
    <#list table.fields as field>
        <#if field_index==0>
            "${field.propertyName}=" + ${field.propertyName} +
        <#else>
            ", ${field.propertyName}=" + ${field.propertyName} +
        </#if>
    </#list>
    "}";
    }
</#if>
}


```

- transfer

```java
package com.example.bootmaven.vo.votransfer;

import org.mapstruct.Mapper;
import com.example.bootmaven.vo.${entity}VO;
import ${package.Entity}.${entity};
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import java.util.List;
import org.mapstruct.Named;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

/**
* <p>
* ${table.comment!}
* </p>
*
* @author ${author}
* @since ${date}
*/
@Mapper(componentModel="spring")
public interface ${entity}Transfer
{
<#--${entity}Transfer INSTANCE = Mappers.getMapper(${entity}Transfer.class);-->


@Mappings({
@Mapping(source = "createat", target = "createat", qualifiedByName = "parseString"),
@Mapping(source = "updateat", target = "updateat", qualifiedByName = "parseString")})
${entity}VO d2v(${entity} d);

@Mappings({
@Mapping(source = "createat", target = "createat", qualifiedByName = "parseDate"),
@Mapping(source = "updateat", target = "updateat", qualifiedByName = "parseDate")})
${entity} v2d(${entity}VO v);

/*
* @author robin
* @description 集合转集合
* @date 2022/6/29 8:09
* @param list
*/
List<${entity}> vList2dList(List<${entity}VO> list);
/*
* @author robin
* @description List集合转List集合
* @date 2022/6/29 8:09
* @param list
*/
List<${entity}VO> dList2vList(List<${entity}> list);
/*
* @author robin
* @description IPage集合转IPage集合
* @date 2022/6/29 8:09
* @param list
*/
Page<${entity}VO> dPage2vPage(Page<${entity}> page);
/*
* @author robin
* @description IPage集合转IPage集合
* @date 2022/6/29 8:09
* @param list
*/
Page<${entity}> vPage2dPage(Page<${entity}VO> page);

/*
* @author robin
* @description 不能单独定义成一行，比如 // DateTimeFormatter df=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")
* @date 2022/6/28 15:38
* @param date
* @return java.lang.String
*/
@Named("parseString")
default String parseString(LocalDateTime date) {
try {
return DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").format(date);
} catch (Exception ex) {
return null;
}
}

/*
* @author robin
* @description  不能单独定义成一行，比如 // DateTimeFormatter df=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")
* @date 2022/6/28 15:38
* @param date
* @return java.time.LocalDateTime
*/
@Named("parseDate")
default LocalDateTime parseDate(String date) {
try {
return LocalDateTime.parse(date, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
} catch (Exception ex) {
return null;
}
}
}

```

## 4. 拦截器

参考[https://blog.csdn.net/wangjiabao001/article/details/124433502](https://blog.csdn.net/wangjiabao001/article/details/124433502)

新建一个拦截器 HandlerInterceptorAdapter,功能是未登录的人不能访问 controller

```java
package com.example.bootmaven.config;

import cn.hutool.jwt.JWTUtil;
import com.alibaba.druid.wall.violation.ErrorCode;
import com.example.bootmaven.tools.JsonUtils;
import com.example.bootmaven.tools.response.R;
import org.springframework.context.annotation.Configuration;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author robin
 * @date 2022年06月29日 16:09,https://blog.csdn.net/qq_23107097/article/details/120130490
 * @description
 */
@Configuration
public class HandlerInterceptorAdapter implements HandlerInterceptor {

/*
 * @author robin
 * @description 拦截没有登录的请求，并在没权限的说话返回一个警告
 * @date 2022/6/30 13:46
 * @param request
 * @param response
 * @param handler
 * @return boolean
 */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("Authorization");
        boolean result = StringUtils.hasLength(token) && JWTUtil.verify(token, GlobalValue.TOKEN_SECRET);
        if (result) {
            return true;
        } else {
            // response.setCharacterEncoding("utf-8");//返回？？？使用这一行可以解决,或者用下面这行
            response.setContentType("text/html;charset=utf-8");
            response.getWriter().print(R.failed("没有权限"));
            return false;
        }
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}


```

注册拦截器

```java
package com.example.bootmaven.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;

import javax.annotation.Resource;

/**
 * @author robin
 * @date 2022年06月29日 16:06
 * @description
 */
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {

    @Resource
    HandlerInterceptorAdapter handlerInterceptorAdapter;

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(handlerInterceptorAdapter).addPathPatterns("/**");
        super.addInterceptors(registry);
    }
}

```

当前会拦截所有的访问地址，显示是不正常的，毕竟登录之类的不能拦截。
<font color=red>备注：请看下面一节（swagger3+美化），会对 SpringMvcSupport 进行修改</font>

## 5. swagger3+美化

- 关于依赖,有时候需要 clean 然后 compile 下。刷新下依赖。

```xml
<dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
</dependency>
<dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>knife4j-spring-boot-starter</artifactId>
            <version>3.0.3</version>
</dependency>
```

- 然后发现了个这么事
  [https://doc.xiaominfo.com/knife4j/faq/springboot-404.html](https://doc.xiaominfo.com/knife4j/faq/springboot-404.html)

最新的 swagger 配置文件

```java
package com.example.bootmaven.config;



import io.swagger.annotations.ApiOperation;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

/**
 * @author robin
 * @date 2022年06月21日 10:00
 */
@Configuration
//@ConditionalOnProperty(value = "springfox.documentation.enabled", havingValue = "true", matchIfMissing = true)
@EnableWebMvc
public class Swagger3Config implements WebMvcConfigurer {

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
                .contact(new Contact("robin", "https://caowujun.github.io/", "https://caowujun.github.io/"))
                .version("1.0")
                .build();
    }


}
```

然后是新的 SpringMvcSupport

```java
package com.example.bootmaven.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;

import javax.annotation.Resource;

/**
 * @author robin
 * @date 2022年06月29日 16:06
 * @description
 */
@Configuration
public class SpringMvcSupport implements WebMvcConfigurer {

    @Resource
    HandlerInterceptorAdapter handlerInterceptorAdapter;

    /*
     * @author robin
     * @description 加载拦截器
     * @date 2022/6/30 10:24
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        String[] excludePatterns = new String[]{
                "/sysuser/login.do",
                "/swagger-resources/**",
                "/webjars/**",
//                "/v2/**",
                "/v3/**",
                "/swagger-ui/**",
                "/api",
//                "/api-docs",
//                "/api-docs/**",
                "/doc.html",
                "/doc.html/**"
        };
        registry.addInterceptor(handlerInterceptorAdapter)
                .addPathPatterns("/**")
                .excludePathPatterns(excludePatterns);
    }

    //https://doc.xiaominfo.com/knife4j/faq/springboot-404.html,解决springboot无法访问新的皮肤问题。
    /*
     * @author robin
     * @description 加载资源映射，Swagger3不加这个会报错
     * @date 2022/6/30 10:24
     * @param registry
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/swagger-ui/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/springfox-swagger-ui/");
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static/");
        registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");

    }
}

```

## 6. 登录 token，jwt，hutool 工具。
