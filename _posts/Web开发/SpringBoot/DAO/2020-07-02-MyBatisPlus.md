---
layout: post
title : 「SpringBoot-DAO」 MyBatisPlus
date: 2020-07-02
tags: [Web开发, SpringBoot, DAO]
categories: [Web开发-SpringBoot-DAO]
---
# MyBatisPlus

[toc]
MyBatisPlus是为了单表查询时能够更方便的增删改查, 省去了.xml文件的书写, 提供了大量的内置方法.
[官网](https://mp.baomidou.com/)

## 配置

### MyBatisPlus

在idea中下载MyBatisPlus插件, 并重启idea

### 配置 pom.xml

``` xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
</dependency>
```

### 配置 application.yml

``` BASH
# mybatis-plus配置
mybatis-plus:
  configuration:
    cache-enabled: false
    map-underscore-to-camel-case: true
  configuration-properties:
    dbType: mysql
  global-config:
    banner: false
    #    下划线自动转换 ,比如 `role_name` 可以在java中写roleName
    #  这里有可能出现idea的警告,但是无伤大雅
    db-column-underline: true
    db-config:
      id-type: id_worker
      logic-delete-value: 1
      logic-not-delete-value: 0
    refresh-mapper: true
  mapper-locations: classpath*:/mapper/*.xml
  type-aliases-package: com.neuedu.entity
```

## 增删改查具体操作

### 数据库

首先新建一个数据库表his_role
里面两个字段

| 名称      | 类型         | 可空? | 主键? | 递增? |
|-----------|--------------|-------|------|------|
| id        | BigInt       | 不可空 | 主键 | 递增 |
| role_name | varchar(100) | 可空  | 否   | 否   |

实际上这里还遇到一个小bug, 就是如果id不设置成递增, 那么就会不让保存, 设置了递增, 但是默认值设为1也会在单元测试卡住, 所以这里就是设置递增, 但是不设置默认值.
另外, 修改数据库类型时, 最好先把数据库内容删掉, 否则会报错.

### 实体类(POJO)

``` java
// 实体类,用于存放数据库的字段
// src/main/java/com.neuedu/entity/Role.java
@TableName("his_role") // 表格名字
@Data                  // 自动生成get/set
public class Role {
    //    指定自增的Id,value主要是为了和数据库的对应,id最好改成bigInt(在数据库中),否则容易放不下
    @TableId(value = "id", type = IdType.AUTO)
    private Long id;
    // @TableField("role_name") // 可以强制指定该成员对应的列
    private String roleName;
}
```

### 包扫描(MapperScan)

``` java
@SpringBootApplication
@MapperScan(value="com.neuedu.mapper") // 包扫描,也是后续函数生效的关键
public class HisMain {
    public static void main(String[] args) {
        // jdbc: select * from users
        SpringApplication.run(HisMain.class, args);
    }
}
```

### 接口类(Mapper)

``` JAVA
// 接口类,注意继承关系,为下文单元测试
// src/main/java/com.neuedu/mapper/RoleMapper.java
@Mapper
public interface RoleMapper extends BaseMapper<Role> {
}
```

### 单元测试

在类名上 ctrl+shift+T 创建单元测试
在新建的单元测试类上面加入注解@SpringBootTest

``` JAVA
// 单元测试,用于检测上面的是否正确
// src/test/java/com.neuedu.mapper/RoleMapperTest.java
@SpringBootTest  // 单元测试
@Slf4j           // 日志
class RoleMapperTest {
    @Autowired
    private RoleMapper roleMapper;

    @Test
    public void selectAll() {
        log.info(roleMapper.toString());
    }

    @Test
    public void insert() {
        Role role = new Role();
        role.setRoleName("管理员");
        roleMapper.insert(role);
    }
}
```
