---
layout: post
title : 「Web开发-Vue」 LocalStorage
date: 2020-07-08
tags: [Web开发, Vue]
categories: [Web开发-Vue]
---

# LocalStorage

[toc]

[老师博客](https://www.jianshu.com/p/99d6a0cead71)
在HTML5中, 新加入了一个localStorage特性, 这个特性主要是用来作为本地存储来使用的, 解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k), 类似一种键值对. 我们主要用它来存储令牌.

可以点击浏览器工具的 Application选项中的 `Local Storage` 标签中对应的网址, 来看到目前 LocalStorage 中存储的内容

## 原生用法

``` js
// 原生的使用方法(其实window可以省略),但是我们还是要封装一下
// 存放
window.localStorage.setItem("token", "123456");
// 取用
let token = window.localStorage.getItem("token");
console.log(token);
```

## 封装

封装后可被反复利用, 用于令牌的校验.

``` js
// src\ utils\ localStorage.js
const token = "token";
// 设置数据
export function setItem(key) {
  window.localStorage.setItem(key);
}
// 获取数据
export function getItem(key) {
  return window.localStorage.getItem(key);
}
// 删除数据
export function remove(key) {
  return window.localStorage.removeItem(key);
}
// 设置token
export function setToken(value) {
  window.localStorage.setItem(token, value);
}
// 获取token
export function getToken() {
  return window.localStorage.getItem(token);
}
// 移除token
export function removeToken() {
  return window.localStorage.removeItem(token);
}
```
