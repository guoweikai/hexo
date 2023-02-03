---
title: vue基础
date: 2022-05-14 09:47:44
tags:
---
# 基础
## 介绍
   **Vue.js 是什么?**

    Vue 是一套用于构建用户界面的渐进式框架,与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。
   **声明式渲染**
   
## Vue 实例
**创建一个实例**
  每一个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的:
  ```js
    var vm = new Vue({
        // 选项
    })
  ```

**数据与方法**

当一个 Vue 实例被创建时, 它将 data 对象中的所有 property 加入到 Vue 的响应式系统中. 当这些 property 的值发生改变时, 视图将会发生响应, 即匹配更新为新的值

**实例生命周期钩子**

每个 Vue 实例在被创建时都要经过一系列的初始化过程--- 例如, 需要设置数据监听, 编译模版, 将实例挂载到 DOM 并在数据变化时更新 DOM 等. 同时在这个过程中也会运行一些叫做生命周期钩子的函数, 这给了用户在不同阶段添加自己代码的机会

## 模版语法

 Vue.js 使用了基于 html 的模版语法, 允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据. 所有 Vue.js 的模版都是合法的 HTML, 所以能被遵循规范的浏览器和 HTML 解析器解析

 在底层的实现上, Vue 将模版编译成虚拟 DOM 渲染函数. 结合响应系统, Vue 能够智能地计算出最少需要重新渲染多少组件, 并把 DOM 操作次数减到最少.

**插值**
1. 文本 使用双大括号
2. 原始 HTML
3.  Attribute 
    双大括号语法不能作用在 HTML attribute 上, 遇到这种情况应该使用 v-bind 指令
4. 使用 javaScript 表达式


注意:
>模版表达式都被放在沙盒中, 只能访问全局变量的一个白名单, 如 Math 和 Date, 你不应该在模版表达式中视图访问用户定义的全局变量


**指令**

指令是带有 v- 前缀的特殊 attribute. 指令 attribute的值预期是单个 javaScript 表达式 (v-for 是例外情况, 稍后我们再讨论). 指令的职责是, 当表达式的值改变时, 将其产生的连带影响,响应地作用于 DOM, 回顾我们在介绍中看到的例子

1. 参数

一些指令能够接收一个参数,在指令名称之后以冒号表示. 例如, v-bind 指令可以用于响应式更新 HTML attribute

```js
  <a v-bind:href="url">...</a>
```
在这里 href 是参数, 告知 v-bind 指令将该元素的 href attribute 与表达式 URL 的值绑定. 

另一个例子是 v-on 指令, 它用于监听 DOM 事件

```js
    <a v-on:click="doSomething">...</a>
```
2. 动态参数

3. 修饰符
   
**缩写**
 它们看起来可能与普通的 html 略有不同, 但 : 与 @ 对于 attribute 名来说都是合法字符, 在所有支持 Vue 的浏览器都能被正确地解析. 而且,它们不会出现在最终渲染的标记中


 ## 计算属性和侦听器

**计算属性**

 >模版内的表达式非常便利, 但是设计她们的初衷是用于简单运算的.  在模版中放入太多的逻辑会让模版过重且难以维护.

```js
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 message 的翻转字符串。当你想要在模板中的多处包含此翻转字符串时，就会更加难以处理。

对于任何复杂逻辑, 你都应当使用**计算属性**.


1. 计算属性缓存 vs 方法
2. 

**侦听器**

虽然计算属性在大多数情况下更适合, 但有时也需要一个自定义的侦听器, 这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法, 来响应数据的变化, 当需要在数据变化时执行异步或开销比较大的操作时, 这个方式是最用的

在这个示例中, 使用 watch 选项允许我们执行异步操作, 限制我们执行该操作的频率, 并在我们得到最终结果钱, 设置中间状态,这些都是计算属性无法做到的.


## Class 与 Style 绑定

操作元素的 class 列表和内联样式是数据绑定的一个常见需求. 因为它们都是 attribute . 所以我们可以用 v-bind 处理它们; 只需要通过表达式计算出字符串结果即可. 不过, 字符串拼接麻烦且易错. 因此, 在将 v-bind 用于 class 和 style 时, Vue.js 做了专门的增强. 表达式结果的类型除了字符串外, 还可以是对象或数组

**绑定 HTML class** 

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


**绑定内联样式**

1. 对象语法

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


2. 数组语法

v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上:

```js

<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

3. 自动添加前缀


4. 多重值

从 2.3.0 起你可以为 style 绑定中的 property 提供一个包含多个值的数组, 常用于提供多个带前缀的值, 例如:

```js
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

```
这样写只会渲染数组中最后一个被浏览器支持的值, 在本例中, 如果浏览器支持不带浏览器前缀的 flexbox, 那么就只会渲染 display:flex



## 条件渲染

**v-if**
1. v-if 指令用于条件性地渲染一块内容. 这块内容只会在指令的表达式返回 truthy 值的时候会被渲染

```js
   <h1 v-if="awesome">Vue is awesome !</h1>
```
也可以用 v-else 添加一个 else 块:

```js
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>

```


2. 在 template 元素上使用 v-if 条件渲染分组
因为  v-if 是一个指令, 所以必须将它添加到一个元素上. 但是如果想切换多个元素呢? 此时可以把一个 template 元素当做不可见的包裹元素, 并在上面使用 v-if. 最终的渲染结果将不包含 template 元素


```js
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

3. v-else

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


4. v-else-if

  2.1.0 新增
  v-else-if 顾名思义, 充当 v-if 的 'else-if 块', 可以连续使用:

5. 用 key 管理可复用的元素

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

那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，input 不会被替换掉——仅仅是替换了它的 placeholder。

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

**v-show**

另一个用于根据条件展示元素的选项是 v-show 指令, 用法大致一样

```js
    <h1 v-show="ok">hello</h1>

```

不同的是 带有 v-show 的元素始终会被渲染并保留在 DOM 中. v-show 只是简单地切换元素的 css property display


```js
 注意, v-show 不支持 <template> 元素, 也不支持 v-else

```
**v-if vs v-show**

1. v-if 是"真正"的条件渲染,因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
2. v-if 也是惰性的: 如果在初始渲染时条件为假, 则什么也不做--直到条件第一次为真的时, 才会开始渲染条件块.

相比之下, v-show 就简单得多-- 不管初始条件是什么, 元素总是会被渲染, 并且只是简单地基于 css 进行切换

一版来说, v-if 有更高的切换开销,而 v-show 有更高的初始渲染开销. 因此, 如果需要非常频繁地切换,则使用 v-show 较好;如果在运行时条件很少改变, 则使用 v-if 较好

**v-if 与 v-for 一起使用**
>不推荐同时使用 v-if 和 v-for 

当 v-if 与 v-for 一起使用时, v-for 具有比 v-if 更高的优先级. 

## 列表渲染

**数组**

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
**对象**


**维护状态**

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

**数组更新检测**

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

**在v-for里使用范围值**


v-for 也可以接受整数. 在这种情况下, 它会把模板重复对应次数

```js
    <div>
  <span v-for="n in 10">{{ n }} </span>
</div>

```

**在 template 上使用 v-for**

类似于 v-if, 你也可以利用带有 v-for 的 template来循环渲染一段包含多个元素的内容,
比如:

```js
 <ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>

```


**v-for 与 v-if 一同使用**


> 注意我们不推荐在同一元素上使用 v-if 和 v-for。更多细节可查阅风格指南。可以利用计算属性,过滤出啦


**在组件上使用 v-for**

在自定义组件上,你可以像在任何普通元素上一样使用 v-for

```js
<my-component v-for="item in items" :key="item.id"></my-component>
```
2.20+ 的版本里, 当在组件上使用 v-for 时 key 现在是必须得

然而, 任何数据都不会背自动传递到组件里, 因为组件有自己独立的作用域, 为了把迭代数据传递到组件里,我们要使用 prop


## 事件处理
**监听事件**
可以用 v-on 指令监听 DOM 事件, 并在触发时运行一些 js 代码

**事件处理方法**
然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 v-on 指令中是不可行的。因此 v-on 还可以接收一个需要调用的方法名称。
**内联处理器中的方法**
除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法：

```js
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```

```js
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 $event 把它传入方法：

```js
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```
```js
// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```


**事件修饰符**

在事件处理程序中调用 event.preventDefault() 事件默认行为 或 event.stopPropagation() 是非常常见的需求. 尽管我们可以在方法中轻松实现这点, 但更好的方式是:方法只有纯粹的数据逻辑, 而不是去处理 DOM 事件细节

**按键修饰符**



**为什么在 HTML 中监听事件?**

你可能注意到这种事件监听的方式违背了关注点分离 (separation of concern) 这个长期以来的优良传统。但不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 v-on 有几个好处：

1. 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。

2. 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。

3. 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们


## 表单输入绑定

**基础用法**
你可以用 v-model 指令在表单 input ,textarea 及 select 元素上创建双向数据绑定. 它会根据控制类型自动选取正确的方法来更新元素. 尽管有些神奇, 但 v-model 本质上不过是语法糖,它负责监听用户的输入事件以更新数据, 并对一些极端场景进行一些特殊处理.

> v-model 会忽略所有表单元素的 value , checked, selected attribute 的初始值而总是将 Vue 实例的数据作为数据来源.你应该通过 js 在 data 选项中声明初始值

* text 和 textarea 元素使用value property 和 input 事件
* checkbox 和 radio 使用 checked property 和 change 事件
* select 字段将 value 作为 prop 并将 change 作为事件
  

> 对于需要使用输入法的语言, 你会发现 v-model 不会再输入法组合文字过程中得到更新, 如果你也想处理这个过程, 请使用 input 事件


1. 文本
2. 多行文本
3. 复选框
    单个复选框, 绑定到布尔值

    ```js
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
    ```
    多个复选框,绑定到同一个数组

    ```js
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
    ``` 
    ```js
    new Vue({
    el: '...',
    data: {
      checkedNames: []
    }
    })

    ```
  4. 单选按钮
  ```js
    <div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
  ```


  ```js
  new Vue({
    el: '#example-4',
    data: {
      picked: ''
    }
  })

  ```
5. 选择框
   * 单选是
  ```js
  <div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
  ```

  ```js
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
  ```
> 如果 v-model 表达式的初始值未能匹配任何选择, select 元素将被渲染为 '未选中'状态,在 ios 中, 这会使用户无法选择第一个选项, 因为这样的情况下, ios 不会触发 change 事件, 因此,更推荐像上面这样提供一个值为空的禁用选项

多选时(绑定到一个数组)

```js
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>

```


```js
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```

用 v-for 渲染的动态选项

```js
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>

```

```js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

**值绑定**


**修饰符**
1. .lazy
  在默认情况下, v-model 在每次 input 事件触发后将输入框的值与数据进行同步(除了上述输入法组合文字时). 你可以添加 lazy 修饰符,从而转为在 change 事件之后进行同步
  > input 输入框的 onchange 事件,要在 input 市区焦点的时候才会触发;
  在输入框内容变化的时候不会触发 change, 当鼠标在其他地方点一下才会触发
  onchange 事件也可用于单选框与复选卡改变后触发的事件
2. .number
   如果想自动将用户的输入值转为数值类型,可以给 v-model 添加 number 修饰符 :

   ```js
    <input v-model.number="age" type="number">

   ```
3. .trim

  如果要自动过滤用户输入的首尾空白字符, 可以给 v-model 添加trim 修饰符

```js
<input v-model.trim="msg">

```

**在组件上使用 v-model**

HTML 原生的输入元素类型并不总能满足需求。幸好，Vue 的组件系统允许你创建具有完全自定义行为且可复用的输入组件。这些输入组件甚至可以和 v-model 一起使用！
要了解更多，请参阅组件指南中的自定义输入组件。



## 组件基础

**基本实例** 

这里有一个 Vue 组件的示例

```js
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```
组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 <button-counter>。我们可以在一个通过 new Vue 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：

```js
<div id="components-demo">
  <button-counter></button-counter>
</div>

```

```js
new Vue({ el: '#components-demo' })

```

因为组件是可复用的 Vue 实例, 所以它们与 new Vue 接收相同的选项, 例如 data, computed ,watch ,methods 以及生命周期钩子等, 仅有的例外是像 el 这样根实例特有的选项


**组件的复用**

data 必须是一个函数

**组件的组织**

通常一个应用会以一棵嵌套的组件树的形式来组织：


例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：全局注册和局部注册。至此，我们的组件都只是通过 Vue.component 全局注册的

```js

Vue.component('my-component-name', {
  // ... options ...
})

```

全局注册的组件可以用在其被注册之后的任何 (通过 new Vue) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

**通过 prop 向子组件传递数据**
prop 是你可以在组件上注册的一些自定义 attribute, 当一个值传递给一个 prop attribute 的时候, 它就变成了那个组件实例的一个 property. 
一个组件默认可以拥有任意数量的 prop, 任何值都可以传递给任何 prop. 在上述模版中,你会发现我们能够在组件实例中访问这个值.就像访问 data 中的值一样


可以是用 v-bind 动态传递 prop

**单个根元素**

**监听子元素事件**
  使用事件抛出一个值
  在组件上使用 v-model

**通过插槽分发内容**

和 HTML元素以信仰,我们经常需要向一个组件传递内容,像这样

```js

```


**动态组件**


**解析 DOM 模板时的注意事项**
有些 HTML 元素, 诸如 ul ,ol, table , select , 对于哪些元素可以出现在其内部是有严格限制的. 而有些元素, 诸如li tr option 只能出现在其它某些特定的元素内部

这会导致我们使用这些有约束条件的元素时遇到一些问题. 例如:

```js
  <table>
  <blog-post-row></blog-post-row>
</table>
```
这个自定义组件 blog-post-row 会被作为无效的内容提升到外边, 并导致最终渲染结果出错.
幸好 这个特殊的 is attribute 给我们一个变通的方法

```js
<table>
  <tr is="blog-post-row"></tr>
</table>

```
需要注意的是如果我们从以下来源使用模板的话，这条限制是不存在的：

* 字符串 (例如：template: '...')
* 单文件组件 (.vue)
* script type="text/x-template"



# 深入了解组件

## 组件注册

**组件名**
在注册一个组件的时候,我们始终需要给它一个名字,不如在全局注册的时候我们已经看到了:

```js
Vue.component('my-component-name', { /* ... */ })
```
该组件名就是 Vue.component 的第一个参数。

你给予组件的名字可能依赖于你打算拿它来做什么。当直接在 DOM 中使用一个组件 (而不是在字符串模板或单文件组件) 的时候，我们强烈推荐遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)。这会帮助你避免和当前以及未来的 HTML 元素相冲突。


**组件名大小写**

**全局注册**

到目前为止, 我们只用过  Vue.component 来创建组件:

```js
  Vue.component("my-component-name",{
    // ....选项...
  })

```

这些组件是全局注册的, 也就是说它们在注册之后可以用在任何新创建的 Vue 根实例(new  Vue)的模板中. 比如:

```js

  Vue.component('component-a', { /* ... */ })
  Vue.component('component-b', { /* ... */ })
  Vue.component('component-c', { /* ... */ })

  new Vue({ el: '#app' })
```

在所有子组件中也是如此，也就是说这三个组件在各自内部也都可以相互使用。

**局部注册**
全局注册往往是不够理想的. 比如, 如果你使用一个想 webpack 这样的构建系统, 全局注册所有的组件意味着即便你已经不再使用一个组件了,它仍然会被包含在你最终的构建结果中. 这造成了用户下载的 js 的无畏的增加

在这些情况下, 你可以通过一个普通的 js对象来定义组件

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

```
然后在 components 选项中定义你想要实用的组件


```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})

```
对于 components 对象中的每个 property 来说, 其 property 名就是自定义元素的名字, 其 property 值就是这个组件的选项对象


> 注意局部注册的组件在其子组件中不可用. 例如. 如果你希望 ComponentA 在 ComponentB中可用, 则你需要这么写

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```
注意在 ES2015+ 中，在对象中放一个类似 ComponentA 的变量名其实是 ComponentA: ComponentA 的缩写，即这个变量名同时是：
* 用在模板中的自定义元素的名称
* 包含了这个组件选项的变量名



**基础组件的自动化全局注册**

可能你的许多组件只是包裹了一个输入框或按钮之类的元素, 是相对通用的. 我们有时候会把它们称为基础组件, 它们会在各个组件中被频繁地用到

所以会导致很多组件都会有一个包含基础组件的长列表

```js

import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'

export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}
```

而只是用于模板中的一小部分：

```js
<BaseInput
  v-model="searchText"
  @keydown.enter="search"
/>
<BaseButton @click="search">
  <BaseIcon name="search"/>
</BaseButton>


```

如果你恰好使用了 webpack(或在内部使用了 webpack 的 Vue CLI 3+),那么就可以使用 require.context 只全局注册这些非常通用的基础组件, 


## Prop

**Prop的大小写**

HTML 中的 attribute 名是大小写不敏感的, 所以浏览器会把所有大写字符解释为小写字符. 这意味着当你使用 DOM 中的模板时, camelCase(驼峰命名法)的 prop 名需要使用等价的 kebab-case(短横线分割命名)命名:

```js
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```
```js
  <!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```


重申一次, 如果你使用模版字符串模版, 那么这个限制就不存在了


**Prop 类型**

传递静态或动态 Prop
    传入一个数字
    传入一个布尔值


**单向数据流**

所有的 prop 都使得其父子 prop 之间形成了一个 单向下行绑定:父级 prop 的更新会向下流动到子组件中, 但是反过来则不行,这样会防止从子组件意外变更父级组件的状态, 从而导致你的应用的数据流向难以理解

额外的, 每次父级组件发生变更时,子组件中所有的 prop 都讲会刷新为最新的值, 这意味着你不应该在一个子组件内部改变 prop, 如果你这样做了, Vue 会在浏览器的控制台中发出警告

1. 这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。在这种情况下，最好定义一个本地的 data property 并将这个 prop 用作其初始值

```js
  props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}

```

2. 这个 prop 以一种原始的值传入且需要进行转换. 在这种情况下, 最好使用 这个 prop 的值来定义一个计算属性


```js
  props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

> 注意在 js 中对象和数组是通过引用传入的. 所以对于一个数组或对象类型的 prop 来说, 在子组件中改变变更这个对象或数组本身将会影响到父组件的状态

**prop 验证**


**非 prop 的 Attibute**

一个 非 prop 的 attribute 是指传向一个组件, 但是该组件没有响应的 prop 定义的 attribute

因为显式定义的 prop 适用于向一个子组件传入信息, 然后组件库的作者并不总能遇见组件会被用于怎样的场景. 这也是为什么组件可以接受任意的 attribute, 而这些 attribute 会被添加到这个组件的根元素上.

例如, 想象一下你通过一个 bootstrap 插件使用了一个第三方的 bootstrap-data-input 组件, 这个插件需要在其 input 上使用一个 data-data-picker attribute , 我们可以将这个 attribute 添加到你的组件实例上

```js

<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>

```
然后这个 data-date-picker="activated" attribute 就会自动添加到 bootstrap-date-input 的根元素上。

1. 替换/合并已有的 Attribute
2. 禁用 Attribute 继承




## 自定义事件

**事件名**

不同于组件和 prop,事件名不存在任何自动化的大小写转换,而是触发的事件名需要完全匹配监听这个事件所用的名称


**自定义组件的 v-model**

一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value attribute 用于不同的目的。model 选项可以用来避免这样的冲突：

```js
  Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 v-model 的时候：


```js

<base-checkbox v-model="lovingVue"></base-checkbox>

```


这里的 lovingVue 的值将会传入这个名为 checked 的 prop。同时当 <base-checkbox> 触发一个 change 事件并附带一个新的值的时候，这个 lovingVue 的 property 将会被更新。



> 注意你仍然需要在组件的 props 选项里声明 checked 这个 prop。



**将原生事件绑定到组件**

**.sync 修饰符**

在有些情况下, 我们可能需要对一个 prop 进行"双向绑定". 不幸的是, 真正的双向绑定会带来维护上的问题,因为子组件可以变更父组件, 且在父组件和子组件两侧都没有明显的变更来源.

这也是为什么我们推荐以 update:myPropName 的模式触发事件取而代之. 举个例子, 在一个包含 title prop 的假设的组件中, 我们可以用一下方法表达对其赋新值的意图

```js
  this.$emit('update:title',newTitle)
```

然后父组件可以监听那个事件并根据需要更新一个本地的数据 property . 例如

```js
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>

```

为了方便起见，我们为这种模式提供一个缩写，即 .sync 修饰符：


```js
<text-document v-bind:title.sync="doc.title"></text-document>

```
>注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无效的)。取而代之的是，你只能提供你想要绑定的 property 名，类似 v-model。


当我们用一个对象同时设置多个 prop 的时候，也可以将这个 .sync 修饰符和 v-bind 配合使用：

```js
<text-document v-bind.sync="doc"></text-document>

```

这样会把 doc 对象中的每一个 property (如 title) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。


>将 v-bind.sync 用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。



## 插槽

**插槽内容**

Vue 实现了一套内容分发的 API, 这套 API 的设计灵感源自  web Components, 将 slot 元素作为承载分发内容的出口

插槽内容可以是文字, 模版代码, 甚至组件, 都是可以的

**编译作用域**

当你想在一个插槽中使用数据时, 例如:

```js
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

该插槽跟模板的其它地方一样可以访问相同的实例 property(也就是相同的作用域), 而不能访问 navigation-link 的作用域,例如 url 是访问不到的:

**后备内容**

**具名插槽**

有时我们需要多个插槽,例如对于一个带有如下模版的 base-layout 组件


```js
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>

```


对于这样的情况,  slot 元素有一个特殊的 attribute: name 这个 

> 注意 v-slot只能添加在 template 上


**作用域插槽**

有时让插槽内容能够访问子组件中才有的数据是很有用的. 例如, 设想一个带有如下模版的 current-user 组件

```js

<span>
  <slot>{{ user.lastName }}</slot>
</span>

```

我们可能想换掉备用的内容, 用名而非姓来显示.如下:

```js
<current-user>
  {{ user.firstName }}
</current-user>

```

然而上述代码不会正常工作，因为只有 current-user 组件可以访问到 user，而我们提供的内容是在父级渲染的。

为了让 user 在父级的插槽内容中可用, 我们可以将 user 作为 slot 元素的 一个 attribute 绑定上去

```js

<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>

```


绑定在 slot 元素上的 attribute 被称为 插槽 prop. 现在在父级作用域中, 我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字

```js
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>

```


在这个例子中, 我们选择将包含所有插槽 prop 的对象命名为 slotProps, 但你也可以使用任意你喜欢的名字



**独占默认插槽的缩写语法**

```js
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>

```


**解构插槽 Prop**

作用域插槽的内部工作原理是将你的插槽内容包裹在一个拥有单个参数的函数里:


```js
function (slotProps) {
  // 插槽内容
}

```


这意味着 v-slot 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。所以在支持的环境下 (单文件组件或现代浏览器)，你也可以使用 ES2015 解构来传入具体的插槽 prop，如下：

```js
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>

```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 user 重命名为 person：

```js
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```
你甚至可以定义后备内容，用于插槽 prop 是 undefined 的情形：

```js
<current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
</current-user>

```

**动态插槽名**


动态指令参数也可以用在 v-slot 上, 来定义动态的插槽名:

```js
  <base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

**具名插槽的缩写**

跟 v-on 和 v-bind 一样, v-slot 也有缩写, 即把参数之前的所有内容(v-slot:)替换为 字符 #, 例如 v-slot:header 可以被重写为 #header


```js
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

然而，和其它指令一样，该缩写只在其有参数的时候才可用。这意味着以下语法是无效的：

```js
<!-- 这样会触发一个警告 -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>

```
如果你希望使用缩写的话，你必须始终以明确插槽名取而代之：

```js
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>

```

**其它示例**

插槽 prop 允许我们将插槽转换为可复用的模板，这些模板可以基于输入的 prop 渲染出不同的内容。这在设计封装数据逻辑同时允许父级组件自定义部分布局的可复用组件时是最有用的。

例如,我们要实现一个  todo-list 组件, 它是一个列表且包含布局和过滤逻辑


```js
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>

```

我们可以将每个 todo 作为父级组件的插槽, 以此通过父级组件对其进行控制. 然后将 todo 作为 一个插槽 prop 进行绑定


```js
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    <!--
    我们为每个 todo 准备了一个插槽，
    将 `todo` 对象作为一个插槽的 prop 传入。
    -->
    <slot name="todo" v-bind:todo="todo">
      <!-- 后备内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>

```

现在当我们使用 todo-list 组件的时候，我们可以选择为 todo 定义一个不一样的 template 作为替代方案，并且可以从子组件获取数据：


```js

<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>

```

**废弃的语法**
带有 slot attribute 的具名插槽
带有 slot-scope attribute 的作用域插槽


## 动态组件 & 异步组件

**动态组件**

我们之前在一个多标签的界面中使用 is attribute 来切换不同的组件:

```js
<component v-bind:is="currentTabComponent"></component>

```

当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重新渲染导致的性能问题。例如我们来展开说一说这个多标签界面：


你会注意到，如果你选择了一篇文章，切换到 Archive 标签，然后再切换回 Posts，是不会继续展示你之前选择的文章的。这是因为你每次切换新标签的时候，Vue 都创建了一个新的 currentTabComponent 实例。

重新创建动态组件的行为通常是非常有用的，但是在这个案例中，我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。为了解决这个问题，我们可以用一个 <keep-alive> 元素将其动态组件包裹起来。

```js
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>

```


> 注意这个 keep-alive 要求被切换到的组件都有自己的名字, 无论是通过组件的 name 选项还是局部/全局注册


**异步组件** 

在大型应用中, 我们可能需要将应用分割成小一些的代码块,并且只在需要的时候才从服务器加载一个模块, 为了简化, Vue 允许你以一个工厂函数的方式定义你的组件, 这个工厂函数会异步解析你的组件定义.  Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数, 且会把结果缓存起来供未来重渲染.例如:


处理加载状态:
这里的异步组件工厂函数也可以返回一个如下格式的对象:

```js
const AsyncComponent = () => ({
  // 需要加载的组件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 如果提供了超时时间且组件加载也超时了，
  // 则使用加载失败时使用的组件。默认值是：`Infinity`
  timeout: 3000
})

```


## 处理边界情况

访问元素 & 组件

在绝大多数情况下, 我们最好不要触达另一个组件实例内部或手动操作 DOM 元素, 不过也确实在一些情况下做这些事情是合适的

1.访问根实例

```js
// Vue 根实例
new Vue({
  data: {
    foo: 1
  },
  computed: {
    bar: function () { /* ... */ }
  },
  methods: {
    baz: function () { /* ... */ }
  }
})

```

所有的子组件都可以将这个实例作为一个全局 store 来访问或使用

```js
// 获取根组件的数据
this.$root.foo

// 写入根组件的数据
this.$root.foo = 2

// 访问根组件的计算属性
this.$root.bar

// 调用根组件的方法
this.$root.baz()

```

> 对于 demo 或非常小型的有少量组件的应用来说这是很方便的. 不过这个模式扩展到中大型应用来说就不然了, 因此在绝大数情况下, 强烈推荐使用  Vuex 来管理应用的状态


2. 访问父级组件实例
和 $root 类似，$parent property 可以用来从一个子组件访问父组件的实例。它提供了一种机会，可以在后期随时触达父级组件，以替代将数据以 prop 的方式传入子组件的方式。

>在绝大多数情况下，触达父级组件会使得你的应用更难调试和理解，尤其是当你变更了父级组件的数据的时候。当我们稍后回看那个组件的时候，很难找出那个变更是从哪里发起的。

另外在一些可能适当的时候，你需要特别地共享一些组件库。举个例子，在和 JavaScript API 进行交互而不渲染 HTML 的抽象组件内，诸如这些假设性的 Google 地图组件一样：

```js

<google-map>
  <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map>

```


3. 访问子组件实例或子元素

4. 依赖注入


在此之前, 在我们描述访问父级组件实例的时候, 展示过一个类似这样的例子:

```js

<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```

在这个组件里，所有 google-map 的后代都需要访问一个 getMap 方法，以便知道要跟哪个地图进行交互。不幸的是，使用 $parent property 无法很好的扩展到更深层级的嵌套组件上。这也是依赖注入的用武之地，它用到了两个新的实例选项：provide 和 inject。

provide 选项允许我们指定我们想要提供给后代组件的数据/方法。在这个例子中，就是 google-map 内部的 getMap 方法：

```js
provide: function () {
  return {
    getMap: this.getMap
  }
}

```
然后在任何后代组件里，我们都可以使用 inject 选项来接收指定的我们想要添加在这个实例上的 property：

```js
inject: ['getMap']

```

你可以在这里看到完整的示例。相比 $parent 来说，这个用法可以让我们在任意后代组件中访问 getMap，而不需要暴露整个 google-map 实例。这允许我们更好的持续研发该组件，而不需要担心我们可能会改变/移除一些子组件依赖的东西。同时这些组件之间的接口是始终明确定义的，就和 props 一样。

实际上，你可以把依赖注入看作一部分“大范围有效的 prop”，除了：
 * 祖先组件不需要知道哪些后代组件使用它提供的 property
 * 后代组件不需要知道被注入的 property 来自哪里

>然而，依赖注入还是有负面影响的。它将你应用程序中的组件与它们当前的组织方式耦合起来，使重构变得更加困难。同时所提供的 property 是非响应式的。这是出于设计的考虑，因为使用它们来创建一个中心化规模化的数据跟使用 $root做这件事都是不够好的。如果你想要共享的这个 property 是你的应用特有的，而不是通用化的，或者如果你想在祖先组件中更新所提供的数据，那么这意味着你可能需要换用一个像 Vuex 这样真正的状态管理方案了。


** 




## 过渡 & 动画


Vue 在插入, 更新或移除 DOM 时, 提供了多种不同方式的应用过渡效果, 包括以下工具:

  * 在 css 过渡和动画中自动应用 class
  * 可以配合使用第三方 css 动画库, 如 animate.css
  * 在过渡钩子函数中使用 js 直接操作 DOM
  * 可以配合使用第三方 js 动画库,  如 velocity.js

在这里,我们只会讲到进入 ,离开和列表的过渡, 你也可以看下一节的管理过渡状态

**单元素/组件的过渡

Vue 提供了 transition 的封装组件,在下列情形中, 可以给任何元素和组件添加进入/离开过渡
* 条件渲染(使用 v-if)
* 条件展示(使用 v-show)
* 动态组件
* 组件根节点

这里是一个典型的例子:

```js
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})

```

```js
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

```
当插入或删除包含在 transition 组件中的元素时, Vue 将会做以下处理:
1. 自动嗅探目标元素是否应用了 css 过渡或动画, 如果是, 在恰当的时机添加/删除 css 类名.
2. 如果过度组件提供了 js 钩子函数, 这些钩子函数将在恰当的时机被调用
3. 如果没有找到 js 钩子并且也没有检测到 css 过渡 /动画, DOM 操作(插入/删除)在下一帧中立即执行


过渡的类名:


## 可复用性 & 组合

混入(mixin) 提供了一种非常灵活的方式,来分发 Vue 组件中的可复用功能. 一个混入对象可以包含任意组件选项.
当组件使用混入对象时, 所有混入对象的选项讲呗



## 混入

**基础**
混入提供了一种非常灵活的方式, 来分发 Vue 组件中的可复用功能. 一个混入对象可以包含任意组件选项. 当组件使用混入对象时, 所有混入对象的选项将被"混合"进入该组件本身的选项.
例如:
```js
 var myMixin = {
  created:function(){
    this.hello()
  },
  methods:{
    hello:function(){
      console.log("hello from mixin!")
    }
  }
 }
//定义一个使用混入对象的组件

var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"

```


**选项合并**

当组件和混入对象含有同名选项时,这些选项将以适当的方式进行"合并"
比如,数据对象在内部会进行递归合并, 并在发生冲突时以数组数据优先.

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})

```
同名钩子函数将合并为一个数组,因此都将被调用. 另外, 混入对象的钩子将在组件自身钩子之前调用.


```js
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"

```
什么时机使用混入呢 ?

值为对象的选项, 例如 methods, components 和 directives, 将被合并为同一个对象, 两个对象键名冲突时,取组件对象的键值对.


```js

var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}


var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

注意: Vue.extend() 也使用同样的策略进行合并.

**全局混入**

混入也可以进行全局注册. 使用时格外小心! 一旦使用全局混入, 它将影响每一个之后闯将的 Vue 实例. 使用恰当时, 这可以用为自定义选项注入处理逻辑.

```js
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"

```


**自定义选项合并策略**


自定义选项将使用默认策略, 即简单地覆盖已有值. 如果想让自定义选项以自定义逻辑合并, 可以向 Vue.config.optionMergesStrategies 添加一个函数:

```js
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // 返回合并后的值
}

```

对于多数值为对象的选项, 可以使用与 methods 相同的合并策略:


## 自定义指令

**简介**

除了核心功能默认内置的指令(v-model 和 v-show), Vue 也允许注册自定义指令. 注意, 在Vue2.0 中,代码复用和抽象的主要形式是组件,然而,有的情况下, 你仍然需要对普通 DOM 元素进行底层操作,这时候会用到自定义指令,举个聚焦输入框的例子,如下


当页面加载时,该元素将获取焦点(注意: autofocus 在移动版 safari 上不工作). 事实上, 只要你在打开这个页面后还没点击过任何内容, 这个输入框就应当还是处于聚焦状态.现在让我们用指令来实现这个功能

```js
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

```

如果想注册局部指令, 组件中也接受一个 directives 的选项


```js
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}

```

然后你可以在模板中任何元素上使用新的 v-focus property，如下：

<input v-focus>

**钩子函数** 

一个指令定义对象可以提供如何几个钩子函数(均为可选)

* bind: 只调用一次, 指令第一次绑定到元素时调用, 在这里可以进行一次性的初始化设置
* inserted: 被绑定元素插入父节点时调用(仅保证父节点存在, 但不一定已被插入文档中)
* udpate: 所在组件的 vnode 更新时调用, 但是可能发生在其子 vnode 更新之前,指令的值可能发生改变, 也可能没有. 但是你可以通过比较更新前后的值来忽略不必要的模版更新



## 渲染函数 & jsx

**基础** 

Vue 推荐在绝大多数情况下使用 模版来创建你的 HTML, 然而在一些场景中, 你真的需要 js 的完全编程的能力, 这时你可以用渲染函数, 它比模版更接近编译器

```js
<h1>
  <a name="hello-world" href="#hello-world">
    Hello world!
  </a>
</h1>
```

对于上面的 HTML，你决定这样定义组件接口：

```js
<anchored-heading :level="1">Hello world!</anchored-heading>

```

当开始写一个只能通过 level prop 动态生成标题(heading)的组件时, 你可能很快想到这样实现


```js
<script type ="text/x-template" id ="anchored-heading-template">
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</script>
```

这里用模板并不是最好的选择, 不但代码冗长, 而且在每一个级别的标题中重复书写了 slot,在要插入锚点元素时还要再次重复.

虽然模版在大多数情况中都非常好用, 但是显然在这里它就不适合了, 那么, 我们来尝试使用 render 函数重写上面的例子:

```js
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})

```

看起来简单多了! 这样代码精简很多,但是需要非常熟悉 Vue 的实例 property. 在这里例子中, 你需要知道, 向组件中传递不带 v-slot 指令的子节点时, 比如 anchored-heading中的 Hello world, 这些子节点被存储在组件实例中 $slots.default 中.


**节点, 树以及虚拟 DOM**

在深入渲染函数之前, 了解一些浏览器的工作原理是很重要的.以下面这段 HTML 为例:

```js
 <div>
  <h1>My title</h1>
  Some text content
  <!-- TODO: Add tagline -->
</div>

```
当浏览器读到这些代码时，它会建立一个“DOM 节点”树来保持追踪所有内容，如同你会画一张家谱树来追踪家庭成员的发展一样。

上述 HTML 对应的 DOM 节点数如下图所示:

每个元素都是一个节点。每段文字也是一个节点。甚至注释也都是节点。一个节点就是页面的一个部分。就像家谱树一样，每个节点都可以有孩子节点 (也就是说每个部分可以包含其它的一些部分)。

高效地更新所有这些节点会是比较困难的，不过所幸你不必手动完成这个工作。你只需要告诉 Vue 你希望页面上的 HTML 是什么，这可以是在一个模板里：

```js
<h1>{{ blogTitle }}</h1>
```
或者一个渲染函数里


```js

  render:function (createElement) {
      return createElement('h1', this.blogTitle)

  }
```

在这两种情况下, Vue 都会自动保持页面的更新, 即便 blogTitle 发生了改变

**虚拟 DOM**

Vue 通过建立一个虚拟 DOM 来追踪自己要如何改变真实 DOM, 请仔细看这行代码

```js
return createElement('h1', this.blogTitle)

```

createElement 到底会返回什么呢? 其实不是一个实际的 DOM 元素. 它更准备的名字可能是 createNodeDescription. 因为它所包含的信息回告诉 Vue 页面上需要渲染什么样的节点, 包括及其子节点的描述信息. 我们吧这样的节点描述为"虚拟节点", 也常简写它为 "VNode". "虚拟 DOM" 是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼.

**createElement 参数**

接下来你需要熟悉的是如何在 createElement 函数中使用模版中的哪些功能. 这里是 createElement 接受的参数:

```js

//  这一块不是很明白
@returns {VNode}

createElement(
  //{String | Object | Function}
  'div',
  //{Object}
  // 一个与模板中 attribute 对应的数据对象. 可选
  {
    // (详情见一下节)
  }

  //{String | Array}
  // 子级虚拟节点 (VNodes),由 `createElement()`构建而成
  // 也可以使用字符串来生成"文本虚拟节点", 可选.
  [
    '先写一些文字',
    createElement('h1','一则头条'),
    createElement(MyComponent,{
       props:{
        someProp:'foobar'
       }
    })
  ]
)

```

**深入数据对象**

有一点要注意: 正如 v-bind:class 和 v-bind:style 在模板语法中会被特别对待一样, 它们在 VNode 数据对象中也有对应的顶层字段, 该对象也允许你绑定普通的 HTML attribute, 也允许绑定如 innerHTML 这样的 DOM property (这会覆盖 v-html指令)


```js

{
  // 与`v-bind:class` 的 API 相同
  //  接受一个字符串, 对象或字符串和对象组成的数组
  `class`:{
    foo:true,
    bar:false
  },
  // 与 `v-bind:style` 的 API 相同,
  // 接受一个字符串, 对象, 或者对象组成的数组
  style:{
    color:'red',
    fontSize:'14px'
  },
  //普通的 HTML attribute

  attrs:{
    id:'foo'
  },
  // 组件 prop
  props:{
    myProp:'bar'
  },
  domProps:{
    innerHTML:'baz'
  }
  // 事件监听器在 `on`内,
  // 但不再支持如 'v-on:keyup.enter' 这样的修饰器
  // 需要在处理函数中手动检查 keyCode
  on:{
    click:this.clickHandler
  },
  nativeOn:{
    click:this.nativeClickhandler
  }
  // 自定义指令. 注意,你无法对 'binding'中的 'oldValue'
  //赋值,因为 Vue 已经自动为你进行了同步
  directives:[{
    name:'my-custom-directive',
    value:'2',
    expression:'1+1',
    arg:'foo',
    modifiers:{
      bar:true
    }
  }],
  scopedSlots:{
    default:props => createElement('span', props.text)
  }
// 如果组件是其它组件的子组件, 需要为插槽指定名称

slot:'name-of-slot',
// 其它特殊顶层 property
key:'myKey',
ref:'myRef',
// 如果你在渲染函数中给多个元素都应用了相同的 Ref 名,
// nam `$refs.myRef` 就变成了一个数组

RefInFor:true
},

```


**约束**


## 内在  考点之一:

**深入响应式原理**

Vue 最独特的特性之一, 是其非侵入性的响应式系统,数据模型仅仅是普通的 js 对象. 而当你修改它们时, 视图会进行更新, 这使得状态管理非常简单直接, 不过理解其工作原理同样重要,这样可以跪地一些常见的问题.

1. 如何追踪变化

当你把一个普通的 js 对象传入 Vue 实例作为 data 选项, Vue 将遍历此对象所有的 property, 并使用 object.defineProperty 把这些 property 全部转为 getter/setter. Object.defineProperty 把这些 property 全部转为 getter/setter Object.defineProperty 是 es5 中一个无法 shim 的特性, 这也就是 Vue 不支持 IE8 以及更低版本浏览器的原因.

这个 getter/ setter 对用户来说是不可见的. 但是在内部它们让 Vue 能够追踪依赖, 在 property 被访问和修改时通知变更.

每个组件实例都对应一个 watcher 实例, 它会在组件渲染啊的过程中把"接触"过的数据 property 记录为依赖. 之后当依赖项的 setter 触发时, 会通知 watcher,从而使它关联的组件重新渲染.


> 响应式原理自己理解
> 数据劫持和观察者模式
> 1. 数据添加 getter 和 setter 方法 
> 2. 编译模版,引用数据, new Watcher() , Watcher 放假的 Dep.target 上
> 3. 因为数据被引用 getter 方法, 可以获取到 Watcher 实例, 添加到 dep 的数组上
> 4. 数据更新调用 setter 方法, 进而通知 watcher 形成关联关系


2. 检测变化的注意事项:
  由于 js 的限制, Vue 不能检测数组和对象的变化,Vue 不能检测数组和对象的变化,尽管如何我们还是有一些办法来回避这些限制并保证它们的响应性

  * 对于对象
  Vue 无法检测 property 的添加或移除. 由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化, 所以 property 必须在 data 对象上存在才能让 Vue 将它转化为响应式. 例如
    ```js

    var vm = new Vue({
      data:{
        a:1
      }
    })

    // `vm.a` 是响应式的

    vm.b = 2
    // `vm.b` 是非响应式的
    ```
  对于已经创建的实例, Vue 不允许动态添加根级别的响应式 property. 但是,可以使用 Vue.set(vm.someObject,propertyName,value) 方法向嵌套对象添加响应式 property. 例如, 对于

  ```js
   Vue.set(vm.someObject, 'b', 2)
  ```
  有时你可能需要为已有对象赋值多个新 property. 比如使用 Object.assign()或 _extend(). 但是, 这样添加到对象上的新 property 不会触发更新. 在这种情况下, 你应该用原对象与要混合进去的对象的 property 一起创建一个新的对象

```js
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })

```

* 对于数组
   Vue 不能检测以下数组的变动:
   1. 当你利用索引直接设置一个数组项时, 例如: vm.items[indexOfItem] = vlaue
   2. 当你修改数组的长度时. 例如: vm.items.length = newLength
  举个例子


**异步更新队列**
Vue 在更新 DOM 时是异步执行的. 只要侦听到数据变化, Vue 将开启一个队列, 并缓存在同一事件循环中发生的所有数据变更. 如果同一个 watcher 被多次触发, 只会被推入到队列中移除, 这种在缓冲时去除重复数据对于避免不必要的技术和 DOM 操作是非常重要的. 然后, 在下一个的事件循环"tick"中,Vue 刷新队列并执行实际(已去重的)工作. vue 在内部对异步队列尝试使用原来的 Promise.then, MutationObserver 和 setIMmediate, 如果执行环境不支持,则会采用 setTimeout(fn,0)代替.
例如: 当你设置 vm.someData = "new Value" 该组件不会立即重新渲染, 当刷新列表时, 组件会在下一个事件循环'tick'中更新. 多数情况我们不需要关心这个过程, 但是如果你想基于更新后的 DOM 状态来做点什么,这就可能会有些棘手. 虽然 Vue.js 通常鼓励开发人员使用"数据驱动" 的方式思考, 避免直接接触 DOM, 但是有时候我们必须要这么做, 为了在数据变化之后等待 Vue 完成更新 DOM,可以在数据变化之后立即使用 Vue.nextTick(callback). 这样回调函数将在 DOM 更新完成后被调用. 例如

```js
  <div id="example">{{message}}</div>
```

```js
 var  vm = new Vue({
  el:'#example',
  data:{
    message:'123'
  }
 })
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```
在组件内使用 vm.$nextTick() 实例方法特别方便, 因为它不需要全局 Vue, 并且回调函数中的 this 将自动绑定到当前的 Vue 实例上:

```js
Vue.component('example',{
  template: '<span>{{ message }}</span>',
  data:function(){
    return{
      message:'未更新'
    }
  },
  methods:{
    updateMessage:function(){
      this.message ="以更新"
      console.log(this.$el.textContent) // =>'未更新'
      this.$nextTick(function(){
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})

```

因为 $nextTick() 返回一个 Promise 对象, 所以你可以使用新 的 async /await 语法完成相同的事件

```js
methods: {
  updateMessage: async function () {
    this.message = '已更新'
    console.log(this.$el.textContent) // => '未更新'
    await this.$nextTick()
    console.log(this.$el.textContent) // => '已更新'
  }
}

```

**检测变化的注意事项**

由于 js 的限制, Vue 不能检查数组和对象的变化,



**异步更新队列**








# api

**el**
* 类型 string| ELement
* 限制: 只在用 new 创建实例时有效
详细:
提供一个在页面上已存在的 DOM 元素作为 Vue实例的挂载目标(挂载的意思就是 Vue 实例与页面发生关系),可以是 css 选择器, 也可以是一个 HTMLElement 实例



面试题:



**什么是生命周期**

每个 Vue 实例在被创建时都要经过一系列的初始化过程--例如,需要设置数据监听,编译模版, 将实例挂载到 DOM 并在数据变化时更新 DOM 等, 同时在这个过程中也会运行一些叫做神功周期钩子的函数, 这给了用户在不同阶段添加自己的代码的机会.

Vue中生命周期, 一共分为四大类, 分别是实例期,挂载期, 更新期, 销毁期, 每个周期飞卫当前周期前后. 其实简单点来说生命周期跟我们人的一生也挺相似的. 

我们人一生分为出生前(实例期)，青年期(挂载期),中老年期(更新期)，死亡(销毁期)，下面见生命周期图解

**生命周期图解**
1. 实例期
 实例创建前(beforeCreate)
   * 组件实例刚刚被创建, 组件属性还未创建
   * 在实例初始化之后, 数据观测(data observe) 和 event/ watcher 事件配置之前调用
 实例创建后(Created)
   组件实例创建完成, 属性已经绑定, 但是挂载阶段还没开始,  DOM 还未生成, $el 属性目前不可见

2. 挂载期:
  挂载前(beforeMount):
  * 在挂载开始之前被调用, 相关的 render 函数首次被调用, 该钩子在服务端渲染期间不被调用
  * 挂载后(mounted)
    + el 被创建的 vm.$el 替换, 并挂载到到实例上去之后调用该钩子
    + 如果root实例挂在了一个文档内元素,当mounted被调用时,vm.$el也在文档内
    + 该钩子在服务器端渲染期间不被调用
3. 更新期:
  更新前(beforeUpdate):
     * 数据更新时调用,发生在虚拟 DOM 打补丁之前. 这里适合在更新之前访问现有的 DOM, 比如手动移除已添加的事件监听器
     * 数据更新时调用,发生在虚拟 DOM 重选渲染和打补丁之前
     * 可以在这个钩子中进一步的更改状态,这不会触发附加的重新渲染过程.
  更新后(updated)
    * 由于数据更改导致的虚拟 DOM 重新渲染和打补丁,在这之后会调用该钩子
    * 当这个钩子被调用时, 组件 DOM 已经更新,所以你现在可以执行依赖于 DOM 的操作
    * 该钩子在服务器端渲染期间不被调用
4. 销毁期
   销毁期(beforeDestory)
    * 实例销毁之前调用,在这一步, 实例仍然完全可用
    * 该钩子在服务器渲染期间不被调用
  销毁后(Destory)
    * Vue 实例销毁后调用,调用后, Vue 实例指定的所有东西都会解除绑定, 所有的事件监听器都会被移除, 所有的子实例也会被销毁
    * 该钩子在服务端渲染期间不会被调用



**vue 中 render 函数做了什么?**









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


### Vue是如何单向数据流的


每个子组件都是一个单独的单个, 组件的树形网络关系, 是解析时确定的  比如 parent ,children 





