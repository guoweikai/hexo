---
title: vuex
date: 2023-02-08 11:11:29
tags:
---

## 核心

### State
**单一状态树** 
vuex 使用单一状态树,用一个对象就包含了全部的应用层级状态, 至此它便作为一个唯一数据源而存在,这也意味着, 每个应用将仅仅包含一个 store 实例, 单一状态树让我们能够直接地定位任一特定的状态片段, 在调试的过程中 也就能轻易地取得整个当前应用状态的快照
单状态树和模块化并不冲突
存储在 Vuex 中的数据和 vue 实例中的 data 遵循相同的规则, 例如状态对象必须是纯粹的
**在 Vue 组件中获得 Vuex 状态**
那么我们如何在 Vue 组件中展示状态呢? 由于 Vuex 的状态存储是响应式的. 从 store 实例中读取状态最剪短的方法就是在 计算属性中返回某个状态:
```js
const Counter = {
    template:'<div>{{ count}}</div>',
    computed:{
        count(){
            return store.state.count
        }
    }
}
```
每当 store.state.count 变化的时候,都会重新求取计算属性, 并且触发更新相关联的 DOM

然而, 这种模式导致组件依赖全局状态单例. 在模块化的构建系统中, 在每个需要使用 state 的组件中需要频繁地导入, 并且在测试组件时需要模拟状态

Vuex 通过 Vue 的插件系统将 store 实例从根组件中“注入”到所有的子组件里。且子组件能通过 this.$store 访问到。让我们更新下 Counter 的实现：

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```
mapState 辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。


```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

**对象展开运算符**

mapState 函数返回的是一个对象。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 computed 属性。但是自从有了对象展开运算符，我们可以极大地简化写法：


```js
computed:{
      localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```
**组件仍然保有局部状态**
使用 Vuex 并不意味着你需要将所有的状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。


### Getter 

有时候我们需要从 store 中的 state 中派生出一些状态,例如对列表进行过滤并计数

```js
computed:{
    doneTodosCount(){
        return this.$store.state.todos.filter((todo)=>todo.done).length
    }
}
```
如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Getter 接受 state 作为其第一个参数：

```js
const store = createStore({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos (state) {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

**通过属性访问**
Getter 会暴露为 store.getters 对象，你可以以属性的形式访问这些值：
```js
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```
Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  doneTodosCount (state, getters) {
    return getters.doneTodos.length
  }
}
```

```js
store.getters.doneTodosCount // -> 1

```
我们可以很容易地在任何组件中使用它：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```
注意，getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。

**通过方法访问**
你也可以通过 让 getter 返回一个函数, 来实现 getter 传参, 在你对 store 里的数组进行查询时非常有用
```js
getters:{
    //...
    getToDoById:(state)=>(id)=>{
        return state.todos.find(todo=> todo.id ===id)
    }
}
```
```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

**mapGetter 辅助函数**
### Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation. Vuex 中的 mutation 非常类似于事件:每个 mutation 都有一个字符串的事件类型和一个回调函数,这个回调函数就是我们实际进行状态更改的地方, 并且它会接受 state 作为第一个参数:



Vuex 面试题汇总

