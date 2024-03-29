---
title: promise
date: 2022-05-08 21:58:52
tags:
---

[参考文档1](https://zhuanlan.zhihu.com/p/58428287);
[参考文档2](https://www.cnblogs.com/songyao666/p/15783903.html);

简单实现一个Promise

我们知道了一个promise内容至少包含以上那些内容，所以一个简单的promise内部至少是这样的

```js
class myPromise{
    static PENDING ='pending',
    static FULDILLED ='fulfilled',
    static REJECTED = 'rejected',
    constructor(init){
        this.state = myPromise.PENDING // promise状态

    }
}

```



### Promise

1. Promise 的含义

callback 的实现原来

```js
function aa(callback){
  setTimeout(()=>{
    callback()
  },1000)
}
```

Promise 是异步编程的一种解决方案,比传统的解决方案-- 回调回调函数和事件-- 更合理和更强大, 它由社区最早提出和实现, es6 将其写进了语言标准,统一了用法, 原生提供了 Promise 对象

所谓 Promise, 简单地说就是一个容器,里边保存着某个未来才会结束的事件(通常是一个异步操作)的结果, 从语法上说, Promise 是一个对象, 从它可以获取异步异步操作的信息.Promise 提供统一的 API, 各种异步操作都可以用同样的方法进行处理.

Promise 对象有以下两个特点
(1) 对象的状态不受外界影响, Promise 对象代表了一个异步操作, 有三种状态: pending(进行中), fufilled(已成功) 和 rejected(已失败). 只有异步操作的结果,可以决定当前是哪一种状态,任何其他 操作都无法改变这个状态. 这也是 Promise 这个名字的由来, 它的英语意思就是承诺, 表示其他手段无法改变
(2) 一旦状态改变, 就不会再变, 任何时候都可以得到这个结果. Promise 对象的状态改变, 只有两种可能: 从 pending 变为 fulfilled 和从 pending 变为 rejected. 只要这两种情况发生, 状态就凝固了, 不会再变了, 会一直保持这个结果, 这时就称为 resolved(已定性). 如果改变已经发生了, 你再对 Promise 对象添加回调函数也会立即得到这个结果, 这与事件(Event)完全不同, 事件的特点是, 如果你错过了它,再去监听,是得不到结果的

注意，为了行文方便，本章后面的resolved统一只指fulfilled状态，不包含rejected状态。

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。

Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 Stream 模式是比部署Promise更好的选择。

2. 基本用法
 
 ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。
 下面代码创造了一个Promise实例。

```js
  const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```
then 方法可以接受两个回调函数作为参数, 第一个回调函数是 Promise 对象的状态变为 resolved 时调用, 第二个回调函数是 Promise 对象的状态变为 rejected 时调用. 这两个函数都是可选的,
不一定要提供, 它们都接受 Promise 对象传出的 值作为参数

下面是一个 Promise 对象的简单例子

```js
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```
上面代码中, timeout 方法返回一个 Promise 实例, 表示一段时间以后才会发生的结果. 过了指定的时间(ms 参数)以后, Promise 实例的状态变为 resolved, 就会触发 then 方法绑定的回调函数

Promise 新建后就会立即执行

```js
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');
// Promise
// Hi!
// resolved
```


如果调用 resolve 函数 和 reject 函数时带有参数, 那么它们的参数会被传递给回调函数, reject 函数的参数通常是 Error 对象的实例, 表示抛出的错误; resolve 函数的参数除了正常的值以外,还可能是另外一个 Promise 实例, 比如像下面这样.

```js
const p1 = new Promise(function (resolve, reject) {
  // ...
});

const p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})
```

上面代码中, p1 和 p2 都是 Promise 的实例,

上面代码中，p1是一个 Promise，3 秒之后变为rejected。p2的状态在 1 秒之后改变，resolve方法返回的是p1。由于p2返回的是另一个 Promise，导致p2自己的状态无效了，由p1的状态决定p2的状态。所以，后面的then语句都变成针对后者（p1）。又过了 2 秒，p1变为rejected，导致触发catch方法指定的回调函数。

注意，调用resolve或reject并不会终结 Promise 的参数函数的执行。


```js

new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```
上面代码中，调用resolve(1)以后，后面的console.log(2)还是会执行，并且会首先打印出来。这是因为立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

一般来说, 调用resolve 或 reject 以后, Promise 的使命就完成了, 后续操作应该放到 then 方法里面, 而不应该直接写在 resolve 或 reject 的 后面. 所以,最好在她们前面加上 return 语句,这样就不会有意外了

```js
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})

```

3. Promise.prototype.then()


Promise 实例具有 then 方法, 也就是说, then 方法是定义在原型对象 Promise.prototype 上的,它的作用是为 Promise 实例添加状态改变时的回调函数. 前面说过, then 方法的第一个参数是 resolve 状态的回调函数, 第二个参数是 rejected 状态的回调函数, 它们都是可选的

then 方法返回的是一个新的 Promise 实例(注意, 不是原来那个 Promise 实例). 因此可以采用链式写法, 即 then 方法后面再调用另一个 then 方法.
```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```
上面的代码使用then方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数

采用链式的then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个Promise对象（即有异步操作），这时后一个回调函数，就会等待该Promise对象的状态发生变化，才会被调用。
```js
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```
上面代码中, 第一个 then 方法指定的回调函数, 返回的是另一个 Promise 对象, 这时, 第二个 then 方法指定的回调函数, 就会等待这个新的 Promise 对象状态发生变化,如果变为 resolved, 就调用第一个回调函数, 如果状态变为 rejected, 就调用第二个回调函数
如果采用箭头函数, 上面的代码就可以写得更简洁

```js
  getJSON('/post/1.json').then(post=>getJSON(post.commentURL)).then(comments=>console.log("resolved:",comments),err=>console.log('rejected',err))
```


4. Promise.prototype.catch()

Promise.prototype.catch() 方法是 .then(null,rejection)或 .then(undefined,rejection) 的别名, 用于指定发生错误的回调函数
```js
getJson('/post.json').then(function(posts){}).catch(function()=>{
  console.log("发生错误",error)
})
```
上面代码中 ,getJSON()方法返回一个 Promise 对象,如果该对象状态变为resolved, 则会调用 then()方法指定的回调函数,如果异步操作报错,状态就会变为 rejected, 则会调用 catch()
方法指定的回调函数, 处理这个错误, 另外, then() 方法指定的回调函数,如果运行中抛出错误, 也会被 catch() 方法捕获


```js

p.then((val) => console.log('fulfilled:', val))
  .catch((err) => console.log('rejected', err));

// 等同于
p.then((val) => console.log('fulfilled:', val))
  .then(null, (err) => console.log("rejected:", err));
```

下面是一个例子。

```js
const promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});
```
上面代码中，promise抛出一个错误，就被catch()方法指定的回调函数捕获。注意，上面的写法与下面两种写法是等价的。

```js
// 写法一
const promise = new Promise(function(resolve, reject) {
  try {
    throw new Error('test');
  } catch(e) {
    reject(e);
  }
});
promise.catch(function(error) {
  console.log(error);
});

// 写法二
const promise = new Promise(function(resolve, reject) {
  reject(new Error('test'));
});
promise.catch(function(error) {
  console.log(error);
});
```
比较上面两种写法，可以发现reject()方法的作用，等同于抛出错误。
如果 Promise 状态已经变成resolved，再抛出错误是无效的。

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });

```
上面代码中, Promise 在 resolve 语句后面, 再抛出错误,不会被捕获, 等于没有抛出, 因为 Promise 的状态一旦改变,就永久保持该状态, 不会再变了.

Promise 对象的错误具有冒泡性质, 会一直向后传递, 知道被捕获为止, 也就是说, 错误总是会被下一个 catch 语法捕获


```js
getJSON('/post/1.json').then(function(post){ return getJSON(post.commentURL)}).then(function(comments){
    //some code 
}).catch(function(error){
    // 处理前面三个 Promise 产生的错误
})

```

面代码中，一共有三个 Promise 对象：一个由getJSON()产生，两个由then()产生。它们之中任何一个抛出的错误，都会被最后一个catch()捕获。
一般来说，不要在then()方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。

```js
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
上面代码中, 第二种写法要好于第一种写法, 理由是第二种写法可以捕获前面 then 方法执行中的错误, 也更接近同步的写法(try/catch), 因此,建议总是使用 catch 方法,而不使用 then 方法的第二个参数

跟传统的try/catch代码块不同的是，如果没有使用catch()方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。

```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});

setTimeout(() => { console.log(123) }, 2000);

```


上面代码中,


