---
title: this/call/apply/bind
date: 2022-05-08 16:48:11
tags:
---

### 1. 对this对象的理解

this 是执行上文的一个属性,它指向了最后一次调用这个方法的对象,在实际开发中,this 的指向可以通过四种调用模式来判断。

* 第一种函数调用模式, 当一个函数不是一个对象的属性时.直接作为函数调用时, this 指向全局对象
* 第二种方法调用,当一个函数作为对象的属性调用时, this 指向这个对象
* 第三种是构造器调用模式, 如果一个函数用 new 调用时,函数执行前会新创建一个对象,this 指向这个新创建的对象
* 第四种是 apply, call , bind 调用模式,

这三个方法都可以显示的指定调用函数的 this 指向。其中 apply 方法接收两个参数：一个是 this 绑定的对象，一个是参数数组。call 方法接收的参数，第一个是 this 绑定的对象，后面的其余参数是传入函数执行的参数。也就是说，在使用 call() 方法时，传递给函数的参数必须逐个列举出来。bind 方法通过传入一个对象，返回一个 this 绑定了传入对象的新函数。这个函数的 this 指向除了使用 new 时会被改变，其他情况下都不会改变。

这四种方式，使用构造器调用模式的优先级最高，然后是 apply、call 和 bind 调用模式，然后是方法调用模式，然后是函数调用模式。

### 2. call() 和 apply() 的区别？
它们的作用一模一样,
* apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传递给被调用的函数。
* call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数。
### 3. 实现call、apply 及 bind 函数
(1) call 函数的实现步骤
* 判断调用对象是否是函数,但是可能出现使用 call 等方式调用的情况。
* 判断传入的上下文对象是否存在,如果不存在,则设置为 window
* 处理传入的参数, 截取第一个参数后的所有参数
* 将函数对象作为上下文对象的一个属性
* 使用上下文对象调用这个方法,并保存返回结果
* 删除刚才新增的属性。
* 返回结果。


```js

Function.prototype.myCall = function(context){
    if(typeof this  !==function ){
        console.error("type error");
    }
    let arg

}

```

(2)


问题: 

### 1. call apply  bind  都定义在哪里

call apply bind 都定义在 Function.prototype 上 
### 2.call,apply,bind 如何使用

call的使用 

```js
let ww ={
    dd:function(a,b,c){
    console.log(a)
        console.log(b)
        console.log(c)
        
    }
}
ww.dd.apply({},1,2,3)

```

apply 如何使用

apply()方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是Array的实例，也可以是arguments对象。例如：
```js
let ww ={
    dd:function(a,b,c){
    console.log(a)
        console.log(b)
        console.log(c)
        
    }
}
ww.dd.apply({},[1,2,3])

function sum(num1, num2){
    return num1 + num2;
}
 
function callSum1(num1, num2){
    return sum.apply(this, arguments);        // 传入arguments对象
}
 
function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]);    // 传入数组
}
 
alert(callSum1(10,10));   //20
alert(callSum2(10,10));   //20

```

bind 的使用

```js
let a ={b:function(){console.log(this)}}
// 正确使用
a.b.bind({c:1})()

// 错误使用
a.b.bind({c:1})
a.b()
//bind方法 fn.bind(thisArg,arg1,arg2...)
//不会调用
//返回的是原函数改变this之后产生的新函数
function fn2(name){
    console.log(this,name,22)
}
let f = fn2.bind(persion,"热巴")
f()
let btn = document.querySelector('button')
btn.onclick = function(){
    this.disabled = true
    setTimeout(function(){
    this.disabled = false
    }.bind(this),3000)


// 参数的合并
let ww ={

    dd:function(a,b,c){
    console.log(a)
        console.log(b)
        console.log(c)
        
    }
}
ww.dd.bind({a:1},1)(2,3)

```

### 3. call apply bind   三者的相同点和不同点 

相同点 ： 三者都有改变this的指向问题 

区别：

1：call和apply都可以调用函数 

2：call和apply传递的参数不一样 call传递参数arg1... ,apply传递参数[agr1...]

3：bind不会调用函数

###  4. call,apply,bind 的实际应用场景有哪些
[参考文档](https://blog.csdn.net/xy19950125/article/details/121124800)
[参考文档2](https://blog.csdn.net/wangzl1163/article/details/81121742)
* call用来继承
* apply经常以数组有关，求最大值，最小值等

```js
 function fn1(arg1,arg2){
      console.log(this,arg1,arg2)
    }
    fn1.apply(persion,["10","2"])
let arr = [10,9,11,12,8]
console.log(Math.max.apply(Math,arr))


```
* bind不调用函数，但是要改变this的指向 如定时器内部的this等

 1. bind可以传入多个参数,是合并到调用函数
```js
// 参数的合并
let ww ={

    dd:function(a,b,c){
    console.log(a)
        console.log(b)
        console.log(c)
        
    }
}
ww.dd.bind({a:1},1)(2,3)

```

 2. bind 还要注意是否是 new 调用的 ,如果是 new 调用的 this 的优先级最高