---
title: module
date: 2022-05-07 10:14:10
tags:
---
主要参考 阮一峰 es6 模块相关


1.  概述

历史上，JavaScript 一直没有模块（module）体系，无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。其他语言都有这项功能，比如 Ruby 的require、Python 的import，甚至就连 CSS 都有@import，但是 JavaScript 任何这方面的支持都没有，这对开发大型的、复杂的项目形成了巨大障碍。
在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。
ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

```js
let {stat,exists,readfile} = require('fs');
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码的实质是整体加载 fs 模块(即加载 fs 的所有方法), 生成一个对象 (_fs),然后再从这个对象上面读取 3 个方法, 这种加载称为"运行时加载", 因为只有运行时才能得到这个对象, 导致完全没方法在编译时做"静态优化".
ES6 模块不是对象,而是通过 export 命令显示指定输出的代码,再通过 import 命令输入.
```js
import { stat, exists, readFile } from 'fs';
```

上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

由于 es6模块是编译时加载,使得静态分析成为可能, 有了它, 就能进一步拓宽 JS 的语法, 比如引入宏和类型检验这些只能靠静态分析实现的功能.
除了静态加载带来的各种好处, es6 模块还有以下好处.
- 不再需要 UMD 模块格式了, 将来服务器和浏览器都会支持 es6 模块格式. 目前,通过各种工具库, 其实已经做到这一点.
- 将来浏览器的新 API 就能用模块格式提供, 不再必须做成全局变量或者 navigator对象的属性.
- 不再需要对象作为命名空间(比如 Math 对象), 未来这些功能可以通过模块提供
  
本章介绍 ES6 模块的语法, 下一章介绍如何在浏览器和 node 之中, 加载 es6 模块.

2. export 命令

模块功能主要由两个命令构成: export 和 import . export 命令用于规定模块的对外接口, import 命令用于输入其他模块提供的功能.

一个模块就是一个独立的文件. 该文件内部的所有变量,外部无法获取,如果你希望外部能够读取模块内部的某个变量, 就必须使用 export 关键字输出该变量,下面是一个 js 文件,里面使用 export 命令输出变量.

```js
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```
上面代码是profile.js文件，保存了用户信息。ES6 将其视为一个模块，里面用export命令对外部输出了三个变量

export的写法，除了像上面这样，还有另外一种。

```js

// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
```
上面代码在export命令后面，使用大括号指定所要输出的一组变量。它与前一种写法（直接放置在var语句前）是等价的，但是应该优先考虑使用这种写法。因为这样就可以在脚本尾部，一眼看清楚输出了哪些变量。

export 命令除了输出变量, 还可以输出函数或类

```js
export function multiply(x,y){
    return x*y
}
```
上面代码对外输出一个函数multiply。
通常情况下，export输出的变量就是本来的名字，但是可以使用as关键字重命名。
```js
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
}
```
上面代码使用as关键字，重命名了函数v1和v2的对外接口。重命名后，v2可以用不同的名字输出两次。
需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```js
// 报错
export 1;

// 报错
var m = 1;
export m;
```

上面两种方法都会报错,因为没有提供对外的接口, 第一种写法直接输出 1, 第二种写法通过变量m, 还是直接输出 1, 1 只是一个值,不是接口. 正确的写法是下面这样.

```js
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```
上面三种写法都是正确的，规定了对外的接口m。其他脚本可以通过这个接口，取到值1。它们的实质是，在接口名与模块内部变量之间，建立了一一对应的关系。
同样的，function和class的输出，也必须遵守这样的写法。
```js
// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};
```
另外，export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
```js
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```

上面代码输出变量 foo, 值为 bar, 500 毫秒之后变成 baz.

这一点与 CommonJS 规范完全不同。CommonJS 模块输出的是值的缓存，不存在动态更新，详见下文《Module 的加载实现》一节。
最后，export命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，下一节的import命令也是如此。这是因为处于条件代码块之中，就没法做静态优化了，违背了 ES6 模块的设计初衷。

```js
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

上面代码中，export语句放在函数之中，结果报错。

3. import 命令

使用 export 命令定义了模块的对外接口以后, 其他 js 文件就可以通过 import 命令加载这个模块.
```js
// main.js
import { firstName, lastName, year } from './profile.js';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```





























