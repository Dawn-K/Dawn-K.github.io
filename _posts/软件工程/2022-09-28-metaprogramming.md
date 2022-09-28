---
layout: post
title : 「软件工程」 metaprogramming
date: 2022-09-28
tags: [软件工程, Missing-Semester]
categories: [软件工程]
---

# 概述

The Missing semester 的 metadataprograming 这节课不是讲某个特定的技术, 也并不是讲编程语言中的元编程, 而是讲软件工程中的一些工具.

## 构建工具

大型复杂的软件通常构建起来也很复杂. 有很多的步骤, 而且步骤之间还有依赖关系. 构建工具就能帮助我们自动化的进行构建过程, 并且在每次构建时都会分析出最少需要新构建的内容. 以 `make` 为例.

关于 `make` , 已经在 [makefile](./makefile.md)中学习过了, 不再赘述。

## 依赖管理

大型的, 维护周期长的软件和库往往具有多个版本, 并且被大量依赖. 因此我们对版本命名有要求:

 `<major version>.<minor number>.<patch version>`

* 对于API没有任何外界可感知的修改, 比如修复了bug, 则只增加 `patch version`
* 对于API有功能的添加, 并且后向兼容, 那么就增加`minor number`, 并且将`patch version`
* 不兼容的更改, 就增加`major version`, 并且将后两个清零

此外还有 lock file 这个概念, 是说一个文件, 列出了自己所需要的依赖的确切版本, 通常需要自己手动才能升级到更新的版本. 这种往往是出于安全和稳定的角度考虑.

## 持续集成

持续集成(Continuous Integration, CI) 可以理解为在代码发生变更时自动执行的脚本. 比如在有commit时, 有pull request时自动执行单元测试, 代码审查, 自动构建等等。或者是来自外界的, 比如项目有个依赖更新了, 这个更新是兼容的, 可以让github bots自动生成新的依赖配置文件, 然后自动发起pull request.

## 测试

* 单元测试(Unit test). 只测试特定功能.
* 集成测试(Integration test). 测试多个组件的配合.
* 回归测试(Regress test). 将之前出现的错误也写个测试, 防止之后的修改再触发之前的错误.

* 测试套件(Test suite). 所有测试的总称.
* 模拟(Mock). 以虚假的实现替代外部依赖.

## 参考资料

[The-missing-semester/metaprogramming](https://missing.csail.mit.edu/2020/metaprogramming/)
