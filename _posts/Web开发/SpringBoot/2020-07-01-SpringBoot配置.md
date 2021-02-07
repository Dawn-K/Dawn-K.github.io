---
layout: post
title : 「Web开发-SpringBoot」 SpringBoot配置
date: 2020-07-01
tags: [Web开发, SpringBoot]
categories: [Web开发-SpringBoot]
---

# SpringBoot

[toc]

## 起源

SSH(struts spring    hibernate)

SSM(spring springMVC mybatis)

前两个框架太多配置, 非常不方便, 

SpringBoot 思路是约定优于配置, 帮助用户整合了很多配置, 其本质还是SSM

## 新建Spring项目

### 通过浏览器下载

打开[Spring网站](https://start.spring.io/)

分别选择 
Project: `Maven Project`

Language  : `Java`

SpringBoot: `2.3.1` (更高的版本都不是稳定版本)
Group: `com.neuedu`

ArtifactId: `neuedu-his`

Name: `neuedu-his`

Description: `Demo project for SpringBoot`

Package name: `com.neuedu.his` (包名不能有 `-` )
Packaging: `Jar`

Java: `8`

点 generate, 下载项目

### 通过idea下载

新建Spring项目, 然后网站选defult, 然后和官网几乎相同的配置

然后会进行依赖的选择, 

#### Developer Tools

`Spring Boot DevTools` (热部署, 用于不重启服务器的情况下进行编译部署. 但是不推荐开, 很麻烦)
 `Lombok`

 `Spring Configuration Processor`

#### Web

`Spring Web` (勾选, )
`Spring Reactive Web` (实时响应, 但本次不涉及)
`Spring Session` (这次不勾, 但建议学习. 在分布式中Session 共享(用redis实现))

#### SQL

数据库, 用于数据持久化
勾选以下内容:
 `mybatis Framework`

 `MySQL Driver`

#### NoSQL

 `Spring Data Redis (Access+Driver)`

 `Spring Data Elasticsearch (Access+Driver)`

#### Messaging

 `Spring fo RabbitMQ`

``` xml
<groupId></groupId>
<artifactId></artifactId>
<version></version>
<name>${project.artifactId}</name>
<description>睿道HIS系统</description>
```

需要在pom.xml文件中继承
注意到这个 denpendency中很多没写版本, 这个是由于springboot已经对这个进行了很多管理, 当然也可以自己进行配置.

``` xml
<!-- 继承父模块 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<!-- 排除依赖,一般是和其他模块出现依赖冲突时,就要排除部分依赖 -->
<exlusions>
```

## resource 配置

如果是手动建立的 `src/main/resources` 文件夹, 记得使用右键, mark as resource

两种配置方法(两者选其一)
其实本质上差不多, 
[转换工具](https://www.toyaml.com/index.html)
application是全局配置

### application.yml

注意使用两个空格进行缩进

``` yml
server:
  port: 8888
  address: localhost
```

### application.properties

``` properties
server.port= 8080
servet.address= localhost
```

## 返回给前端的接口

返回值是对象, 会自动转化为Json

``` java
@RestController
public class TestController {
    @GetMapping("test")
    public User testUrl() {
        User user = new User();
        user.setUsername("张三");
        user.setUseraddress("20");
        user.setUsersex("男");
        return user;
    }
}
```

``` json
// localhost:8888/test
{"username":"张三","useraddress":"20","usersex":"男"}
```

## Lombok

在类前使用 `@Data`

就可以自动生成 `get/set` 方法(准确的说是不用自己写, 然后可以自动调用, 及时生成, 哪怕是成员的名称发生了改变)

## 注入对象

要想注入对象, 必须将注入对象的类放在同级或子包中.(主要是指带入口的那个文件)

## 常见问题

###  Error:(3, 32) java: 程序包org.springframework.boot不存在

[参考资料](https://blog.csdn.net/lanben886/article/details/106622900/)

在setting中查找 runner->将IDE构建/运行 委托给Maven 将这个选项勾上, 重新编译即可

并且还要注意Maven设置的路径和配置文件是否正确

[最终参考资料](https://blog.csdn.net/qq_35524157/article/details/105867493?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

最终的解决方案很奇葩, 认为是idea2020的bug, 所以就在setting.xml中删除关于本地仓库的路径, 而在idea中强制规定路径, 然后重启idea, 就可以正常使用了, 使用maven接管这个构建只是一个技巧, 会影响效率.

### 可能会出现运行时Tomcat的报错

``` BASH
2020-07-01 17:51:07.196 ERROR 16900 --- [           main] o.a.catalina.core.AprLifecycleListener   : An incompatible version [1.2.5] of the Apache Tomcat Native library is installed, while Tomcat requires version [1.2.14]
```

[解决方案](https://blog.csdn.net/CronousGT/article/details/83618227)
访问:[官网网站](http://archive.apache.org/dist/tomcat/tomcat-connectors/native/)
下载下来, 里面有32位和64位的 tcnative-1.dll 文件, 根据自己的jdk和tomcat版本选择一个, 复制到jdk 的bin目录下即可.

这是tomcat的错误, 当然老师的建议是换个Undertow
[老师的简书](https://www.jianshu.com/p/5d291be7e58d)

``` xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!--undertow-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```
