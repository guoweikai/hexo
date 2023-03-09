---
title: react面试题
date: 2023-03-09 11:13:01
tags:
---

## 组件基础:

### 1. React 事件机制( 和原生事件区别)

```js
    <div onClick={this.handleClick.bind(this)}></div>
```
React 并不是将 click 事件绑定到 div 的真实 DOM 上, 而是在 document 处监听了所有的事件, 当事件发生并且冒泡到 document 处的时候, React 将事件内容封装并交由真正的处理函数运行, 这样的方式不仅仅减少了内存的消耗, 还能在组件挂在销毁时统一订阅和移除事件

除此之外, 冒泡到 document 上的 事件也不是原生的浏览器事件, 而是由 react 自己实现的合成事件,因此 如果不想要是事件冒泡的话应该调用 e.preventDefault() 方法,而不是调用 event.stopProppagtion()方法


### 2. 



### 3 react 组件中怎么做事件代理? 它的原理是什么?

React 基于 virtual DOM 实现了一个 合成事件层, 定义的事件处理器会接收到一个合成事件对象的实例, 它符合 w3c标准, 且与原生的浏览器事件拥有相同的接口, 支持冒泡机制,所有的事件都自动绑定在最外层上

在 React 底层, 主要对合成事件做了两件事
* 事件委派: React 会把所有的事件绑定到结构的最外层, 使用统一的事件监听器, 这个事件监听器上维持了一个映射来保存所有组件内部事件监听和处理函数.
* 自动绑定: React 组件中, 每个方法的上下文都会指向该组件的实例, 即自动绑定 this 为当前组件
  
### 4 React 高阶组件, Render props ,hooks 有什么区别, 为什么要不断迭代

这三者是目前 react解决代码复用的主要方式

* 高阶组件(HOC) 是 React 中用于复用组件逻辑的一种高级技巧, HOC 自身不是 React API 的一部分, 它是一种基于 React 的组合特性而形成的设计模式, 具体而言,高阶组件是参数为组件, 返回值为新组件的函数
 
* render props 是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术, 更具体的说, render prop 是一个用于告知组件需要渲染什么内容的函数 prop
* 通常, render props 和高阶组件只渲染一个子节点, 让 hook 来服务这个使用场景更简单. 这两种模式仍有用武之地, (例如,一个虚拟滚动条组件或许会有一个 renderItem 属性, 或是一个可见的容器组件或许会有它自己的 DOM 结构). 但在大部分场景下, Hook 足够了, 并且能够帮忙减少嵌套

(1) HOC 官方解释:

高阶组件(HOC) 是 React 中用于复用组件逻辑的一种高级技巧,HOC 自身不是 React API 的一部分, 它是一种基于 React 的组合特性而形成的设计模式. 

简言之, HOC 是一种组件的设计模式, HOC 接受一个组件和额外的参数(如果需要),返回一个新的组件. HOC 是纯函数,

HOC 的优缺点:
* 优点: 逻辑复用,不影响被包裹组件的内部逻辑
* 缺点: hoc 传递给被包裹组件的 props 容易和被包裹后的组件重名,进而被覆盖
  
(2) Render props 的官方解释:
 "render prop" 是指一种在 React 组件之间使用一个值为函数的的 props 共享代码的简单技术

 具有 render prop 的组件接受一个返回 React 元素的函数, 将 render 的渲染逻辑注入到组件内部. 在这里, "render" 的命名可以是任何其他有效的标识符

 由此可以看到, render props 的优缺点也很明显

 * 优点: 数据共享, 代码复用,将组件内的 state 作为 props 传递给调用着, 将渲染逻辑交给调用着
 * 缺点: 无法在 return 语句外访问数据, 嵌套写法不够优雅

(3) Hooks 官方解释:

Hook 是 React 16.8 的新增特性, 它可以让你在不编写 class 的情况下使用 state 以及其他的 React 的特性, 通过自定义 hook, 可以复用代码逻辑.


### 5 对 React-Fiber 的理解

React v15 在渲染时, 会递归对比 virtual DOM 树, 找出需要变动的节点, 然后同步更新它们, 一气呵成. 这个过程期间, React 会占据浏览器资源, 这会导致用户触发的事件得不到响应, 并且会导致掉帧, 导致用户感觉卡顿


### 6 React.Component 和 React.PureComponent 的区别

PureComponent 表示一个纯组件,可以用来优化 Rect 程序, 减少 render 函数执行的次数, 从而提供组件的性能
在 React 中, 当 prop 或者 state 发生变化时, 可以通过在 shoudComponentUpdate 声明周期函数中执行 return fasle 来阻止页面的更新, 从而减少不必要的 render 执行, React.PureComponent 会自动执行 shouldComponenUpdate

不过, pureComponent 中的 shouldComponentUpdate() 进行的是浅比较,也就是说如果是引用数据类型的数据, 只会比较不是同一个地址,而不会比较这个地址里面的数据是否一致,浅比较会忽略属性或状态突变情况, 其实也就是数据引用指针没有辩护, 而数据发生变化的时候render 是不会执行的. 如果需要重新渲染,那么就需要重新开辟空间引用数据, pure


### 7. Component Element instance 之间有什么区别和联系?

* 元素: 一个元素 element 是一个普通对象,描述了对于一个 DOM 节点或者其他组件 
component ,你想让它在屏幕上呈现什么样子, 元素 element 可以在它的属性 props
中 包含其他元素. 创建一个 React 元素 element 成本很低, 元素 element 创建之后是不可变得

* 组件: 一个组件 component 可以通过多种方式声明, 可以是带有一个 render() 方法的类, 简单点也可以定义为一个函数.这两种情况下, 它都把属性 props 作为输入, 把返回的一个元素树作为输出

* 实例: 一个实例 instance 是你在所写的组件类 component class 中使用关键字 this 所指向的东西. 它还用来存储本地状态和响应声明周期事件很有用.
  
### 10 对 componentWillReceivedProps 的理解

该方法当 props 发生变化时执行,初始化 render 时不执行, 在这个回调函数里面,你可以根据属性的变化,通过调用 this.setState() 来更新你的组件状态, 旧的属性还是可以通过 this.props 来获取, 这里调用更新状态是安全的. 并不会触发额外的 render 调用

使用好处在: 在这个声明周期中, 可以在子组件的 render 函数执行前获取新的 props,从而更新子组件自己的 state. 可以将数据请求放在这里进行执行, 需要传的参数则从 componentWillReceivedProps(nextProps) 中获取,而不必将所有的请求都放在父组件中. 于是该请求只会在该组件渲染时才会发出, 从而减轻请求负担

componentWillReceivedProps 在初始化 render 的时候不会执行, 它会在 Component 接受到新的状态时被触发,一般用于父组件状态更新时子组件的重新渲染.
### 11  哪些方法会触发 React 重新渲染, 重新渲染render 会做些什么?

(1) 哪些方法会触发 react 重新渲染?
* setState () 方法被调用
   
setState 是 React 中最常用的命令, 通常情况下, 执行 setState 会触发 render. 但是这里有个点值的关注, 执行 setState 的时候不一定会重新选人, 当 setState 传入 null时, 并不会触发 render

*  父组件重新渲染

只要父组件重新渲染了, 即使传入子组件的 props 未发生变化, 那么子组件也会重新渲染,进而触发  render 

(2) 重新渲染 render 会做些什么?
 * 会对新旧 vNode 进行对比, 也就是我们所说的 Diff 算法
 * 对新旧两棵树进行一个深度优先遍历, 这样每一个节点都会有个标记, 在深度遍历的时候,每遍历到一个节点, 就把该节点和新的节点树进行对比,如果有差异就放到一个对象里边
 * 遍历差异对象, 根据差异的类型, 根据对应规则更新 vNode
  
React 的处理 render 的基本思维模式是每一次有变动就会去重新渲染整个应用,在 virtual DOM 没有出现之前, 最简单的方法就是直接调用 innerHTML . virtualDOM 厉害的地方并不是说它比直接操作 DOM 快,而是说

### 12 React 如何判断什么时候重新渲染组件

组件状态的改变可以因为 props 的改变,或者直接通过 setState 方法改变, 组件获得新的状态,然后 react 决定是否应该重新渲染组件, 只要组件的 state发生变化, React 就会对组件进行重新渲染, 这是因为 React 中的 shouldComponentUpdate 方法默认返回 true, 这就是导致每次更新都需要重新渲染的原因.

当 React 将要渲染组价时会执行 shouldComponentUpdate 方法来看它是否返回 true (组件应该更新, 也就是重新渲染).所以需要重写 shouldComponentUpdate方法让它根据情况返回 true 或者 false来告诉 React 什么时候重新渲染什么时候跳过重新渲染
### 13 React 声明组件有哪几种方法


### 14 对有状态组件和无状态组件的理解及使用场景

(1) 有状态组件
特点:
    * 是类组件
    * 有继承
    * 可以使用 this
    * 可以使用 react 的声明周期
    * 使用较多, 容易频繁触发声明周期钩子函数,影响性能
    * 内部使用 state, 维护自身状态的变化, 有状态组件根据外部组件传入的 props和自身的 state 进行渲染
使用场景:
    * 需要使用状态的
    * 需要使用状态操作组件的(无状态组件的也可以实现新版本 react hooks 也可实现)

总结: 类组件可以维护自身的状态变量,即组件的 state, 类组件还有不同的声明周期方法, 可以让开发者能够在组件的不同阶段(挂载,更新,卸载), 对组件做更多的控制. 类组件既可以充当无状态组件, 也可以充当有状态组件. 当一个类组件不需要管理自身状态时, 也可称为无状态组件

(2) 无状态组件 特点:
* 不依赖自身的状态 state
* 可以是类组件或者函数组件
* 可以完全避免使用 this 关键字 (由于使用的是箭头函数事件无需绑定)
* 有更高的性能,当不需要使用生命周期钩子时, 应该首先使用无状态函数组件
* 组件内部不维护 state, 只根据外部组件传入的 props 进行渲染的组件, 当 props 改变时. 组件重新渲染
  

使用场景:
* 组件不需要管理 state, 纯展示
优点:
* 简化代码, 专注于 Render
* 组件不需要被实例化, 无声明周期,提升性能. 输出(渲染)只取决于输入(属性), 无副作用
* 视图和数据的解耦分离
  
缺点:
* 无法使用 ref
* 无声明周期方法
* 无法控制组件的重渲染,因为无法使用 shouldComponentUpdate 方法, 当组件接受到新的属性时则会重渲染
  
总结: 组件内部状态且与外部无关的组件. 可以考虑用状态组件, 这样状态树就不会过于复杂, 易于理解和管理,
当一个组件不需要管理自身状态时, 也就是无状态组件, 应该优先设计为函数组件, 比如自定义的 <Button/>
<Input/> 等组件



### 15 对 React 中  Fragment 的理解, 它的使用场景是什么?
 在 React 中, 组件返回的元素只有一个根元素,为了不添加多余的 DOM 节点,我么可以使用 Fragment 标签来包裹所有的元素, Fragment 标签不会渲染出任何元素, React 官方对 Fragment 的官方解释:
     React 中的一个常见模式是一个组件返回多个元素, Fragment 允许你将子列表分组, 而无需向 DOM 添加额外节点

### 16 React 如何获取组件对应的 DOM 元素?



### 17 React 中可以再 redner 访问 refs 吗? 为什么?


不可以, render 阶段 DOM 还没有生成,无法获取 DOM, DOM 的获取需要在 pre-commmit 阶段和 commit 阶段


### 18 对 React 的插槽(portals) 的理解, 如何使用, 有哪些使用场景

React 官方对 Portals 的定义:

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案

Portals 是 React 16 提供的官方解决方案,使得组件可以脱离父组件层级


## 2. 数据管理

### 1. React setState 调用的原理


## 3. 组件通信