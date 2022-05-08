---
title: javascript中es6
date: 2022-03-10 11:20:23
tags:
---

# ES6

   ### 1. let const var 的区别
   块级作用域： 块级作用域由 {} 包括，let 和 const 具有块级作用域，var 不存在块级作用域，块级作用域解决了es5 中的两个问题

   * 内存变量可能覆盖外层变量
   * 用以计数的循环变量泄漏为全局变量
   变量提升：var 存在变量提升，let const 不存在变量提升，即变量只能在声明之后使用，否则会报错

   给全局添加属性: 浏览器的全局对象是 window, note 的全局对象是 global。 var声明的变量为全局变量,并且会将该变量添加为全局对象的属性，但是 let 和const 不会

   重复声明： var 声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的变量. const 和let 不允许重复声明变量

   暂时性死去： 在使用 let const 命令声明变量之前，该变量都是不可用的。这在语法上，称为暂时性死区， 使用var 声明的变量不存在暂时性死区

   初始值设置： 在变量声明时，var 和 let 可以不用设置初始值。 而 const 声明变量必须设置初始值
   指针指向： let 和const 都是 es6 新增的用于创建变量的关键字，let 创建的变量是可以更改指针指向（可以重新赋值），但是 const 声明的变量是不允许改变指针的指向

   ### 2. const 对象的属性可以修改吗

   const 保证的并不是变量的值不能改动， 而是变量指向的那个内存地址不能改动。 对于基本类型的数据（数值，字符串，布尔值， null，undefined）,其值就保存在变量指向的那个内存地址，因此等同于常量

   但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const 只能保证这个指针式固定不变的， 至于它指向的数据结构是不是可变的，就完全不能控制了。

    ### 3. 如果 new 一个箭头函数的话会怎么样

    箭头函数是 es6 中提出来的， 它没有 prototype， 也没有自己的 this 指向， 更不可以使用 arguments 参数， 所以不能 new 一个箭头函数
    new操作符的实现步骤如下：

    1. 创建一个对象
    2. 设置原型
    3. 将构造函数的 this 指向该对象
    4. 返回新的对象
   所以，上面的第二、三步，箭头函数都是没有办法执行的

### 4. 箭头函数和普通函数的区别

（1）箭头函数比普通函数更加简洁
* 如果没有参数，就直接写一个空括号即可
* 如果只有一个参数，可以省去参数的括号
* 如果有多个参数，用逗号分割
* 如果函数体的返回值只有一句，可以省略大括号
* 如果函数体不需要返回值，且只有一句话，可以给这个语句前面加一个void关键字。最常见的就是调用一个函数

```js
let fn = () => void doesNotReturn();
```

(2) 箭头函数没有自己的this

箭头函数不会创建自己的this， 所以它没有自己的this，它只会在自己作用域的上一层继承this。所以箭头函数中this的指向在它在定义时已经确定了，之后不会改变。

(3) 箭头函数继承来的this指向永远不会改变

```js
var id = 'GLOBAL';
var obj = {
  id: 'OBJ',
  a: function(){
    console.log(this.id);
  },
  b: () => {
    console.log(this.id);
  }
};
obj.a();    // 'OBJ'
obj.b();    // 'GLOBAL'
new obj.a()  // undefined
new obj.b()  // Uncaught TypeError: obj.b is not a constructor
```
对象obj的方法b是使用箭头函数定义的，这个函数中的this就永远指向它定义时所处的全局执行环境中的this，即便这个函数是作为对象obj的方法调用，this依旧指向Window对象。需要注意，定义对象的大括号{}是无法形成一个单独的执行环境的，它依旧是处于全局执行环境中。

（4）call()、apply()、bind()等方法不能改变箭头函数中this的指向

```js
var id = 'Global';
let fun1 = () => {
    console.log(this.id)
};
fun1();                     // 'Global'
fun1.call({id: 'Obj'});     // 'Global'
fun1.apply({id: 'Obj'});    // 'Global'
fun1.bind({id: 'Obj'})();   // 'Global'
```
（5）箭头函数不能作为构造函数使用
构造函数在new的步骤在上面已经说过了，实际上第二步就是将函数中的this指向该对象。 但是由于箭头函数时没有自己的this的，且this指向外层的执行环境，且不能改变指向，所以不能当做构造函数使用。

（6）箭头函数没有自己的arguments
箭头函数没有自己的arguments对象。在箭头函数中访问arguments实际上获得的是它外层函数的arguments值。
（7）箭头函数没有prototype
（8）箭头函数不能用作Generator函数，不能使用yeild关键字

### 5. 箭头函数的this指向哪⾥？
箭头函数不同于传统JavaScript中的函数，箭头函数并没有属于⾃⼰的this，它所谓的this是捕获其所在上下⽂的 this 值，作为⾃⼰的 this 值，并且由于没有属于⾃⼰的this，所以是不会被new调⽤的，这个所谓的this也不会被改变。
可以⽤Babel理解⼀下箭头函数:

```js
const obj = { 
  getArrow() { 
    return () => { 
      console.log(this === obj); 
    }; 
  } 
}

```
转化后：

```js
// ES5，由 Babel 转译
var obj = { 
   getArrow: function getArrow() { 
     var _this = this; 
     return function () { 
        console.log(_this === obj); 
     }; 
   } 
};

```


### 6. 扩展运算符的作用及使用场景



### 10. 扩展运算符被用在函数形参上时，它还可以把一个分离的参数序列整合成一个数组：

```js
function mutiple(...args) {
  let result = 1;
  for (var val of args) {
    result *= val;
  }
  return result;
}
mutiple(1, 2, 3, 4) // 24

```
这里，传入 mutiple 的是四个分离的参数，但是如果在 mutiple 函数里尝试输出 args 的值，会发现它是一个数组：

```js
function mutiple(...args) {
  console.log(args)
}
mutiple(1, 2, 3, 4) // [1, 2, 3, 4]
```
这就是 … rest运算符的又一层威力了，它可以把函数的多个入参收敛进一个数组里。这一点经常用于获取函数的多余参数，或者像上面这样处理函数参数个数不确定的情况。


11. ES6中模板语法与字符串处理

 # 相关问题 

1. js中栈和堆的概念和区别？[链接地址](https://juejin.cn/post/6854573215327617031)
2. this 和指针有关系吗？this也是指针
3. 