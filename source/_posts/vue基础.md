---
title: vue基础
date: 2022-05-14 09:47:44
tags:
---
# 基础

## Vue.js 是什么?

Vue 是一套用于构建用户界面的渐进式框架

## Vue 实例

### 创建一个实例

每一个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的:

```js
    var vm = new Vue({
        // 选项
    })
```

### 数据与方法

当一个 Vue 实例被创建时, 它将 data 对象中的所有 property 加入到 Vue 的响应式系统中. 当这些 property 的值发生改变时, 视图将会发生响应, 即匹配更新为新的值

### 实例生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程--- 例如, 需要设置数据监听, 编译模版, 将实例挂载到 DOM 并在数据变化时更新 DOM 等. 同时在这个过程中也会运行一些叫做生命周期钩子的函数, 这给了用户在不同阶段添加自己代码的机会

## 模版语法

 Vue.js 使用了基于 html 的模版语法, 允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据. 所有 Vue.js 的模版都是合法的 HTML, 所以能被遵循规范的浏览器和 HTML 解析器解析

 在底层的实现上, Vue 将模版编译成虚拟 DOM 渲染函数. 结合响应系统, Vue 能够智能地计算出最少需要重新渲染多少组件, 并把 DOM 操作次数减到最少.

 ### 插值

1. 文本 使用双大括号
2. 原始 HTML
3.  Attribute 

双大括号语法不能作用在 HTML attribute 上, 遇到这种情况应该使用 v-bind 指令
4. 使用 javaScript 表达式


注意:

```js
模版表达式都被放在沙盒中, 只能访问全局变量的一个白名单, 如 Math 和 Date, 你不应该在模版表达式中视图访问用户定义的全局变量
```

### 指令

指令是带有 v- 前缀的特殊 attribute. 指令 attribute的值预期是单个 javaScript 表达式 (v-for 是例外情况, 稍后我们再讨论). 指令的职责是, 当表达式的值改变时, 将其产生的连带影响,响应地作用于 DOM, 回顾我们在介绍中看到的例子

### 参数

一些指令能够接收一个参数,在指令名称之后以冒号表示. 例如, v-bind 指令可以用于响应式更新 HTML attribute

```js
  <a v-bind:href="url">...</a>
```
在这里 href 是参数, 告知 v-bind 指令将该元素的 href attribute 与表达式 URL 的值绑定. 

另一个例子是 v-on 指令, 它用于监听 DOM 事件

```js
    <a v-on:click="doSomething">...</a>
```
### 动态参数

### 修饰符

### 缩写:
 它们看起来可能与普通的 html 略有不同, 但 : 与 @ 对于 attribute 名来说都是合法字符, 在所有支持 Vue 的浏览器都能被正确地解析. 而且,它们不会出现在最终渲染的标记中


 ## 计算属性和侦听器

 ### 计算属性

 模版内的表达式非常便利, 但是设计她们的初衷是用于简单运算的.  在模版中放入太多的逻辑会让模版过重且难以维护.


对于任何复杂逻辑, 你都应当使用计算属性.


#### 计算属性缓存 vs 方法



### 侦听器

虽然计算属性在大多数情况下更适合, 但有时也需要一个自定义的侦听器, 这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法, 来响应数据的变化, 当需要在数据变化时执行异步或开销比较大的操作时, 这个方式是最用的

在这个示例中, 使用 watch 选项允许我们执行异步操作, 限制我们执行该操作的频率, 并在我们得到最终结果钱, 设置中间状态,这些都是计算属性无法做到的.


## Class 与 Style 绑定

操作元素的 class 列表和内联样式是数据绑定的一个常见需求. 因为它们都是 attribute . 所以我们可以用 v-bind 处理它们; 只需要通过表达式计算出字符串结果即可. 不过, 字符串拼接麻烦且易错. 因此, 在将 v-bind 用于 class 和 style 时, Vue.js 做了专门的增强. 表达式结果的类型除了字符串外, 还可以是对象或数组

### 绑定 HTML class 

1. 对象语法

2. 数组语法

3. 用在组件上

 当在一个自定义组件上使用 class property时, 这些 class 将被添加到该组件的根元素上面. 这个元素上已经存在的 class 不会被覆盖

 例如, 如果你声明了这个组件:

 ```js
    Vue.component('my-component',{
        template: '<p class="foo bar">Hi</p>'
    })
 ```
 然后在使用它的时候添加一些class

```js
    <my-component class="baz boo"></my-component>
```

HTML 将被渲染为:

```js
    <p class="foo bar baz boo">Hi</p>
```

对于带数据绑定 class 也同样适用：


```js
<my-component v-bind:class="{ active: isActive }"></my-component>

```

当 isActive 为 truthy[1] 时，HTML 将被渲染成为：

```js
<p class="foo bar active">Hi</p>
```



###  绑定内联样式

#### 对象语法

v-bind:style 的对象语法十分直观-- 看着非常像 css , 但其实是一个 JavaScript 对象, css property 名可以用 驼峰式或短横线分隔来命名

```js
 <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```


```js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

直接绑定到一个样式对象通常更好, 这会让模版更清晰

```js

<div v-bind:style="styleObject"></div>

```



```js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}

```

同样的, 对象语法常常结合返回对象的计算属性使用


### 数组语法

v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上:

```js

<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### 自动添加前缀


### 多重值

从 2.3.0 起你可以为 style 绑定中的 property 提供一个包含多个值的数组, 常用于提供多个带前缀的值, 例如:

```js
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

```
这样写只会渲染数组中最后一个被浏览器支持的值, 在本例中, 如果浏览器支持不带浏览器前缀的 flexbox, 那么就只会渲染 display:flex



## 条件渲染

v-if 指令用于条件性地渲染一块内容. 这块内容只会在指令的表达式返回 truthy 值的时候会被渲染

```js
   <h1 v-if="awesome">Vue is awesome !</h1>
```
也可以用 v-else 添加一个 else 块:

```js
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>

```

在 <template> 元素上使用 v-if 条件渲染分组
因为  v-if 是一个指令, 所以必须将它添加到一个元素上. 但是如果想切换多个元素呢? 此时可以把一个 <template> 元素当做不可见的包裹元素, 并在上面使用 v-if. 最终的渲染结果将不包含 <template> 元素


```js
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

v-else

你可以使用 v-else 指令来表示 v-if 的 'else 块'

```js
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>

```

v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面, 否则它将不会被识别


v-else-if

2.1.0 新增


v-else-if 顾名思义, 充当 v-if 的 'else-if 块', 可以连续使用:



用 key 管理可复用的元素

Vue 会尽可能高效地渲染元素,通常会复用已有元素而不是从头开始渲染.这么做除了使 Vue 变得非常快之外, 还有其它一些好处. 例如, 如果你允许用户在不同的登录方式之间切换:


```js
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>

```

那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，<input> 不会被替换掉——仅仅是替换了它的 placeholder。

自己动手试一试，在输入框中输入一些文本，然后按下切换按钮：

这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key attribute 即可：

```js
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>

```

v-show

另一个用于根据条件展示元素的选项是 v-show 指令, 用法大致一样

```js
    <h1 v-show="ok">hello</h1>

```

不同的是 带有 v-show 的元素始终会被渲染并保留在 DOM 中. v-show 只是简单地切换元素的 css property display


```js
 注意, v-show 不支持 <template> 元素, 也不支持 v-else

```
v-if  vs v-show

1. v-if 是"真正"的条件渲染,因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
2. v-if 也是惰性的: 如果在初始渲染时条件为假, 则什么也不做--直到条件第一次为真的时, 才会开始渲染条件块.

相比之下, v-show 就简单得多-- 不管初始条件是什么, 元素总是会被渲染, 并且只是简单地基于 css 进行切换

一版来说, v-if 有更高的切换开销,而 v-show 有更高的初始渲染开销. 因此, 如果需要非常频繁地切换,则使用 v-show 较好;如果在运行时条件很少改变, 则使用 v-if 较好

v-if 与 v-for 一起使用


```js
   不推荐同时使用 v-if 和 v-for 
```

当 v-if 与 v-for 一起使用时, v-for 具有比 v-if 更高的优先级. 

## 列表渲染

用 v-for 把一个数组对应为一组元素

我们可以使用 v-for 指令基于一个数组来渲染一个列表, v-for 指令需要使用 item in items 形式的特殊语法, 其中 items 是源数据对象, 而 items 则是被迭代的数组元素的别名

```js
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})

```

在 v-for里使用对象


维护状态

当 Vue 正在更新使用 v-for 渲染的元素列表时, 它默认使用"就地更新"的策略. 如果数据项的顺序被改变, Vue 将不会移动 DOM 元素来匹配数据项的顺序, 而是就地更新每个元素, 并且确保它们在每个索引位置正确渲染.

这个默认的模式是高效地, 但是只适用于不依赖子组件状态或临时 DOM 转台的列表渲染输出

为了给 Vue 一个提示, 以便它能跟踪每个节点的身份, 从而重用和重新排序现有元素, 你需要为每项提供一个唯一 key attribute


```js
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>

```

建议尽可能在使用 v-for 时提供 key attribute,除非遍历输出的 DOM 内容非常简单, 或者是刻意依赖默认行为以获取性能上的提升

因为它是 Vue 识别节点的一个通用机制，key 并不仅与 v-for 特别关联。后面我们将在指南中看到，它还具有其它用途。

```js
不要使用对象或数组之类的非基本类型值作为 v-for 的 key。请用字符串或数值类型的值。

```

数组更新检测

* 变更方法

Vue 将被侦听的数组的变更方法进行了包裹, 所以它们也将会触发视图更新,这些被包裹过的方法包括
  * push()
  * pop()
  * shift()
  * unshift()
  * splice()
  * sort()
  * reverse()

* 替换数组

 变更方法, 顾名思义, 会变更调用了这些方法的原始数组.相比之下, 也有非变更方法, 例如 filter() , concat() 和 slice(). 它们不会变更原始数组, 而总是返回一个新数组,当使用非变更方法时,可以用新数组替换旧数组:

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表,幸运的是, 事实并非如此, Vue 为了使得 DOM 元素得到更大范围的重用而实现了一些智能的启发式方法,所以用一个含有相同元素的数组去替换原来的数组是非常高效地操作

* 注意事项

显示过滤/排序后的结果

有时, 我们想要显示一个数组经过过滤或排序后的版本, 而不实际变更或重置原始数据. 在这种情况下, 可以创建一个计算属性, 来返回过滤或排序后的数组

例如:

```js
<li v-for="n in evenNumbers">{{ n }}</li>

```

```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}

```

在计算属性不适用的情况下(例如, 在嵌套 v-for 循环中) 你可以使用一个方法:

```js
<ul v-for="set in sets">
  <li v-for="n in even(set)">{{ n }}</li>
</ul>
```


```js
data: {
  sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}

```

在 v-for 里使用范围值


v-for 也可以接受整数. 在这种情况下, 它会把模板重复对应次数

```js
    <div>
  <span v-for="n in 10">{{ n }} </span>
</div>

```

在 <template> 上使用 v-for

类似于 v-if, 你也可以利用带有 v-for 的<template> 来循环渲染一段包含多个元素的内容,
比如:

```js
 <ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>

```


v-for 与 v-if 一同使用


> 注意我们不推荐在同一元素上使用 v-if 和 v-for。更多细节可查阅风格指南。





















### 1. 如何在 非 vue 实例中使用 vuex 或者 router 
直接在使用的地方引用就可以的

### 2. Vue 的基本原理

当一个 Vue 实例创建时, Vue 会遍历 data 的属性, 用 Object.defineProperty 将它们转换为 getter/setter, 并且在内部追踪相关依赖


### 3. vue 中的双向数据绑定是什么意思? 单向数据绑定呢



### 4. vue 中双向数据绑定的原理

Vue.js 是采用数据劫持结合发布者-订阅者模式的方式



## 5. 什么是 vue ?

    Vue 是一款用于构建用户界面的 javascript 框架, 它基于标准 html,css 和 js 构建, 并提供了一套声明式的 ,组件化的变成模型,帮助你高效地开发用户界面. 无论是简单还是复杂的界面,Vue 都可以胜任


## 6 vue 2 和 vue 3 的区别 ,为什么使用 vue3


### 问题 

#### 1. Vue 3.0 也要看一下

#### 2. Vue 组件是 Vue 的实例吗


####  3 . Vue 包含哪些东西 

Vue-cli, vuex ,vuerouter(已经基本掌握) , axios , vue   全家桶 ,npm 




#### Vue 组件如何拆分






