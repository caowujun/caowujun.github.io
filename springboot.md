# springboot 学习

_made by caowujun,2022.3.19_

---

[toc]

## 1. Springboot 项目创建

按图示依次创建
![新建springboot](/images/springboot/1.png)
![新建springboot](/images/springboot/2.png)
![新建springboot](/images/springboot/3.png)
<font color=red>更正:上面的图里不要加入 lombok，一直提示错误。</font>
pom 文件增加几个依赖(初始的时候比较少，后来随着我们功能越来越多，这里的 pom 依赖也越多，直接放一个总的)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>boot-maven</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>boot-maven</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--            去除自带的log-->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- mysql 依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!-- mybatis plus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!-- mybatis plus 自动生成代码-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!-- mybatis plus 自动生成代码用的引擎 -->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
        </dependency>
        <!-- druid数据库加密 -->
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
        <!-- swagger 皮肤扩展-->
        <dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>knife4j-spring-boot-starter</artifactId>
            <version>3.0.3</version>
        </dependency>

        <!-- lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>

        <!--VO,DO转换工具-->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>1.5.1.Final</version>
        </dependency>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.5.1.Final</version>
        </dependency>
        <!--Hutool是一个小而全的Java工具类库-->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.8.4</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j</artifactId>
            <version>1.3.8.RELEASE</version>
        </dependency>
        <!--db-flway-->
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-core</artifactId>
            <version>6.1.0</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.flywaydb</groupId>
                <artifactId>flyway-maven-plugin</artifactId>
                <version>5.1.1</version>
                <configuration>
                    <url>jdbc:mysql://ip:3306/db?serverTimezone=Asia/Shanghai&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;useSSL=false
                    </url>
                    <user>username</user>
                    <password>pwd</password>
                    <driver>com.mysql.cj.jdbc.Driver</driver>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

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
server:
  port: 5015
  servlet:
    context-path: /boot/
spring:
  application:
    name: boot
  main:
    allow-bean-definition-overriding: true
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://ip:3306/robin?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8&useSSL=false
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: username
    password: 加密后的pwd
    druid:
      filter:
        config:
          enabled: true
      connection-properties: config.decrypt=true;config.decrypt.key=MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAJjVoNjug97bUT80hzrM761GNC4un1csTEff6QnPj4CAgWLDLYyuwoVfi+ll0+iMZRYR04gn52gE39H10BIOSrkCAwEAAQ==
  flyway:
    enabled: true
    baseline-on-migrate: true
    clean-disabled: true
    encoding: utf-8
    locations: classpath：db/migration
    sql-migration-prefix: V
    sql-migration-separator: __
    sql-migration-suffixes: .sql
```

密钥生成命令，跳转到 druid 的 jar 包下面，打开 cmd

```bash
 java -cp druid-1.2.4.jar com.alibaba.druid.filter.config.ConfigTools userpwd >miyao.txt
```

## 3. mybatis-plus 自动生成代码

[https://blog.csdn.net/qq_42682745/article/details/124829925](参考资料1)
[https://blog.csdn.net/qq_42682745/article/details/124906331](参考资料2)

easy code 目前的版本不够新，默认返回的 R 还是上个版本的，研究了下 mybatis-plus 自带的 3.5.1 及以上版本可用的自动生成。(我把 R 从上个版本抽了出来，毕竟觉得挺好用的)
适用版本：mybatis-plus-generator 3.5.1 及其以上版本，对历史版本不兼容！
另外自带的 dto 生成代码有问题，它生成 dto 的路径是/entityname/dto.java。所以我重写了 FreemarkerTemplateEngineExtend，见 3.2.

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


    /*
     * @author robin
     * @description 重写write方法这种方式适用于定义需要处理plus自带数据才能得到的数据。比如依赖注入的对象名首字母小写，这种在配置map时是无法处理的，因为plus的默认数据都还没有
     * @date 2022/7/4 8:40
     * @param objectMap
     * @param templatePath
     * @param outputFile
     */
    @Override
    public void writer(Map<String, Object> objectMap, String templatePath, File outputFile) throws Exception {
        TableInfo tableInfo = (TableInfo) objectMap.get("table");
        String entityName = tableInfo.getEntityName();
        String lowerEntityName = entityName.substring(0, 1).toLowerCase() + entityName.substring(1);
        objectMap.put("lowerEntityName", lowerEntityName);
        super.writer(objectMap, templatePath, outputFile);
    }
}


```

自动生成代码

```java
package com.example.bootmaven.autoGenerateCode;

import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.generator.FastAutoGenerator;
import com.baomidou.mybatisplus.generator.config.OutputFile;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.fill.Column;
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
                            .addTableFills(new Column("createat", FieldFill.INSERT))
                            .addTableFills(new Column("updateat", FieldFill.UPDATE))
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
package com.example.bootmaven.config.interceptor;

import cn.hutool.core.date.DateTime;
import cn.hutool.core.date.DateUnit;
import cn.hutool.core.date.DateUtil;
import cn.hutool.jwt.JWT;
import cn.hutool.jwt.JWTUtil;
import cn.hutool.jwt.JWTValidator;
import cn.hutool.log.StaticLog;
import com.example.bootmaven.config.GlobalValue;
import com.example.bootmaven.response.R;
import org.springframework.context.annotation.Configuration;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.text.SimpleDateFormat;
import java.util.Date;

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
     * 调用时间：Controller方法处理之前,执行顺序：链式Intercepter情况下，Intercepter按照声明的顺序一个接一个执行,若返回false，则中断执行，注意：不会进入afterCompletion
     * @date 2022/6/30 13:46
     * @param request
     * @param response
     * @param handler
     * @return boolean
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("Authorization");
        DateTime expire_time = DateUtil.date((Long) JWTUtil.parseToken(token).getPayload("expire_time"));
        boolean result = StringUtils.hasLength(token) && JWTUtil.verify(token, GlobalValue.TOKEN_SECRET) && expire_time.after(DateUtil.date());
        //        JWTUtil.parseToken(token).validate(0)一直等于false，不知道为什么
        if (result) {
            return true;
        } else {
            // response.setCharacterEncoding("utf-8");//返回？？？使用这一行可以解决,或者用下面这行
            response.setContentType("text/html;charset=utf-8");
            response.getWriter().print(R.failed("没有权限"));
            StaticLog.info("您没有权限，拒绝访问!.", "WARN");
            return false;
        }
    }

    /*
    调用前提：preHandle返回true
    调用时间：Controller方法处理完之后，DispatcherServlet进行视图的渲染之前，也就是说在这个方法中你可以对ModelAndView进行操作
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    /*
    调用前提：preHandle返回true
    调用时间：DispatcherServlet进行视图的渲染之后
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}


```

注册拦截器

```java
package com.example.bootmaven.config.interceptor;

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


    //过滤器与拦截器原理    https://blog.csdn.net/weixin_45433031/article/details/125614080
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
                "/v2/**",
                "/v3/**",
                "/swagger-ui/**",
                "/api",
                "/api-docs",
                "/api-docs/**",
                "/doc.html",
                "/doc.html/**",
                "***/error"
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
        registry.addResourceHandler("doc.html")
                .addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");

    }
}


```

当前会拦截所有的访问地址，显示是不正常的，毕竟登录之类的不能拦截。
<font color=red>备注：请看下面一节（swagger3+美化），会对 SpringMvcSupport 进行修改</font>

## 5. swagger3+美化

- 关于依赖,有时候需要 clean 然后 compile 下。刷新下依赖。

```xml
        <!-- swagger -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>
        <!-- swagger 皮肤扩展-->
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

- 问题

```json
Failed to start bean 'documentationPluginsBootstrapper'; nested exception is
```

解决：在 swaggerConfig 类上加@EnableWebMvc.上面代码上已经修正过的。

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

访问地址：http://localhost:5015/boot/doc.html#/home

## 6. 登录 token，jwt，hutool 工具。

原来是自己写 jwt 工具，后来发现 hutool 已经继承了，直接上代码

```java
    public  static final  byte[] TOKEN_SECRET = "your_privatekey".getBytes();

```

```java
  /*
     * @author robin
     * @description 登录,expire_time的效果未验证，总感觉超过2小时了还没失效，难道 要自己写验证？
     * @date 2022/6/29 16:55
     * @param sysuser
     * @param request
     * @param response
     * @return com.example.bootmaven.test.R<?>
     */
    @PostMapping(value = "login.do")
    @ApiOperation(value = "登录", notes = "根据账号密码登录")
    public R<?> login(@RequestBody Sysuser sysuser, HttpServletRequest request, HttpServletResponse response) {
        try {
            //1fa65854-92aa-42a9-b16b-ffdb6a68ae91
            sysuser = iSysuserService.getById(sysuser.getId());
            if (null != sysuser) {
                Sysuser finalSysuser = sysuser;
                Map<String, Object> map = new HashMap<String, Object>() {
                    private static final long serialVersionUID = 1L;
                    {
                        put("id", finalSysuser.getId());
                        put("loginname", finalSysuser.getUsername());
                        put("cnname", finalSysuser.getCnname());
                        put("expire_time", System.currentTimeMillis() + 120 * 60 * 1000);
                    }
                };
                String token = JWTUtil.createToken(map, GlobalValue.TOKEN_SECRET);
                System.out.print(token);
                response.addHeader("token", token);

                return success(transfer.d2v(sysuser));
            } else {
                return success(false);
            }
        } catch (Exception ex) {
            return failed(ResponseCode.FAILED);
        }
    }
```

拦截器见 #4 章

## 7. 日志

hutool 集成了日志输出的方法

- pom,增加 spring-boot-starter-web 中增加 exclusions 节点，引入 log4j

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--            去除自带的log-->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

                <!--log4j-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j</artifactId>
            <version>1.3.8.RELEASE</version>
        </dependency>

```

- log4j.properties

```properties
log4j.rootLogger=INFO,DEBUG,ERROR, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %p %c{2}: %m%n

#----------------------------文件存在?目根目?下-----------------------#
#----------------------------------------------------------------------------------------------------
#?出INFO??以上的内容到info.log中
log4j.logger.INFO=INFO
log4j.appender.INFO = org.apache.log4j.DailyRollingFileAppender
log4j.appender.INFO.File = logs/supersiver/objects/info.log
log4j.appender.INFO.Threshold = INFO
log4j.appender.INFO.Append = true
log4j.appender.INFO.layout = org.apache.log4j.PatternLayout
log4j.appender.INFO.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
#-----------------------------------------------------------------------------------------------------
#?出DEBUG ??以上的日志到debugger.log
log4j.logger.DEBUG=DEBUG
log4j.appender.DEBUG = org.apache.log4j.DailyRollingFileAppender
log4j.appender.DEBUG.File = logs/supersiver/objects/debugger.log
log4j.appender.DEBUG.Threshold = DEBUG
log4j.appender.DEBUG.Append = true
log4j.appender.DEBUG.layout = org.apache.log4j.PatternLayout
log4j.appender.DEBUG.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
#-----------------------------------------------------------------------------------------------------
#?出ERROR ??以上的日志到error.log
log4j.logger.ERROR=ERROR
log4j.appender.ERROR = org.apache.log4j.DailyRollingFileAppender
log4j.appender.ERROR.File =logs/supersiver/objects/error.log
log4j.appender.ERROR.Threshold = ERROR
log4j.appender.ERROR.Append = true
log4j.appender.ERROR.layout = org.apache.log4j.PatternLayout
log4j.appender.ERROR.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```

然后代码中使用

```java
StaticLog.info("您所访问的页面请求中有违反安全规则元素存在，拒绝访问!.", "ERROR");
或者
//推荐创建不可变静态类成员变量
private static final Log log = LogFactory.get();
log.log(Level.INFO, "init");

```

## 8. flyway

[原文链接：https://blog.csdn.net/qq_41378597/article/details/124134146](原文链接：https://blog.csdn.net/qq_41378597/article/details/124134146)

### 8.1 工作流程：

- 项目启动，应用程序完成数据库连接池的建立后，Flyway 自动运行。
- 初次使用时，flyway 会创建一个 flyway_schema_history 表，用于记录 sql 执行记录。
- Flyway 会扫描项目指定路径下(默认是 classpath:db/migration )的所有 sql 脚本，与 flyway_schema_history 表脚本记录进行比对。如果数据库记录执行过的脚本记录，与项目中的 sql 脚本不一致，Flyway 会报错并停止项目执行。
- 如果校验通过，则根据表中的 sql 记录最大版本号，忽略所有版本号不大于该版本的脚本。再按照版本号从小到大，逐个执行其余脚本。
- flyway 执行 migrate 必须在空白的数据库上进行，否则报错。
- 对于已经有数据的数据库，必须先 baseline，然后才能 migrate。
- clean 操作是删除数据库的所有内容，包括 baseline 之前的内容。
- 尽量不要修改已经执行过的 SQL，即便是 R 开头的可反复执行的 SQL，它们会不利于数据迁移。
- 当需要做数据迁移的时候，更换一个新的空白数据库，执行下 migrate 命令，所有的数据库更改都可以一步到位地迁移过去

### 8.2 代码

- pom 文件新增

```xml
        <!--db-flway-->
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-core</artifactId>
            <version>6.1.0</version>
        </dependency>
```

- 另外 pom 文件的 plugin 增加

```xml
 <plugin>
                <groupId>org.flywaydb</groupId>
                <artifactId>flyway-maven-plugin</artifactId>
                <version>5.1.1</version>
                <configuration>
                    <url>jdbc:mysql://192.168.109.128:3306/robin?serverTimezone=Asia/Shanghai&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;useSSL=false
                    </url>
                    <user>root</user>
                    <password>Win2003@</password>
                    <driver>com.mysql.cj.jdbc.Driver</driver>
                </configuration>
            </plugin>
```

- yml 增加配置

```yml
flyway:
  enabled: true
  baseline-on-migrate: true
  clean-disabled: true
  encoding: utf-8
  locations: classpath：db/migration
  sql-migration-prefix: V
  sql-migration-separator: __
  sql-migration-suffixes: .sql
```

- resource 目录下建立文件夹 db/migration

界面如图
![flyway](/images/springboot/5.png)

![flyway sql](/images/springboot/6.png)

1. V0 是 ddl。
2. V1 是基本数据初始化。
3. 如果后续有增加，按版本号往上+1。

## 9. 过滤器

参考[https://blog.csdn.net/qq_23107097/article/details/120130490](https://blog.csdn.net/qq_23107097/article/details/120130490)

- XssAndSqlHttpServletRequestWrapper

```java
package com.example.bootmaven.config.filter;

import cn.hutool.core.util.StrUtil;
import cn.hutool.http.HtmlUtil;
import cn.hutool.json.JSON;
import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;
import com.google.common.base.Charsets;
import org.apache.commons.lang3.StringUtils;

import javax.servlet.ReadListener;
import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.io.*;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.regex.Pattern;

/**
 * @author robin
 * @date 2022年07月04日 9:57
 * @description
 */
public class XssAndSqlHttpServletRequestWrapper extends HttpServletRequestWrapper {

    HttpServletRequest httpServletRequest = null;
    private String body;
    private static final Set<String> notAllowedKeyWords = new HashSet<String>(0);
    static {
        String key = "and|exec|insert|select|delete|update|count|*|%|chr|mid|master|truncate|char|declare|;|or|-|+";
        String[] keyStr = key.split("\\|");
        notAllowedKeyWords.addAll(Arrays.asList(keyStr));
    }

    public XssAndSqlHttpServletRequestWrapper(HttpServletRequest request) throws IOException {
        super(request);
        httpServletRequest = request;
        StringBuilder stringBuilder = new StringBuilder();
        BufferedReader bufferedReader = null;
        try {
            InputStream inputStream = request.getInputStream();
            if (inputStream != null) {
                bufferedReader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8));
                char[] charBuffer = new char[128];
                int bytesRead = -1;
                while ((bytesRead = bufferedReader.read(charBuffer)) > 0) {
                    stringBuilder.append(charBuffer, 0, bytesRead);
                }
            } else {
                stringBuilder.append("");
            }
        } catch (IOException ex) {
            throw ex;
        } finally {
            if (bufferedReader != null) {
                try {
                    bufferedReader.close();
                } catch (IOException ex) {
                    throw ex;
                }
            }
        }
        body = stringBuilder.toString();
    }


    @Override
    public String getParameter(String name) {
        String value = super.getParameter(name);
        if (!StrUtil.hasEmpty())
            value = HtmlUtil.filter(value);
        return value;
    }

    @Override
    public String[] getParameterValues(String name) {
        String[] values = super.getParameterValues(name);
        if (null != values) {
            for (int i = 0, l = values.length; i < l; i++) {
                values[i] = !StrUtil.hasEmpty(values[i]) ? HtmlUtil.filter(values[i]) : values[i];
            }
        }
        return values;
    }

    @Override
    public Map<String, String[]> getParameterMap() {
        Map<String, String[]> parameters = super.getParameterMap();
        Map<String, String[]> map = new LinkedHashMap<>();
        if (parameters != null) {
            for (String key : parameters.keySet()) {
                String[] values = parameters.get(key);
                for (int i = 0, l = values.length; i < l; i++) {
                    values[i] = !StrUtil.hasEmpty(values[i]) ? HtmlUtil.filter(values[i]) : values[i];
                }
                map.put(key, values);
            }
        }
        return map;
    }

    @Override
    public String getHeader(String name) {
        String value = super.getHeader(name);
        if (!StrUtil.hasEmpty())
            value = HtmlUtil.filter(value);
        return value;
    }

    public HttpServletRequest getHttpServletRequest() {
        return httpServletRequest;
    }


    public static String stripXSSAndSql(String value) {
        if (value != null) {
// NOTE: It's highly recommended to use the ESAPI library and
// uncomment the following line to
// avoid encoded attacks.
// value = ESAPI.encoder().canonicalize(value);
// Avoid null characters
/** value = value.replaceAll("", ""); ***/
// Avoid anything between script tags
            Pattern scriptPattern = Pattern.compile(
                    "<[\r\n| | ]*script[\r\n| | ]*>(.*?)<!--[\r\n| | ]*script[\r\n| | ]*-->", Pattern.CASE_INSENSITIVE);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid anything in a
// src="http://www.yihaomen.com/article/java/..." type of
// e-xpression
            scriptPattern = Pattern.compile("src[\r\n| | ]*=[\r\n| | ]*[\\\"|\\\'](.*?)[\\\"|\\\']",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            value = scriptPattern.matcher(value).replaceAll("");
// Remove any lonesome tag
            scriptPattern = Pattern.compile("<!--[\r\n| | ]*script[\r\n| | ]*-->", Pattern.CASE_INSENSITIVE);
            value = scriptPattern.matcher(value).replaceAll("");
// Remove any lonesome <script ...> tag
            scriptPattern = Pattern.compile("<[\r\n| | ]*script(.*?)>",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid eval(...) expressions
            scriptPattern = Pattern.compile("eval\\((.*?)\\)",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid e-xpression(...) expressions
            scriptPattern = Pattern.compile("e-xpression\\((.*?)\\)",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid javascript:... expressions
            scriptPattern = Pattern.compile("javascript[\r\n| | ]*:[\r\n| | ]*", Pattern.CASE_INSENSITIVE);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid vbscript:... expressions
            scriptPattern = Pattern.compile("vbscript[\r\n| | ]*:[\r\n| | ]*", Pattern.CASE_INSENSITIVE);
            value = scriptPattern.matcher(value).replaceAll("");
// Avoid onload= expressions
            scriptPattern = Pattern.compile("onload(.*?)=",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            value = scriptPattern.matcher(value).replaceAll("");
        }
        return value;
    }

    public boolean checkXSSAndSql(String value) {
        boolean flag = false;
        if (value != null) {
// NOTE: It's highly recommended to use the ESAPI library and
// uncomment the following line to
// avoid encoded attacks.
// value = ESAPI.encoder().canonicalize(value);
// Avoid null characters
/** value = value.replaceAll("", ""); ***/
// Avoid anything between script tags
            Pattern scriptPattern = Pattern.compile(
                    "<[\r\n| | ]*script[\r\n| | ]*>(.*?)</[\r\n| | ]*script[\r\n| | ]*>", Pattern.CASE_INSENSITIVE);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid anything in a
// src="http://www.yihaomen.com/article/java/..." type of
// e-xpression
            scriptPattern = Pattern.compile("src[\r\n| | ]*=[\r\n| | ]*[\\\"|\\\'](.*?)[\\\"|\\\']",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Remove any lonesome </script> tag
            scriptPattern = Pattern.compile("<!--[\r\n| | ]*script[\r\n| | ]*-->", Pattern.CASE_INSENSITIVE);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Remove any lonesome <script ...> tag
            scriptPattern = Pattern.compile("<[\r\n| | ]*script(.*?)>",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid eval(...) expressions
            scriptPattern = Pattern.compile("eval\\((.*?)\\)",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid e-xpression(...) expressions
            scriptPattern = Pattern.compile("e-xpression\\((.*?)\\)",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid javascript:... expressions
            scriptPattern = Pattern.compile("javascript[\r\n| | ]*:[\r\n| | ]*", Pattern.CASE_INSENSITIVE);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid vbscript:... expressions
            scriptPattern = Pattern.compile("vbscript[\r\n| | ]*:[\r\n| | ]*", Pattern.CASE_INSENSITIVE);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
// Avoid onload= expressions
            scriptPattern = Pattern.compile("onload(.*?)=",
                    Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL);
            flag = scriptPattern.matcher(value).find();
            if (flag) {
                return flag;
            }
            flag = checkSqlKeyWords(value);
        }
        return flag;
    }
    public boolean checkSqlKeyWords(String value) {
        for (String keyword : notAllowedKeyWords) {
            if (value.length() > keyword.length() + 4
                    && (value.contains(" " + keyword) || value.contains(keyword + " ") || value.contains(" " + keyword + " "))) {
//                log.error(this.getRequestURI() + "，参数异常，包含sql的关键词(" + keyword + ")");
                return true;
            }
        }
        return false;
    }
    public final boolean checkParameter() {
        Map<String, String[]> submitParams = new HashMap(super.getParameterMap());
        Set<String> submitNames = submitParams.keySet();
        for (String submitName : submitNames) {
            Object submitValues = submitParams.get(submitName);
            if ((submitValues != null)) {
                for (String submitValue : (String[]) submitValues) {
                    if (checkXSSAndSql(HtmlUtil.filter(submitValue))) {
                        return true;
                    }
                }
            }
        }
        return false;
    }

    public String getBody() {
        return this.body;
    }

    /*
     * @author robin
     * @description
     * @date 2022/7/4 11:19
     * @return java.io.BufferedReader
     */
    @Override
    public BufferedReader getReader() throws IOException {
        return new BufferedReader(new InputStreamReader(this.getInputStream(), StandardCharsets.UTF_8));
    }
    @Override
    public ServletInputStream getInputStream() throws IOException {
        final ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(body.getBytes(StandardCharsets.UTF_8));
        ServletInputStream servletInputStream = new ServletInputStream() {
            @Override
            public boolean isFinished() {
                return false;
            }
            @Override
            public boolean isReady() {
                return false;
            }
            @Override
            public void setReadListener(ReadListener readListener) {
            }
            @Override
            public int read() throws IOException {
                return byteArrayInputStream.read();
            }
        };
        return servletInputStream;
    }
    /**
     * 设置自定义post参数 //
     *
     * @param paramMaps
     * @return
     */
    public void setParamsMaps(Map paramMaps) {
        Map paramBodyMap = new HashMap<String,Object>();
        if (!StringUtils.isEmpty(body)) {
            paramBodyMap =JSONUtil.toBean(body, Map.class);
        }
        paramBodyMap.putAll(paramMaps);
        body = JSONUtil.toJsonStr(paramBodyMap);
    }
}


```

- XssAndSqlFilter

```java
package com.example.bootmaven.config.filter;


import cn.hutool.json.JSONUtil;
import cn.hutool.log.Log;
import cn.hutool.log.LogFactory;
import cn.hutool.log.StaticLog;
import cn.hutool.log.level.Level;
import com.example.bootmaven.config.GlobalValue;
import com.example.bootmaven.response.R;
import org.apache.commons.lang3.StringUtils;
import org.springframework.stereotype.Component;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

/**
 * @author robin
 * @date 2022年07月04日 12:25
 * @description
 */

@WebFilter(urlPatterns = "/*", filterName = "xssAndSqlFilter", dispatcherTypes = DispatcherType.REQUEST)
@Component
public class XssAndSqlFilter implements Filter {
    //推荐创建不可变静态类成员变量
    private static final Log log = LogFactory.get();

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.log(Level.INFO, "init");
    }

    //    1. GET 传递, 参数可以直接通过request.getParameter获取。
//    2. Post 传递,产生不能过直接从request.getInputStream() 读取，必须要进行重新写。（request.getInputStream()只能够读取一次）
//    方式：通过重写 HttpServletRequestWrapper 类 获取getInputStream中的流数据，然后在将body数据进行重新写入传递下去。
    /*
     * @author robin
     * @description
     *
     * @date 2022/7/6 14:04
     * @param request
     * @param response
     * @param chain
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        ServletOutputStream outputStream = response.getOutputStream();
        String method = "GET";//设置初始值
        XssAndSqlHttpServletRequestWrapper xssRequest = null;
        if (request instanceof HttpServletRequest) {//判断左边的对象是否是它右边对象的实例
            method = ((HttpServletRequest) request).getMethod();//得到请求URL地址时使用的方法
            // 防止流读取一次后就没有了, 所以需要将流继续写出去
            xssRequest = new XssAndSqlHttpServletRequestWrapper((HttpServletRequest) request);//创建对象
        }
        TreeMap paramsMaps = new TreeMap();

        if ("POST".equalsIgnoreCase(method)) {//判断是否为post

            String body = xssRequest.getBody();
            paramsMaps = JSONUtil.toBean(body, TreeMap.class);
            if (StringUtils.isNotBlank(body) && (xssRequest.checkXSSAndSql(body) || xssRequest.checkParameter())) {//等价于 str != null && str.length > 0 && str.trim().length > 0
                throwError(outputStream, body);
            } else {
                xssRequest.setParamsMaps(paramsMaps);
                chain.doFilter(xssRequest, response);
            }

        } else if (method.equals("GET")) {
            assert xssRequest != null;
            if (xssRequest.checkParameter())
                throwError(outputStream, "");
            else
                chain.doFilter(xssRequest, response);
//            assert xssRequest != null;
//            Map<String, String[]> parameterMap = xssRequest.getParameterMap();
//            Set<Map.Entry<String, String[]>> entries = parameterMap.entrySet();
//            Iterator<Map.Entry<String, String[]>> iterator = entries.iterator();
//            while (iterator.hasNext()) {
//                Map.Entry<String, String[]> next = iterator.next();
//                paramsMaps.put(next.getKey(), next.getValue()[0]);
//            }
        } else {
            chain.doFilter(xssRequest, response);
        }
    }


    public static void throwError(ServletOutputStream outputStream, String param) throws IOException {
        StaticLog.info("您所访问的页面请求中有违反安全规则元素存在，拒绝访问!.[]==" + param, "ERROR");
        outputStream.write("您所访问的页面请求中有违反安全规则元素存在，拒绝访问!.".getBytes());
        outputStream.flush();
    }

    // 获取request请求body中参数
    public static String getBodyString(BufferedReader br) {
        String inputLine;
        StringBuilder str = new StringBuilder();
        try {
            while ((inputLine = br.readLine()) != null) {
                str.append(inputLine);
            }
            br.close();
        } catch (IOException e) {
            System.out.println("IOException: " + e);
        }
        return str.toString();
    }


    @Override
    public void destroy() {

    }
}

```

注意：

- 原先网上参考，有异常参数是用的 return;但是有个问题，login.do 这个本应该不被拦截的 action，发现最后走了拦截器。改为 outputStream.flush();后没这个问题。
- 发现但凡调用 request.getInputStream()，它后续都会报错/error，然后跑到拦截器了。后来参考了[https://blog.csdn.net/fuwenshen/article/details/90203395]

```
Post 传递的产生不能过直接从request.getInputStream() 读取，必须要进行重新写。（request.getInputStream()只能够读取一次）

方式： 通过重写 HttpServletRequestWrapper 类 获取getInputStream中的流数据，然后在将body数据进行重新写入传递下去。
```

所以在 POST 的时候，通过 xssRequest.setParamsMaps(paramsMaps);重新把参数传入。最后调用 chain.doFilter(xssRequest, response);

## 10. 注解验证

| 注解            | 作用                                                        |
| --------------- | ----------------------------------------------------------- | --- |
| @NotNull        | 参数不能为 null                                             |
| @NotBlank       | 参数值不不为 null，且去除首尾空格后长度不为 0，多于用字符串 |
| @NotEmpty       | 参数不为 null 且不为空，字符串长度不为 0、集合大小不为 0    |
| @Size(max,min)  | 参数字符长度必须在 min 到 max 之间                          |
| @Pattern(value) | 参数必须符合指定的正则表达式                                |
| @Null           | 参数只能为 null                                             |     |
| @Email          | 参数值是邮箱格式，也可以通过@Pattern 自定义正则表达式来实现 |
| @Past           | 参数必须是过去的日期                                        |
| @Future         | 参数必须是未来的日期                                        |
| @Max(value)     | 参数不能大于 value 的值                                     |
| @Min(value)     | 参数不能小于 value 的值                                     |

————————————————
原文链接：https://blog.csdn.net/weixin_39025362/article/details/108387606

## 11. 访问 application.properties

前提：新建了 GlobalValue.class，希望能在这统一管理

新增 application.properties 文件

```properties
globle.token.secret=robin_privatekey
globle.token.expiretime=1000*60*2
```

### 11.1 第一个

```java
//application.properties可以不加下面这一行
// @PropertySource("classpath:application.properties")
// @Component
// @Data
@Service
public class GlobalValue {

    @Value("${globle.token.secret}")
    private String TOKEN_SECRET;

    @Value("${globle.token.expiretime}")
    private String TOKEN_EXPIRETIME;
}

```

在使用类

```java
@Resource
private GlobalValue globalValue;
```

然后正常用,注意 properties 文件返回的都是默认 string 的。

```java
globalValue.getTOKEN_EXPIRETIME()
```

### 11.2 第 2 个

注意这里有个变化，tokenexpiretime 我去掉了中间的“.”，发现
“private String tokenexpiretime;”可以不加注解就取到值。
但是
“private String tokensecret;”就必须加注解才行

```properties
globle.token.secret=robin_privatekey
globle.tokenexpiretime=1000*60*2
```

```java
@Component
@Data
@ConfigurationProperties(prefix = "globle")
public class GlobalValue {
    @Value("${globle.token.secret}")
    private String tokensecret;
    private String tokenexpiretime;
}
```

另外因为 secret 我们要的是 byte[]，所以重写了下

```java
//@PropertySource("classpath:application.properties")
@Component
@Data
//@ConfigurationProperties(prefix = "globle")
public class GlobalValue {

    @Value("${globle.token.secret}")
    private String tokenSecret;

    @Value("${globle.token.expiretime}")
    private String tokenExpiretime;


    public byte[] getTokenSecret() {
        return tokenSecret.getBytes();
    }
}

```
