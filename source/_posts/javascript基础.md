---
title: javascript基础
date: 2022-03-07 10:58:24
tags:
---
 [链接地址](https://juejin.cn/post/6940945178899251230)
# 数据类型

### 1 JavaScript有哪些数据类型，它们的区别？
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

### 2.数据类型检测的方式有哪些

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

### 3判断数组的方式由哪些

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

### 4. null 和 undefined 区别
首先 null 和 undefined 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 null 和 undefined 

undefined 代表的含义是未定义， null 代表的含义是空对象， 一般变量声明了但还没有定义的时候会返回undefined， null 主要用于复制给 一些可能会返回对象的变量，作为初始化

undefined 在 js 中不是一个保留字，这意味着可以使用 undefined 来作为一个变量名， 但是这样的做法是非常危险的， 它会影响对 undefined 值的判断， 我们可以通过一些方法获取安全的 undefinded值， 比如 void 0

当对这两种类型使用typeof 进行判断时。 Null类型会返回"object",这是一个历史遗留的问题， 当使用双等号对两种类型的值进行比较时会返回 true， 使用三个等号时会返回false

### 5.typeof null 的结果是什么， 为什么？

typeof null 的结果是 Object

在 JS 第一版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签（1-3 bits） 以及当前要存储值的真实数据. 类型标签存储在每个单元的低位中，共有五种数据类型

    ```JS
        000:object  当前存储的数据指向一个对象
        1:init - 当前存储的数据是一个 31 位的有符号整数
        010: double - 当前存储的数据指向一个双进度的浮点数
        100: string - 当前存储的数据指向一个字符串
        110: boolean - 当前存储的数据是布尔值
    ```
    如果最低位是1， 则类型标签标志位的长度只有一位； 如果最地位是 0， 则类型标签标志位的长度占三位， 为存储其他四种数据类型提供了额外两个 bit 的长度
    有两种特殊数据类型：
* undefined 的值是 （-2）30（一个超出整数范围的数字）；
* null 的值是机器码 null 指针（null指针的值全是 0）；
  
那也就是说 null 的类型标签也是 000， 和Object 的类型标签一样，所以会被判定为 object

### 6.instanceof 操作符的实现原理
    instanceof 运算符用于判断构造函数的prototype属性是否出现在对象的原型链中的

    ```JS
     function myInstanceof（left，right）{
        //  获取对象的原型
         let proto = Object.getPrototypeOf(left);
        // 获取构造函数的 prototype 对象
         let prototype = right.prototype;
         while(true){
             if(!proto) return false;
             if(proto === prototype) return true;
             proto = Object.getPrototypeOf(proto)
         }  
     }
    ```
### 7.为什么 0.1+0.2 ！== 0.3 如何让其相等


### 8. 如何安全的获取 undefined 的值

### 9 typeof NaN 的结果是什么？

> NaN 指“不是一个数字”， NaN是一个“警戒值”，用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果
> typeof NaN //“number”
NaN 是一个特殊值，它和自身不相等， 是唯一一个非自饭  NaN ！== NaN 为true
### 10 isNaN 和 Number.isNaN 函数的区别

### 11 == 操作运算符的强制类型转换规则

1. 首先会判断两者类型是否相同，相同的话就比较两者的大小
2. 类型不相投的话， 就会进行类型转换


### 12 其他值到字符串的转换规则

* Null 和 Undefined 类型, null 转换为“null”
* Null 类型的值转换为 0


### 18 JavasScript 中如何进行隐式类型转换
### 19 + 操作符什么时候用于字符串的拼接？

### 20 为什么会有 bigInt 的提案

### 21 Object.assign() 和扩展运算符是深拷贝还是浅拷贝，两者区别

扩展运算符：
```js
    let outObj = {inObj:{a:1,b:2}}
    let newObj = Object.assign(...outObj)
    newObj.inObj.a = 2
    console.log(outObj)
```
Object.assign():

```js
let outObj = {
  inObj: {a: 1, b: 2}
}
let newObj = Object.assign({}, outObj)
newObj.inObj.a = 2
console.log(outObj) 
```
可以看出，两者都是浅拷贝
>Object.assign()方法接收的第一个参数作为目标对象，后面的所有参数作为源对象。然后把所有的源对象合并到目标对象中。它会修改了一个对象，因此会触发 ES6 setter。
>扩展操作符（…）使用它时，数组或对象中的每一个值都会被拷贝到一个新的数组或对象中。它不复制继承的属性或类的属性，但是它会复制ES6的 symbols 属性。



** 类型的考察基本就这些**

<hr>

# ES6

   ### let const var 的区别
   块级作用域： 块级作用域由 {} 包括，let 和 const 具有块级作用域，var 不存在块级作用域，块级作用域解决了es5 中的两个问题

   * 内存变量可能覆盖外层变量
   * 用以计数的循环变量泄漏为全局变量
   变量提升：var 存在变量提升，let const 不存在变量提升，即变量只能在声明之后使用，否则会报错

   给全局添加属性: 浏览器的全局对象是 window, note 的全局对象是 global。 var声明的变量为全局变量,并且会将该变量添加为全局对象的属性，但是 let 和const 不会

   重复声明： var 声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的变量. const 和let 不允许重复声明变量

   暂时性死去： 在使用 let const 命令声明变量之前，该变量都是不可用的。这在语法上，称为暂时性死区， 使用var 声明的变量不存在暂时性死区

   初始值设置： 在变量声明时，var 和 let 可以不用设置初始值。 而 const 声明变量必须设置初始值
   指针指向： let 和const 都是 es6 新增的用于创建变量的关键字，let 创建的变量是可以更改指针指向（可以重新赋值），但是 const 声明的变量是不允许改变指针的指向

   ### const 对象的属性可以修改吗

   const 保证的并不是变量的值不能改动， 而是变量指向的那个内存地址不能改动。 对于基本类型的数据（数值，字符串，布尔值， null，undefined）,其值就保存在变量指向的那个内存地址，因此等同于常量

   但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const 只能保证这个指针式固定不变的， 至于它指向的数据结构是不是可变的，就完全不能控制了。

    ### 如果 new 一个箭头函数的话会怎么样

    箭头函数是 es6 中提出来的， 它没有 prototype， 也没有自己的 this 指向， 更不可以使用 arguments 参数， 所以不能 new 一个箭头函数



# JavaScript 基础
# 相关问题 

1. js中栈和堆的概念和区别？[链接地址](https://juejin.cn/post/6854573215327617031)
2. this 和指针有关系吗？this也是指针





