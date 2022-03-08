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

constructor 有两个作用，一个是判断数据的类型，二是对象实例通过 constrcutor 对象访问它的构造函数。 需要注意， 如果创建一个对象来改变它的原型， constructor 就不能用来判断数据类型了

4. Object.prototype.toString.call()

```js
    var a = Object.prototype.toString;
    console.log(a.call(2));
    console.log(a.call(true));
    console.log(a.call([]));
    console.log(a.call(function(){}));
    console.log(a.call(undefined));
    console.log(a.call(null));
```
   
同样是检测对象 obj 调用 toString 方法， obj.toString 的结果和  Objec.prototype.toString.call(obj)的结果不一样，这是为什么？
这是因为 toString 是 Object 的原型方法， 而 Array 和 function 等类型作为 Object 的实例， 都重写了 toString 方法， 不同的对象类型
调用 toString方法时，根据原型链的知识，调用的是对应的重写之后的 toString 方法，(function 类型返回内容为函数体的字符串，Array 类型返回元素组成的字符串),
而不会去调用 Object 上原型toString 方法（返回对象的具体类型）， 所有采用 obj.toString() 不能得到其对象类型，只能将 obj 转换为字符串类型，因此，在想要得到对象的具体类型时，应该调用 Object 原型上的 toString 方法

### 判断数组的方式由哪些

通过 Object.prototype.toString.call()⬅️判断

通过原型链做判断

obj.__proto__ === Array.prototype 

通过 es6 的 Array.isArray() 做判断

Array.isArray(obj)

通过 instanceof 做判断

obj instanceof Array

通过 Array.prototype.isPrototypeof

```js
    Array.prototype.isPrototypeOf(obj) ? 这个稍后梳理一下
```

### null 和 undefined 区别
首先 null 和 undefined 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 null 和 undefined 

undefined 代表的含义是未定义， null 代表的含义是空对象， 一般变量声明了但还没有定义的时候会返回undefined， null 主要用于复制给 一些可能会返回对象的变量，作为初始化

undefined 在 js 中不是一个保留字，这意味着可以使用 undefined 来作为一个变量名， 但是这样的做法是非常危险的， 它会影响对 undefined 值的判断， 我们可以通过一些方法获取安全的 undefinded值， 比如 void 0

当对这两种类型使用typeof 进行判断时。 Null类型会返回"object",这是一个历史遗留的问题， 当使用双等号对两种类型的值进行比较时会返回 true， 使用三个等号时会返回false

### typeof null 的结果是什么， 为什么？

typeof null 的结果是 Object

在 JS 第一版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签（1-3 bits） 以及当前要存储值的真实数据. 类型标签存储在每个单元的低位中，共有五种数据类型

    ```JS
        000:object  当前存储的数据指向一个对象
        1:init - 当前存储的数据是一个 31 位的有符号整数
        010: double - 当前存储的数据指向一个双进度的浮点数
        100: string - 当前存储的数据指向一个字符串
        110: boolean - 当前存储的数据是布尔值
    ```

### instanceof 操作符的实现原理
    instanceof 运算符用于判断构造函数的prototype属性是否出现在对象的原型链中的
    ```js
    ```

### isNaN 和 Number.isNaN 函数的区别



###  什么是 js 中的包装类型

    在 js 中， 基本类型是没有属性和方法的， 但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 js 会在后台隐式地将
函数 isNaN
# ES6
1. let const var 的区别
   块级作用域： 块级作用域由 {} 包括，let 和 const 具有块级作用域，var 不存在块级作用域，块级作用域解决了es5 中的两个问题

   内存变量可能覆盖外层变量
   用以计数的循环变量泄漏为全局变量

   变量提升：var 存在变量提升，let const 不存在变量提升，即变量只能在声明之后使用，否则会报错
   
   给全局添加属性: 浏览器的全局对象是 window, note 的全局对象是 global。 var声明的变量为全局变量 




