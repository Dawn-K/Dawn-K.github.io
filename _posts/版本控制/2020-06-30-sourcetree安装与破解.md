---
layout: post
title : 「版本控制」 sourcetree安装与破解
date: 2020-06-30
tags: [版本控制]
categories: [版本控制]
---

# soucetree 安装和破解

[参考资料1](https://www.jianshu.com/p/d02e9fba7cf3)
[参考资料2](https://www.jianshu.com/p/d839d4571b86)

## 介绍

Sourcetree windows是一款强大的Git/Mercurial桌面客户端, 是一个分布式版本控制系统. 支持Windows和Mac操作系统, 支持创建、克隆、提交、push、pull和合并等操作.

## 安装和破解

[官网下载](https://www.sourcetreeapp.com/)

1. 打开安装程序
2. 进入注册界面,先置之不理
3. 打开 `C:\Users\Administrator\AppData\Local\Atlassian\SourceTree`

4. 新建 accounts.json 文件,输入下列内容
5. 关闭安装程序,并再次打开,就可以使用了

``` json
[  
  {  
    "$id": "1",  
    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",  
    "Authenticate": true,  
    "HostInstance": {  
      "$id": "2",  
      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",  
      "Host": {  
        "$id": "3",  
        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",  
        "Id": "atlassian account"  
      },  
      "BaseUrl": "https://id.atlassian.com/"  
    },  
    "Credentials": {  
      "$id": "4",  
      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",  
      "Username": "",  
      "Email": null  
    },  
    "IsDefault": false  
  }  
]  

```

## SSH配置

`工具->选项->SSH客户端配置` , 选择OpenSSH, 并选中自己的私钥.

注意这里可能会出现一个bug, 具体原因没有搜到, 就是在项目界面的右上角的远端图标上多了一个感叹号.

从外网查到了解决方案, 但是只是临时去除, 在下次启动时还是会恢复.

[临时解决方案](https://community.atlassian.com/t5/Sourcetree-questions/Red-Exclamation-Mark-on-Remote-Icon/qaq-p/1174179)
仓库->仓库设置, 选中当前的仓库, 编辑, 然后将名字改为远程仓库用的账户名, 就会去掉.
