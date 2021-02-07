---
layout: post
title : 「Web开发-SpringBoot」 SpringBoot业务总览
date: 2020-07-03
tags: [Web开发, SpringBoot]
categories: [Web开发-SpringBoot]
---
# SpringBoot业务总览

## 结构

 `View`
 `Service`
 `DAO`

## 项目目录分析

随着项目越来越大, 也越来越复杂. 在此对目录结构进行简单整理.

``` bash
# 打印当前目录(及其子目录)下的文件结构重定向到文件中
tree /f > tree.txt
```

``` bash
src
│
├─main
│  ├─java
│  │  └─com
│  │      └─neuedu
│  │          │  HisMain.java
│  │          │  
│  │          │ # 配置类
│  │          ├─config
│  │          │      MybatisPlusConfig.java
│  │          │      WebConfig.java
│  │          │      
│  │          │ # 约束(封装到类中的全局变量)
│  │          ├─constrant
│  │          │      GlobalConstrant.java
│  │          │      ResultConstrant.java
│  │          │      
│  │          │ # 控制器(视图层)
│  │          ├─controller
│  │          │      AuthController.java
│  │          │      TestController.java
│  │          │      UserController.java
│  │          │      
│  │          ├─core
│  │          │      Result.java
│  │          │      
│  │          │ # 实体类
│  │          ├─entity
│  │          │  │  Role.java
│  │          │  │  User.java
│  │          │  │  
│  │          │  ├─request
│  │          │  │      LoginParams.java
│  │          │  │      
│  │          │  └─vo
│  │          │          TokenVO.java
│  │          │          UserVO.java
│  │          │          
│  │          │ # 拦截器
│  │          ├─interceptor
│  │          │      LoginInterceptor.java
│  │          │      
│  │          │ # 持久层
│  │          ├─mapper
│  │          │      RoleMapper.java
│  │          │      UserMapper.java
│  │          │      
│  │          │ # 服务层
│  │          ├─service
│  │          │  │  UserService.java
│  │          │  │  
│  │          │  └─impl
│  │          │          UserServiceImpl.java
│  │          │ # 通用工具
│  │          └─utils
│  │                  JwtUtils.java
│  │                  
│  │ # 资源
│  └─resources
│      │  application.yml
│      │  
│      └─mapper
│              UserMapper.xml
│              
└─test
    └─java
        └─com
            └─neuedu
                ├─mapper
                │      RoleMapperTest.java
                │      UserMapperTest.java
                │      
                ├─service
                │  └─impl
                │          UserServiceImplTest.java
                │          
                └─utils
                        JwtUtilsTest.java
```
