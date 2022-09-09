---
title: react1
date: 2022-09-06 16:48:09
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


### 7. Component Element instance zhi'jian'you

### 哪些方法会触发 React 重新渲染, 重新渲染render 会做些什么?

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



### 17 React 中可以再 redner 访问 refs 吗? 为什么?


不可以, render 阶段 DOM 还没有生成,无法获取 DOM, DOM 的获取需要在 pre-commmit 阶段和 commit 阶段


### 18 对 React 的插槽(portals) 的理解, 如何使用, 有哪些使用场景

React 官方对 Portals 的定义:

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案

Portals 是 React 16 提供的官方解决方案,使得组件可以脱离父组件层级


## 2. 数据管理

### 1. React setState 调用的原理


## 3. 组件通信





