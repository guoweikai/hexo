---
title: javascript基础
date: 2022-03-07 10:58:24
tags:
---
 [链接地址](https://juejin.cn/post/6940945178899251230)
# 数据类型

### JavaScript有哪些数据类型，它们的区别？
``` bash
    javascript 共有八种数据类型， 分别是 Undefined ,Null,Boolean,Number,String, Object, Symbol,BigInt
    其中 Symbol 和 BigInt 是 ES6 中新增的数据类型：
        Symbol 代表创建后独一无二且不可变的数据类型， 它主要是为了解决可能出现的全局变量冲突的问题。（感觉没意思）
        ## 如何使用
        创建
        const  s = Symbl();
        console.log(typeof s);//"symbol"
        BigInt 是一种数字类型的数据， 它可以表示任意精度格式的整数， 使用 BigInt 可以安全地存储和操作大整数， 即使这个数已经超过了 Number 能够表示的安全整数范围
```
这些数据中可以分为原始数据类型和引用数据类型:
* 栈： 原始数据类型（Undefined, Null, Boolean,  Number, String）
* 堆： 引用数据类型(对象，数组，函数)
两种类型的区别在于存储位置的不同：
> 原始数据类型是直接存放在栈中的简单数据段，占据空间小，大小固定，属于被频繁使用数据，所以放于栈中存放
> 引用数据类型存储在堆中段的对象， 占据空间大，大小不固定。 如果存储在栈中，将会影响程序运行的性能； 引用数据类型在栈中储存了指针， 该指针指向堆中该实体的起始地址。 当
> 解释器寻找引用值时， 会首先检索其在栈中的地址，取得地址后从堆中获得实体

堆和栈的概念存在于数据结构和操作系统内存中，在数据结构中
* 在数据结构中，栈中数据的存取方式为先进后出
* 堆时一个优先队列，是按优先级来进行排序的， 优先级可以按照大小来规定的。

### 数据类型检测的方式有哪些

1.  typeof 

```js
console.log(typeof 2) //number
console.log(typeof true) //boolean
console.log(typeof 'str') // string
console.log(typeof [])  //object
console.log(typeof function (){}) // function 
console.log(typeof {}) // object
console.log(typeof undefined) //undefined
console.log(typeof null) // object
```
其中数组，对象，null都会判断为object， 其他判断都正确
为什么null，对象，数组，会是object 呢，因为机器码存储的类型的时候，后三位都是 000 ，所以都是 object
// 为什么呢  
相关问题： 
如何判断变量是不是数组类型？

2. instanceof(实例)

instanceof 可以正确判断对象的类型，其内部运行机制是判断在其原型链中能否找到该类型的原型

```js
    console.log(2 instanceof Number) // false
    console.log( true instanceof Boolean) // false 
    console.log("str" instanceof String)// false 
    console.log([] instanceof Array) //true
    console.log(function(){} instanceof Function) //true
    console.log({} instanceof Object) // true
```
// 基本数据类型的原型是什么呢？
可以看出，instanceof 只能正确判断引用数据类型，而不能判断基本数据类型。 instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性

3. constructor (构造函数)

```js
    console.log((2).constructor === Number) // true
    console.log((true).constructor === Boolean) // true 
    console.log(("str").constructor === String) // true
    console.log([].constructor === Array) //true
    console.log((function(){}).constructor === Function)// true 
    console.log(({}).constructor === Object) //true
```

constructor 有两个作用，一个是判断数据的类型，


# ES6
1. let const var 的区别
   块级作用域： 块级作用域由 {} 包括，let 和 const 具有块级作用域，var 不存在块级作用域，块级作用域解决了es5 中的两个问题

   内存变量可能覆盖外层变量
   用以计数的循环变量泄漏为全局变量

   变量提升：var 存在变量提升，let const 不存在变量提升，即变量只能在声明之后使用，否则会报错
   
   给全局添加属性: 浏览器的全局对象是 window, note 的全局对象是 global。 var声明的变量为全局变量 




