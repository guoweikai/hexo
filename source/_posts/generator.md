---
title: generator
date: 2022-08-03 15:17:40
tags:
---

## 参考文件
[深入理解Generator](https://segmentfault.com/a/1190000013264053)
(阮一峰es6)[https://es6.ruanyifeng.com/#docs/generator]

## 为什么要用 generator
在前端开发过程中我们经常需要先请求后端的数据, 再用拿来的数据进行使用网页页面渲染等操作,然而请求数据是一个异步操作, 而我们的页面渲染又是同步操作, 这里 es6 中 的 generator 就能发挥它的作用,使用它可以像写同步代码一样写异步代码.下面是一个例子, 先忽略下面的写法,后面会详细说明,如果你已经理解 generator 基础可以直接跳过这部分和语法部分,直接深入理解的部分

## 基本概念

generator 函数是


generator语法


```js
function *foo() {
  var a = yield 1 + 1;
  var b = yield 2 + a;
  console.log(b);
}

var it = foo();
it.next();
it.next(2);
it.next(4);
```


## 深入理解

前两部分我们学习了为什么要用 generator 以及 generator 的语法, 这些都是基础, 下面我们来看点不一样的东西, 老规矩先带这问题才能更有目的性的看,这里先提出几个问题
* 怎么在异步代码中使用, 上面的例子都是同步的
* 如果出现错误要怎么进行错误处理
* 一个个调用 next 太麻烦了, 能不能循环执行或者自动执行

## 迭代器
进行下面所有的部分之前我们先说一说迭代器, 看到现在,我们都知道 generator 函数执行完返回的是一个迭代器. 在 es6 中同样提供了一种新的迭代方式 for...of, for...of 可以帮忙我们直接迭代出每个的值, 在数组中它像这样

```js
    for(var  i of ['a','b','c']){
        console.log(i)
    }

    // 输出结果
    //a
    //b
    //c
```

