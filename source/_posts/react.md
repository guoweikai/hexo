---
title: react
date: 2022-09-06 14:54:29
tags:
---
### 1. 什么是React?(谁开发的, 主要作用,特点是什么)
React 是一个由 Facebook 开发和维护的 JavaScript 库，用于构建用户界面。React 的主要特点是使用组件化和虚拟 DOM 的方式来使 UI 代码更加模块化、可维护和可重用。它使用 JSX 语法来描述组件的外观和行为，并使用 JavaScript 来控制其渲染和交互。React 通常与其他技术，如 Redux、GraphQL 和 React Native 等一起使用，以构建完整的 Web 应用程序、移动应用程序和其他类型的应用程序。React 也具有活跃的社区和生态系统，提供了许多有用的工具、库和插件，以支持 React 应用程序的开发和维护。

### 2. jsx (jsx 是什么, 如何使用, 使用时注意什么,解决了什么问题)

1. jsx是什么?
JSX 是一种 JavaScript 的语法扩展，用于描述 React 组件的 UI 层。它允许开发人员在 JavaScript 代码中编写 HTML 类似的结构和样式，使得组件的编写更加直观和易于维护。在 JSX 中，开发人员可以使用类似 HTML 的标记来描述组件的结构和内容，同时也可以在标记中嵌入 JavaScript 表达式来实现动态的 UI 效果
2. jsx 会生成 ast 吗

是的，JSX 代码会被转换为 AST（抽象语法树），然后再转换为普通的 JavaScript 代码。React 编译器会将 JSX 代码解析成一个 AST，然后将 AST 转换成 JavaScript 代码。
在 React 中，使用 Babel 编译器来转换 JSX 代码。Babel 是一个用于将新版 JavaScript 转换成向后兼容的代码的工具。它可以将 JSX 代码转换成常规的 JavaScript 代码，并将其打包成浏览器可以执行的 JavaScript 文件
以下是一个使用 Babel 编译器将 JSX 代码转换为 JavaScript 代码的示例：

```js
// JSX 代码
const element = <h1>Hello, world!</h1>;
// 转换后的 JavaScript 代码
const element = React.createElement("h1", null, "Hello, world!");

```
在这个例子中，Babel 编译器将 JSX 代码转换成了 React.createElement() 方法的调用，这个方法创建了一个描述组件的 JavaScript 对象，最终被用于渲染页面。
总之，JSX 代码被转换为 AST，然后再转换为 JavaScript 代码，这些代码最终被用于渲染页面。这个过程是由 Babel 编译器和 React 编译器共同完成的。jsx 最终生成 虚拟 dom, 虚拟 dom 就是 js 的一个描述元素结构和内容的对象

3. react 为什么使用 jsx
JSX 是一种 JavaScript 语法扩展，它允许在 JavaScript 代码中直接嵌入 HTML 标记。这种语法扩展使得开发人员可以更直观地描述组件的结构和外观，同时也使得代码更易于阅读和理解。
使用 JSX 还有一些其他的好处。例如，它可以在编译时进行语法检查，避免一些常见的错误；它还可以提供更好的代码提示和自动完成功能，减少了开发人员的手动输入。
另外，使用 JSX 还可以使得 React 组件更容易进行测试和维护。由于 JSX 使得代码结构更加清晰和可读，因此开发人员可以更容易地定位和修复问题，从而提高了代码的可维护性和可测试性。
总的来说，React 使用 JSX 是为了提高开发效率和可读性，并提供更好的代码提示、自动完成、测试和维护功能。虽然 JSX 语法对一些开发人员来说可能有些陌生，但它的优点远远超过了缺点

4. jsx 的语法规则: 
 * 定义虚拟 dom ,不用写引号
 * 标签中混入 js 表达式时要用 {},表达式很重要
   一定注意区分: js 语句 于 js 表达式
   1. 表达式: 一个表达式会产生一个值,可以放在任何一个需要值的地方
    下面这些都是表达式
    (1) a
    (2) a+b
    (3) arr.amp()
    (5) function test(){

    }
   2. 语句(代码)
    下面这些都是语句(代码):
      (1)if(){}
      (2) for(){}
      (3)switch(){case:xxxx} 
 * 样式的类名的指定不要用 class, 要用 className
 * style={{color:'red'}} , 内联样式, 要用 style = {{key:value}}
 * jsx 只能有一个根标签
 * 标签必须闭合
 * 标签首字母
   (1)若小写字母开头, 则将标签转为 html 中同名元素, 若 html 中无该标签对应的同名元素, 则报错
   (2)若大写字母开发, react 就去渲染对应的组件, 若组件没有定义则报错
 * jsx 中遍历的写法 (只能是数组不能是对象)
  ```js
  <ul>
    {
      data.map((item,index)=>{
        return <li :key="index">{item}</li>
      })
    }
  </ul>
  ```
### 3. react 组件(组件是什么? 为什么拆分组件? 组件的功能有哪些, 难点有哪些和模块的区别)
1. js 模块
  定义:
  向外提供特定功能的 js 程序, 一般就是一个 js 文件
  为什么要拆分模块:
  随着业务逻辑增加,代码越来越多且复杂
2. 组件
  定义:用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
  为什么:一个界面的功能更复杂
  作用:复用编码,简化项目编程,提供运作效率
3. 组件的类型
   函数式组件,适用于简单组件的定义

    ```js
    function Demo (){
      return <h1>简单组件的定义</h1>
    }
    ReactDom.render(<Demo/>, document.getElementById('test'))
    ```
    执行了 ReactDom.render(<MyComponent/>).....之后,发生了什么?
    1. React 解析了组件标签,找到了 MyComponent 组建,
    2. 发现组件是使用函数定义的,随后调用该函数, 就将会的虚拟 DOM 转为真实 DOM,随后呈现在页面
   类式组件,使用于复杂的组件的定义
    类的复习:
    1. 类的构造器函数和普通函数
    2. 集成 
    总结: 类中的构造器不是必须写的,要对实例进行一些初始化的操作,如添加指定属性时采血
         如果 A 类继承了 B 类,且 A类中写了构造器函数, 那么 A 构造器函数中的 super 是必须要调用的
         类中所定义的方法,都是放在了类的原型对象上, 供实例去使用
     组件是继承于 react.component
3. 组件实例的三大属性
  - (组件的状态是干什么的?如何往 state 放东西 如何修改状态?)
    this.setState()  
  -  props
  -  事件, 如何添加事件
     事件名称 onClick.
     事件处理函数中处理函数.
1. 组件实例的通信有几种方式
2. 高阶组件
3. react组件的生命周期(生命周期是什么?生命周期解决了什么问题, react 的生命周期有些, 生命周期都做了什么?)
   1. 挂载阶段(Mounting): 当组件实例被创建并插入到 DOM 中时执行以下声明周期方法:
     - constructor(): 构造函数, 在组件被创建时调用. 在这里可以初始化组件的状态(state)和绑定事件处理函数(bind 方法)等
     - getDerivedStateFromProps(props,state): 静态方法, 在组件挂载前调用,用于根据传入的 props 更新组件的 state
     - render(): 渲染函数,将组件的虚拟 DOM 渲染到页面上
     - componentDidMount(): 组件挂载后执行, 用于执行一些副作用, 如发送数据请求, 添加事件监听器等
   2. 更新阶段(updating): 当组件的 props 或 state 发生变化时执行以下生命周期方法:
     - static getDerivedStateFromProps(props, state):静态方法, 在组件更新前调用,用于根据传入的 props 更新组件的 state
     - shouldComponentUpdate(nextProps,nextState): 在组件更新之前调用,用于判断组件是否需要重新渲染,返回值为 true 或 false
     - render() : 渲染函数,将组件的虚拟 DOM 渲染到页面上
     - getSnapshotBeforeUpdate(prevProps,prevState): 在组件更新前调用, 用户获取更新之前的 DOM 信息, 返回值会传递给     componentDidUpdate() 方法的第三个参数
     - componentDidUpdate(prevProps, prevState, snapshot): 在组件更新后调用，用于执行一些副作用，如更新其他组件、处理 DOM 变更等
   3. 卸载阶段(Unmounting): 当组件从 DOM 中移除时执行以下生命周期的方法:
      - componentWillUnMount(): 组件卸载前执行,用于取消事件监听器, 清理定时器等
   除了上述声明周期方法, React 还有一些其他的生命周期方法,如:
      - static getDerivedStateFromError(error): 静态方法，在组件内部发生错误时调用，用于根据错误更新组件的 state。

      - componentDidCatch(error, info): 在组件内部发生错误后调用，用于处理错误信息
### 3. react 响应原理是什么?
当组件的状态发生改变时, React 会重新渲染组件,并生成一颗新的虚拟 DOM树. React 会将新的虚拟 DOM 树与旧的虚拟 DOM 树进行比较,找出需要更新的节点, 并进行更新. 这个过程被称为调和.

React 在调和时, 采用了一些优化策略, 例如批量更新, DOMDiff 算法等. 通过这些优化, React 可以快速地更新 DOM, 提供性能

在 React 中, 状态更新是异步的. 当组件的状态发生改变时, React 会将状态的改变放入一个队列中,等到合适的时机再去更新组件. 这个过程被称为批量更新,批量更新可以避免不必要的重渲染,提供性能.

在 React 中, 通过 `setState`方法来更新组件的状态. `setState` 方法接受一个对象或函数作为参数, 用于更新组件的状态. 在更新状态时, React 会自动触发组件的重新渲染, 从而更新组件的显示

总的来说, React 的响应式原理是基于虚拟 DOM 和状态更新机制实现的. 通过虚拟 DOM 的比较和更新,React 可以高效的更新组件的显示.而通过状态更新机制. React 可以实现组件的响应式

### 4. react 如何监听数据变化

在react 中监听数据变化的方式是通过组件的状态(state) 和属性(props)来实现的.
具体来说,可以采用以下几种方式:
- setState() 方法
在 React 中, 通过调用组件的 setState()方法来更新组件的状态, 当调用 setState() 方法时, React 会自动重新渲染组件, 并将更新后的状态传递给组件. 在 setState() 方法中,可以使用回调函数来监听数据的变化,例如:

```js
this.setState({ count: this.state.count + 1 }, () => {
  console.log('count has been updated: ', this.state.count);
})
```

- componentWillReceiveProps() 生命周期函数
在React中, 组件的属性(props) 发生变化时,会触发 componentWillReceiveProps() 生命周期函数. 可以在这个函数中监听数据的变化, 例如:
```js
componentWillReceiveProps(nextProps) {
  if (nextProps.data !== this.props.data) {
    console.log('data has been updated: ', nextProps.data);
  }
}
```

- shouldComponentUpdate() 生命周期函数
在 React 中, 可以通过 shouldComponentUpdate() 生命周期函数来判断组件是否需要重新渲染. 可以在这个函数中监听数据的变化,例如

```js
shouldComponentUpdate(nextProps, nextState) {
  if (nextState.count !== this.state.count) {
    console.log('count has been updated: ', nextState.count);
  }
  return true;
}
```
 

### 4. vue 和 react 中父子组件传递有哪些不同?

在 Vue 中, 父组件向子组件传递数据通常使用 `props`, 即将数据以属性的形式传递给子组件. 子组件通过 `this.$props` 和`this.$attrs` 来获取父组件传递过来的数据. 另外, Vue 中也可以使用 `provide` 和`inject` 来进行跨级组件的数据传递.

在 React 中, 父组件向子组件传递数据也可以使用`props`, 即将数据以属性的形式传递给子组件. 子组件通过 `props` 来获取父组件传递过来的数据. 另外, 在React 中也可以使用  `context`来进行跨级组件的数据传递

Vue 和 React 在父子组件传递数据的方式上有些不同，但是它们的思想是一致的，都是通过组件之间的数据传递来实现组件之间的通信。需要注意的是，在 Vue 中，父组件向子组件传递数据时，数据是单向流动的，即父组件可以向子组件传递数据，但子组件不能直接修改父组件的数据，只能通过 $emit 方法向父组件发送事件来触发父组件的方法从而改变数据。而在 React 中，由于所有的组件都是可变的，因此可以通过回调函数或 context 来实现子组件修改父组件的数据(这个不一定全对)

### 5.React组件如何绑定事件, 和 vue 以及原生事件的不同?

React 的事件机制是基于合成事件实现的.合成事件是 React封装的跨浏览器的事件系统, 它在底层使用了事件委托和事件池来优化性能

事件委托是一种常见的优化手段, 它可以将事件处理函数绑定到容器元素上, 然后在容器元素内部捕获所有事件, 通过判断事件来源来执行相应的事件处理函数. 这个可以避免在每个子元素都绑定事件处理函数. 从而减少内存占用和事件注册时间


### 6.在 React 中使用 原生事件不能冒泡 React 事件的原因

在 React 中使用原生事件无法冒泡到 React 组件的事件处理程序，是因为 React 使用了一种叫做“合成事件”的技术来处理事件。合成事件是 React 提供的一种事件系统，它封装了原生事件，并提供了一些额外的功能，比如自动绑定 this、跨浏览器兼容性等。

React 为了提高性能，会在组件的根节点上绑定一些原生事件，例如 onClick、onKeyDown 等。当这些事件被触发时，React 会生成一个合成事件，并调用与之相关联的组件的事件处理程序。合成事件不会冒泡到 DOM 树上，而是通过事件委派的方式交由 React 进行处理。

因此，如果在 React 组件中使用原生事件来处理事件，该事件不会被传递给 React 的事件处理程序。这意味着，无法通过原生事件来触发 React 组件的状态更新、重渲染等操作，也无法与其他 React 组件共享状态。如果需要在 React 组件中使用原生事件，可以通过手动触发 React 组件的事件处理程序来实现

### 7. react 中对 jsx 的理解
jsx 是 React 中一种用于描述用户界面的语法, 它允许我们使用类似于 HTML 的标记来描述 ui 组件,并且可以在其中迁入 js 代码, 🙆🏻‍♀️组件的逻辑和渲染可以更加灵活,

### 8 . react 中的 React.createElement 都做了什么



### 9.ReactDom.render 做了什么呢?
ReactDOM.render() 是 React 中的一个核心方法，用于将 React 元素渲染到 DOM 中。
它的主要作用是将 React 元素转换为真实的 DOM 节点, 并将其插入到指定的容器中,其实就是将虚拟 dom 转化为真实 dom 并且上树的过程

### React 的渲染流程可以分为两个阶段: 初始化阶段和更新阶段
1. 初始化阶段:
   - 创建组件: 当 React 遇到组件标签时, 会创建对应的组件实例, 并将组件的 props 传递给组件实例.
   - 解析组件: React 会通过组件实例的 render 方法生成一个 virtual DOM 树(即虚拟 Dom树), 该树包含了组件的结构和状态.
   - 构建 DOM 树: React 会将 虚拟 DOM 树转换为真实的 DOM 树, 并插入到页面中.
   - 注册事件: React 会注册组件中的事件监听器, 并绑定事件处理函数
2. 更新阶段:
   - 更新状态：当组件的状态发生变化时，React 会重新调用组件的 render 方法生成一个新的 Virtual DOM 树。
   - Diff 算法：React 会使用 Diff 算法比较新旧 Virtual DOM 树的差异，并更新需要更新的部分。
   - 执行生命周期钩子函数：如果组件的状态发生变化，React 会依次执行组件的生命周期钩子函数，从而触发组件的副作用
   - React 的渲染流程采用了 Virtual DOM 技术，将组件的状态和视图分离开来，并通过 Diff 算法优化了组件的更新效率，从而提高了应用的性能和用户体验


### 10. React  中 嵌套的组件虚拟 dom 是如何关联的呢

在 React 中, 每个组件都有自己的虚拟 Dom(也称为"React 元素"), 包括嵌套的组件, 当一个组件嵌套在另外一个组件中时,React 将两个组件的虚拟 Dom 关联在一起, 形成一个树形结构.

具体来说，当一个组件渲染时，它会返回一个虚拟 DOM 树。这个虚拟 DOM 树包含了这个组件及其子组件的所有虚拟 DOM 节点。当这个组件被渲染到页面上时，React 会遍历这个虚拟 DOM 树，并将其转换成实际的 DOM 节点，然后将这些实际的 DOM 节点添加到页面中。

在这个过程中，当 React 遍历到一个嵌套的组件时，它会将这个组件的虚拟 DOM 树插入到父组件的虚拟 DOM 树中，作为父组件的一个子节点。这样，就形成了一个组件树，每个组件都是另一个组件的子节点。

在组件树中，每个组件的虚拟 DOM 树都是相互独立的，但它们之间存在父子关系，这个关系是通过 React 在渲染过程中自动构建和维护的。这也是 React 能够高效地更新页面的关键之一，因为它只会重新渲染发生变化的部分，而不是整个页面。

### 10 React 组件和虚拟 dom 之间的关系
React 组件和虚拟 DOM 之间是紧密相关的，可以说虚拟 DOM 是 React 组件工作的基础。下面分别介绍一下它们之间的关系
1. React 组件
   React 组件是 React 应用的核心概念，它是由若干个 React 元素组合而成的。每个组件都有自己的状态（state）和属性（props），以及生命周期方法。在组件中，我们可以通过修改状态或属性来控制组件的行为和渲染输出

2. 虚拟 Dom
   虚拟 DOM 是 React 中用于优化渲染性能的一种技术，它是 React 组件的一种抽象表示。虚拟 DOM 是一个轻量级的 JavaScript 对象，它描述了组件的结构和属性，但并不是真实的 DOM 节点。

  虚拟 DOM 通过比较前后两个版本的虚拟 DOM 树，找出差异并只对差异部分进行更新，从而避免了直接操作真实 DOM 的开销，提高了渲染性能。
3. 组件和虚拟 DOM 之间的关系
   React 组件通过 render 方法返回一个描述组件结构和属性的虚拟 DOM。当组件状态或属性发生变化时，React 会比较前后两个版本的虚拟 DOM 树，找出需要更新的部分，并仅仅更新这部分差异，从而实现组件的高效渲染。

   具体来说，当 React 组件状态或属性发生变化时，React 会调用组件的 render 方法生成新的虚拟 DOM 树。然后，React 会比较前后两个版本的虚拟 DOM 树，找出需要更新的部分，生成更新操作并应用到真实 DOM 上。

   因此，可以说 React 组件和虚拟 DOM 是紧密相关的，虚拟 DOM 是 React 组件工作的基础，通过它实现了高效的渲染和更新。


### 11. React jsx 和 Vue 的模版的区别



### 12. 有状态组件和 无状态组件
自定义 Button 为什么里边可以写文字

### 组件为什么可以嵌套另外一个组件
因为在组件中 有 this.props.children,可以将两个括号的内容传递到内部组件,在 React 中


### 组件传递过去的数据类型
* 1. jsx 可以是一个组件   
* 2. 函数, 可以在组件内部调用, 传值出来
* 3. 其他正常数据类型
* 4. className 传递样式进去
这些都是因为 有 props

### React.create 后给到了谁?

### react hook? 



### react 的全家桶?

### React 和 Vue 渲染流程的区别?
