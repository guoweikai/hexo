---
title: javascript数据类型
date: 2022-03-10 11:21:26
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
3. 会先判断是否在对比 null 和 undefinded 是的话就会返回 true
4. 判断两者类型是否为 string 和 number ， 是的话就会将字符串转化为 number

```js
    1=="1"
    1==1
```
5. 判断其中一方是否为 boolean， 是的话就会把 boolean 转为 number， 再进行判断
6. 判断其中一方是否为 object 且另一方为 string number 或者 symbol  是的话就会把 object 转为原始类型再进行判断
   
其流程图如下
具体可以看一下流程图
### 12 其他值到字符串的转换规则

* Null 和 Undefined 类型, null 转换为“null”，undefined 转化为 “undefined”
* Boolean 类型， true 转化为 “true”， false 转化为 “false”
* Number类型的值直接转换， 但是只允许显式强制类型转换， 使用隐式强制类型转换会产生错误
* Symbol 类型的值转换， 但是只允许显式强制类型转换，使用隐式强制类型转换会产生报错
* 对普通对象来说，除非自行定义 toString()方法， 否则会调用toString()
  Object.prototype.toString() 来返回内部属性 [[Class]]的值， 如"[object Object]]". 如果对象有自己的 tostring 方法， 字符串化时就会调用该方法并使用其返回值

### 13 其他到数字值的转化规则
* Undefined 类型的值转换为 NaN
* Null 类型的值 转换为 0
* Boolean 类型的值。 true 转换为1， false转换为 0
* String 类型的值转换如同使用Number 函数进行转化， 如果包含非数字值则转化为 NaN， 空字符串为 0
* 对象（包括数组）会首先被。。。。

### 14 其他值到布尔类型的值的转换规则

以下这些是假值 undefined null NaN +0 -0 ""  false 
从逻辑上说， 假值列表以外的都应该是真值

### 15 || 和 && 操作符的返回值
|| 和 && 首先会对第一个操作数执行条件判断， 如果其不是布尔值就先强制转化为布尔类型， 然后再执行条件判断

### 16 Objet.is() 与比较操作符 “===“， ”==” 的区别
* 使用双等号（==） 进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较
* 使用三等号（===）进行相等判断时， 如果两边的类型不一致时， 不会做强制类型转换，直接返回 false
* 使用 Object.is 来进行相等判断时，它处理了一些特殊的情况， 比如 -0 和 +0  不能相等， 两个 NaN 是相等的

### 17 什么是 JavaScript 中的包装类型？
在 JavaScript 中， 基本类型是没有属性和方法的， 但是为了便于操作基本类型的值，在调用基本类型的属性或方法时JavaScript 会在后台隐式地将基本类型的值转换为对象如
```js
    const a = "abc"
    a.length;//3
    a.toUpperCase();//"ABC"
```
在访问 “abc”.length 时， JavaScript 将 “abc” 在后台转化为 String（“abc”），然后再访问其 length 属性

```js
    var a = "abc"
    Object(a) // string{"abc"}
```
也可以使用 valueOf 方法将包装类型倒转成基本类型
```js
    var a = "abc"
    var b  = Object(a)
    var c = b.valueOf()
```
看看如下代卖会打印什么：

    ```js
     var  a = new Boolean(false)
     if(!a){
         console.log("Oops")
     }
    ```
答案是什么都不会打印，因为虽然包裹的基本类型是false，但是false被包裹成包装类型后就成了对象，所以其非值为false，所以循环体中的内容不会运行。

### 18 JavasScript 中如何进行隐式类型转换

首先要介绍 ToPrimitive 方法， 这是 js 
### 19 + 操作符什么时候用于字符串的拼接？

### 20 为什么会有 bigInt 的提案
JavaScript中Number.MAX_SAFE_INTEGER表示最⼤安全数字，计算结果是9007199254740991，即在这个数范围内不会出现精度丢失（⼩数除外）。但是⼀旦超过这个范围，js就会出现计算不准确的情况，这在⼤数计算的时候不得不依靠⼀些第三⽅库进⾏解决，因此官⽅提出了BigInt来解决此问题。### 21 Object.assign() 和扩展运算符是深拷贝还是浅拷贝，两者区别

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





#  问题 

 Object.is(-1,1)

Object.prototype.valueOf()

valueof()方法返回指定对象的原始值

语法：
object.valueof()

[对象的方法详解](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)






