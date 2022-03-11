---
title: javascript基础
date: 2022-03-10 11:20:34
tags:
---

### 1. new 操作符的实现原理
* 首先创建一个空对象
* 设置原型，将对象的原型设置为函数的 prototype对象
* 让函数的this 指向这个对象，执行构造函数的代码
* 判断函数的返回值类型，如果是值类型，返回创建的对象，如果是引用类型，就返回这个引用类型的对象(这个地方有疑惑)

具体实现

```js
    function ojbectFactory(){
        let newObject = null;
        let constructor = Array.prototype.shift.call(arguments);
        let result = null;
        if(typeof constructor ! == "function"){
            console.error("type error");
            return
        }
        newObject = Object.create(constructor.prototype);
        result = constructor.apply(newObject,arguments);
          // 判断返回对象
  let flag = result && (typeof result === "object" || typeof result === "function");
  // 判断返回结果
  return flag ? result : newObject;
    }
    // 使用方法
objectFactory(构造函数, 初始化参数);

```

### 2 map 和 Object 的区别

|          | Map                                                                        | Object                                                                      |
| -------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| 意外的健 | Map默认情况不包含任何健，只包含显示插入的健                                | Object 有一个原型, 原型链上的键名有可能和自己在对象上的设置的键名产生冲突。 |
| 键的类型 | Map的键可以是任意值，包括函数、对象或任意基本类型。                        | Object 的键必须是 String 或是Symbol。                                       |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候， Map 对象以插入的顺序返回键值。 | Object 的键是无序的                                                         |
| Size     | Map 的键值对个数可以轻易地通过size 属性获取                                | Object 的键值对个数只能手动计算                                             |
| 迭代     | Map 的键值对个数可以轻易地通过size 属性获取                                | Object 的键值对个数只能手动计算                                             |

### 3 map和weakMap的区别

### 4 JavaScript有哪些内置对象

全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在 全局作用域里的对象。全局作用域中的其他对象可以由用户的脚本创建或由宿主程序提供。

标准内置对象的分类：

（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。例如 Infinity、NaN、undefined、null 字面量
（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。例如 eval()、parseFloat()、parseInt() 等
（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等
（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date
（5）字符串，用来表示和操作字符串的对象。例如 String、RegExp
（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array
（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。
例如 Map、Set、WeakMap、WeakSet
（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。
例如 SIMD 等
（9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等
（10）控制抽象对象
例如 Promise、Generator 等
（11）反射。例如 Reflect、Proxy
（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等
（13）WebAssembly
（14）其他。例如 arguments

总结：
js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般经常用到的如全局变量值 NaN、undefined，全局函数如 parseInt()、parseFloat() 用来实例化对象的构造函数如 Date、Object 等，还有提供数学计算的单体内置对象如 Math 对象


### 5. 常用的正则表达式有哪些?


### 6. 对JSON的理解

JSON 是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。
在项目开发中，使用 JSON 作为前后端数据交换的方式。在前端通过将一个符合 JSON 格式的数据结构序列化为
JSON 字符串，然后将它传递到后端，后端通过 JSON 格式的字符串解析后生成对应的数据结构，以此来实现前后端数据的一个传递。
因为 JSON 的语法是基于 js 的，因此很容易将 JSON 和 js 中的对象弄混，但是应该注意的是 JSON 和 js 中的对象不是一回事，JSON 中对象格式更加严格，比如说在 JSON 中属性值不能为函数，不能出现 NaN 这样的属性值等，因此大多数的 js 对象是不符合 JSON 对象的格式的。
在 js 中提供了两个函数来实现 js 数据结构和 JSON 格式的转换处理，

JSON.stringify 函数，通过传入一个符合 JSON 格式的数据结构，将其转换为一个 JSON 字符串。如果传入的数据结构不符合 JSON 格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为 JSON 格式的字符串。
JSON.parse() 函数，这个函数用来将 JSON 格式的字符串转换为一个 js 数据结构，如果传入的字符串不是标准的 JSON 格式的字符串的话，将会抛出错误。当从后端接收到 JSON 格式的字符串时，可以通过这个方法来将其解析为一个 js 数据结构，以此来进行数据的访问


### 7 js 脚本延迟加载的方法有哪些

### 8 js 类数组对象的定义

### 9 数组有哪些原生方法

### s
### 疑惑点

1.  new 最后一步

2. 迭代是什么意思，迭代器是什么？ 

3.  数据的传输是json 格式的字符串