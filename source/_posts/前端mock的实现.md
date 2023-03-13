---
title: 前端mock的实现
date: 2023-03-10 16:02:23
tags:
---

Mock.js 是一个用于生成随机数据的 JavaScript 库，它可以模拟 Ajax 请求和响应，并且能够拦截 Ajax 请求。这是因为 Mock.js 通过重写 XMLHttpRequest 对象的 open 和 send 方法，实现了对 Ajax 请求的拦截。当我们使用 Mock.js 模拟 Ajax 请求时，它会拦截我们发送的 Ajax 请求，并根据我们定义的规则生成相应的随机数据，然后将其返回给前端。

具体来说，当我们在代码中使用 Mock.mock() 方法来定义接口时，Mock.js 会将这个定义注册到 XMLHttpRequest 对象的 open 方法中，这样当浏览器执行 xhr.open() 方法时，Mock.js 就会捕获到这个请求，并根据我们定义的规则生成随机数据，最终返回给前端。

需要注意的是，Mock.js 只能拦截通过 XMLHttpRequest 对象发起的 Ajax 请求，无法拦截使用 fetch、axios 等其他方式发起的请求。

总结: 了解 mock js 的使用