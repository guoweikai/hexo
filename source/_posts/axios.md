---
title: axios
date: 2022-05-03 06:00:04
tags:
---

文档链接请点击[此处](http://www.axios-js.com/zh-cn/docs/)


### 1.什么是 axios?
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。


### 2.特性
* 从浏览器中创建 XMLHttpRequests
* 从 node.js 创建 http 请求
* 支持 Promise API
* 拦截请求和响应
* 转换请求数据和响应数据
* 取消请求
* 自动转换 JSON 数据
* 客户端支持防御 XSRF

### 并发

* 处理并发请求的助手函数
* axios.all(iterable)
* axios.spread(callback)

### 创建实例

可以使用自定义配置新建一个 axios 实例
axios.create([config])

```js
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

### 实例方法

以下是可用的实例方法。指定的配置将与实例的配置合并。

axios#request(config)
axios#get(url[, config])
axios#delete(url[, config])
axios#head(url[, config])
axios#options(url[, config])
axios#post(url[, data[, config]])
axios#put(url[, data[, config]])
axios#patch(url[, data[, config]])

### 请求配置

