# Swagger简介

## 1.Swagger产生背景
```markdown
# 相信无论是前端还是后端开发，都或多或少地被接口文档折磨过。
1. 前端经常抱怨后端给的接口文档与实际情况不一致。
2. 后端又觉得编写及维护接口文档会耗费不少精力，经常来不及更新。
 - 其实无论是前端调用后端，还是后端调用后端，都期望有一个好的接口文档。
 - 但是这个接口文档对于程序员来说，就跟注释一样，经常会抱怨别人写的代码没有写注释，
 - 然而自己写起代码起来，最讨厌的，也是写注释。
3. 仅仅只通过强制来规范大家是不够的，随着时间推移，版本迭代，接口文档往往很容易就跟不上代码了。
```

## 2.Swagger是什么
```markdown
1. Swagger是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。
2. 可用于：
    - 1.接口的文档在线自动生成、
    - 2.功能测试。
```


## 3.Maven坐标
```xml
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
```

## 4.常用注解
| 注解               | 说明                                                     |
| :----------------- | :------------------------------------------------------- |
| @Api               | 用在请求的类上，例如Controller，表示对类的说明           |
| @ApiModel          | 用在类上，通常是实体类，表示一个返回响应数据的信息       |
| @ApiModelProperty  | 用在属性上，描述响应类的属性                             |
| @ApiOperation      | 用在请求的方法上，说明方法的用途、作用                   |
| @ApiImplicitParams | 用在请求的方法上，表示一组参数说明                       |
| @ApiImplicitParam  | 用在@ApilmplicitParams注解中，指定一个请求参数的各个方面 |