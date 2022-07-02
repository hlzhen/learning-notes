# Swagger入门案例

## 1.新建Maven工程配置相关依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- SpringBoot父依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath/>
    </parent>
    <groupId>com.waner</groupId>
    <artifactId>swagger-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- swagger依赖 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>

        <!-- lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

</project>
```

## 2.创建相关实体
```java
package com.waner.entity;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

/**
 * 菜单实体
 * Created by Ale on 2022/7/1
 */
@ApiModel(value = "菜单实体", description = "菜单实体")
@Data
public class Menu {

    @ApiModelProperty(value = "菜单主键ID")
    private int id;

    @ApiModelProperty(value = "菜单名称")
    private String name;
}



package com.waner.entity;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

/**
 * 用户实体
 * Created by Ale on 2022/7/1
 */
@Data
@ApiModel(description = "用户实体类")
public class User {

    @ApiModelProperty(value = "主键ID")
    private int id;

    @ApiModelProperty(value = "用户名称")
    private String name;

    @ApiModelProperty(value = "用户年龄")
    private int age;

    @ApiModelProperty(value = "用户地址")
    private String address;
}
```

## 3.编写对应控制器
```java
package com.waner.controller.menu;

import com.waner.entity.Menu;
import com.waner.entity.User;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

/**
 * 菜单控制器
 * Created by Ale on 2022/7/1
 */
@RestController
@RequestMapping("/menu")
@Api(tags = "菜单控制器")
public class MenuController {


    @ApiOperation(value = "查询所有菜单", notes = "查询所有菜单信息")
    @RequestMapping(value = "/getmenus", method = RequestMethod.GET)
    public List<Menu> getAllMenus() {
        Menu menu = new Menu();
        menu.setId(10);
        menu.setName("首页");
        List<Menu> menus = new ArrayList<>();
        menus.add(menu);
        return menus;
    }

    @ApiImplicitParams({
            @ApiImplicitParam(name = "currentPage", value = "当前页", required = true, type = "Integer"),
            @ApiImplicitParam(name = "pageSize", value = "页大小", required = true, type = "Integer")
    })
    @ApiOperation(value = "分页查询菜单信息")
    @RequestMapping(value = "/page/{currentPage}/{pageSize}", method = RequestMethod.GET)
    public String findByPage(@PathVariable Integer currentPage, @PathVariable Integer pageSize) {
        return "ok";
    }


    @ApiOperation(value = "新增菜单", notes = "新增菜单信息")
    @RequestMapping(value = "/save", method = RequestMethod.POST)
    public String save(@RequestBody Menu menu) {
        return "ok";
    }

    @ApiOperation(value = "修改菜单", notes = "修改菜单信息")
    @RequestMapping(value = "/update", method = RequestMethod.POST)
    public String update(@RequestBody Menu menu) {
        return "oK";
    }

    @RequestMapping(value = "/delete", method = RequestMethod.DELETE)
    @ApiOperation(value = "删除菜单", notes = "删除菜单信息")
    public String delete(int id) {
        return "OK";
    }
}
```


## 4.Swagger配置
```java
package com.waner.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

/**
 * Swagger配置类
 * @EnableSwagger2: 启用Swagger
 * Created by Ale on 2022/7/1
 */
@Configuration
@EnableSwagger2
public class SwaggerAutoConfiguration {


    @Bean
    public Docket createRestApi1() {
        // docket用于封装接口文档信息
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(getApiInfo())
                .groupName("用户接口组")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.waner.controller.user"))
                .build();
        return docket;
    }

    @Bean
    public Docket createRestApi2() {
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(getApiInfo())
                .groupName("菜单接口组")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.waner.controller.menu"))
                .build();
        return docket;
    }


    private ApiInfo getApiInfo() {
        return new ApiInfoBuilder()
                .title("pd权限管理系统接口文档")
                .contact(new Contact("waner", "http://localhost:9000/swagger", "qingchenorg@163.com"))
                .version("1.0")
                .description("pd权限管理系统Api接口文档")
                .build();
    }

}
```

## 5.测试
```markdown
1. 启动项目
2. 浏览器输入
    - http://localhost:端口号/swagger-ui.html
```