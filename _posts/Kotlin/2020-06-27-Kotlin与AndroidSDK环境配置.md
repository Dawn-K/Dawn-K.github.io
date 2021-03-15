---
layout: post
title : 「Kotlin」 Kotlin与AndroidSDK环境配置
date: 2020-06-27
tags: [Kotlin]
categories: [Kotlin]
---

# Kotlin基础
[toc]

## Kotlin 介绍

Kotlin 是一个用于现代多平台应用的静态编程语言, 由 JetBrains 开发.
Kotlin可以编译成Java字节码, 也可以编译成JavaScript, 方便在没有JVM的设备上运行.
Kotlin已正式成为Android官方支持开发语言.

## 开发环境

### 总览

* IntelliJ IDEA(社区版足够)
* JDK 1.6+
* Android SDK(如果需要开发安卓的话)

### Android SDK

如果本身没有 android sdk, 事情就简单了.
打开Idea主界面, Configure-> Setting
里面选择 Android SDK , 点edit, 选择路径后 idea可以给你自动下载一个. 然后采用下文23的步骤, 就可以轻松配置.

理论上的方式:

1. 先下载一个sdk
2. 打开Idea主界面, Configure-> Structure for New Project
3. 设置jdk, 然后打开左侧的SDKs, 点加号, 添加 Android SDK , 选中.

这里很可能出现一个报错

Cannot find any Android targets in this SDK

根据多方查询
[stackoverflow的回答](https://stackoverflow.com/questions/38212868/cannot-find-any-android-targets-in-this-sdk-in-intellij-2016/40984451?r=SearchResults#40984451)

这个指出, 出现这个问题是因为

> This error message means that IntelliJ didn't find any virtual device to run your project in.
> You need to create a new Android Virtual Device (AVD) using the AVD manager tool.
> Next time you'll try to create a new project you select the same SDK path as before and IntelliJ will automatically find your newly created AVD.

根据我的猜想, 是点击弹出框中的 package mannager 然后会回到和上面的 Setting 相同的界面, 然后在edit中设置路径, 然后在选择你安装的sdk的路径, idea会自动给你下几个其他的工具, platform之类的, 然后就等待安装完成即可.

还有个猜想没有验证, 就是下载后的sdk里面有两个exe, 一个是 AVD manager.exe 一个是SDK manager.exe 后者在Idea里有, 前者我认为就是上文答案中所说的控制AVD的东西. 但是我尝试打开了, 并不懂如何操作, 或许还要等更深入的理解之后才可以.
