---
title: vue-router
date: 2022-12-26 11:37:17
tags:
---


# 基础
## 入门
1. 当加入 vue router 时, 我们需要做的就是将我们的组件映射到路由上, 让 Vue Router 知道在哪里渲染它们, 

## 动态路由匹配

1. 我们经常需要把某种模式匹配到的所有路由, 全都映射到同个组件. 例如, 我们有一个 User 组件, 对于所有 ID 各不相同的用户, 都要使用这个组件来渲染, 那么, 我们可以在 vue-router 的路由路径中使用"动态路径参数", 来达到这个效果

2. 一个 路径参数使用冒号: 标记, 当匹配到一个路由时, 参数值会被设置到 this.$route.params, 可以在每个组件内使用. 于是, 我们可以更新 User 的模板, 输入当前用户的 ID

3. 还可以设置多端"路径参数"

## 响应路由参数的变化
 当使用 路由参数时, 例如从 /user/foo 导航到 /user/bar 原来的组件实例会被复用, 因为两个路由都渲染同个组件, 比起销毁再创建, 复用则显得更高效, 不过, 这也意味着组件的声明周期钩子不会再被调用

复用组件时, 想对路由参数的变化作出响应的话, 可以简单的 watch $route 对象或者使用导航守卫 beforeRouteUpdate(to,from,next)


## 捕获所有路由或 404 Not found 路由

常规参数只会匹配被 / 分隔的 URL 片段中的字符. 如果想匹配任意路径 , 我们可以使用通配符(*)

404 错误, 也就是说 含有通配符的路由应该放到最后.


## 高级匹配模式

可以参考  path-to-regexp

## 匹配优先级

有时候, 同一个路径可以匹配多个路由, 此时,匹配的有划线就按照路由的定义顺序: 路由定义的越早, 优先级就越高

## 嵌套路由

实际生活中的应用界面, 通常由多层嵌套的组件组合而成, 同样地, URL 中各段动态路径也按照某种结构对应嵌套的各层组件

要注意, 以 / 开头的嵌套路径会被当做跟路径, 这让你充分的使用嵌套组件而无须设置嵌套的路径





## 编程式的导航

除了使用 <router-link> 组件创建 a 标签来定义导航链接, 我们还可以借助 router 的实例方法, 

router.push(location, onComplete?, onAbort?)

注意: 在 vue 实例内部, 你可以通过 $router 访问路由实例. 因此你可以调用 this.$router.push

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：

```js

// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

```

注意：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：

```js
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user

```

 在 2.2.0+，可选的在 router.push 或 router.replace 中提供 onComplete 和 onAbort 回调作为第二个和第三个参数。这些回调将会在导航成功完成 (在所有的异步钩子被解析之后) 或终止 (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的调用。在 3.1.0+，可以省略第二个和第三个参数，此时如果支持 Promise，router.push 或 router.replace 将返回一个 Promise。


注意： 如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 /users/1 -> /users/2)，你需要使用 beforeRouteUpdate 来响应这个变化 (比如抓取用户信息)。

router.replace(location, onComplete?, onAbort?)
跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录


router.go(n)
这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。


操作 History


你也许注意到 router.push、 router.replace 和 router.go 跟 window.history.pushState、 window.history.replaceState 和 window.history.go (opens new window)好像， 实际上它们确实是效仿 window.history API 的。

因此，如果你已经熟悉 Browser History APIs (opens new window)，那么在 Vue Router 中操作 history 就是超级简单的。

还有值得提及的，Vue Router 的导航方法 (push、 replace、 go) 在各类路由模式 (history、 hash 和 abstract) 下表现一致。



## 命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
要链接到一个命名路由，可以给 router-link 的 to 属性传一个对象：

```js
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

```

这跟代码调用 router.push() 是一回事：


```js
router.push({ name: 'user', params: { userId: 123 } })

```

## 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar (侧导航) 和 main (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 router-view 没有设置名字，那么默认为 default。

```js
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```


一个视图使用一个组件渲染, 因此对于同个路由, 多个视图就需要多个组件,确保正确使用 components 配置(带上 s):

嵌套命名视图



## 重定向

重定向也是通过 routes 配置来完成，下面例子是从 /a 重定向到 /b

```js
    const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})

```
重定向的目标也可以是一个命名的路由：

```js

const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})

```

甚至是一个方法，动态返回重定向目标：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})

```

注意导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。在下面这个例子中，为 /a 路由添加一个 beforeEnter 守卫并不会有任何效果。


## 别名

“重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b，那么“别名”又是什么呢？

/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。

上面对应的路由配置为：


```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})

```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。



## 路由组件传参

在组件中使用 $route 会使之与其对应路由形成高度耦合, 从而使组件只能在某些特定的 URL 上使用,限制了其灵活性

使用 props 将组件和路由解耦:

取代与 $route 的耦合

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [{ path: '/user/:id', component: User }]
})

```

通过 props 解耦


```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})

```
这样你便可以在任何地方使用该组件，使得该组件更易于重用和测试。

### 布尔模式
如果 props 被设置为 true，route.params 将会被设置为组件属性。

### 对象模式

如果 props 是一个对象，它会被按原样设置为组件属性。当 props 是静态的时候有用。

```js

const router = new VueRouter({
  routes: [
    {
      path: '/promotion/from-newsletter',
      component: Promotion,
      props: { newsletterPopup: false }
    }
  ]
})
```

### 函数模式
你可以创建一个函数返回 props。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。

```js

const router = new VueRouter({
  routes: [
    {
      path: '/search',
      component: SearchUser,
      props: route => ({ query: route.query.q })
    }
  ]
})

```


URL /search?q=vue 会将 {query: 'vue'} 作为属性传递给 SearchUser 组件。

请尽可能保持 props 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 props，请使用包装组件，这样 Vue 才可以对状态变化做出反应。



## HTML5 History 模式

vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...][label](https://flutter.cn/docs/get-started/install/)
})

```

* 一:  hash 模式

1. 定义 

hash 模式是一种把前端路由的路径用 井号拼接在真实 url 后面的模式.当井号 # 后面的路径发生变化时, 浏览器并不会重新发起请求, 而是会触发
onhashchange 事件

* hash 变化会触发网页跳转, 即浏览器的前进和后退
* hash 可以改变 url, 但是不会触发页面重新加载, 即不会刷新页面,也就是说,所有页面的跳转都是在客户端进行操作. 因此, 这并不算是一次 http 请求, 所以这种模式不利于 SEO 优化. hash 只能修改 # 后面的部分, 所以只能跳转到与当前 url 同文档的 url. 
* hash 通过 window.onhashchange 的方式, 来监听 hash 的变化,借此实现无刷新跳转的功能
* hash 永远不会提交到 server 端

* 二 history 模式

1. 定义

history API 是 H5 提供的新特性, 允许开发者直接更改前端路由, 即更新浏览器 URL 地址而不重新发起请求

2. 与 hash 的区别

我们用一个例子来演示, hash 与 history 在浏览器刷新时的区别.

具体如下

正常页面浏览

```js

https://github.com/xxx 刷新页面
https://github.com/xxx/yyy 刷新页面
https://github.com/xxx/yyy/zzz 刷新页面

```

改造 H5 history 模式

```js
https://github.com/xxx 刷新页面
https://github.com/xxx/yyy 前端跳转，不刷新页面
https://github.com/xxx/yyy/zzz 前端跳转，不刷新页面
```

3. history 的 API


| API                                    | 定义                                                                                                                                                                                                                   |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| history.pushState(data,title [, url])  | pushState 主要用于往历史记录堆栈顶部添加一条记录, 各参数解析如下: 1. data 会在 onpopstate 事件触发时作为参数传递过去; 2 title 为页面标题, 当前所有浏览器都会忽略此参数; 3 url 为页面地址, 可选, 缺少时表示为当前页地址 |
| history.replaceState(data,title[,url]) | 更改当前的历史记录,参数同上; 上面的 pushstate 是添加, 这个更改                                                                                                                                                         |
| histroy.state                          | 用于存储以上方法的 data 数据, 不同浏览器的读写权限不一样                                                                                                                                                               |
| window.onpopstate                      | 响应 pushstate 或者 replaceState的调用                                                                                                                                                                                 |
 
 4. history 的特点

对于 history 来说, 主要有以下特点

* 新的 url 可以是与当前 url 同源的任意 url, 也可以是与当前 url 一样的地址, 但是这样会导致的一个问题是, 会吧重新的这一次操作记录到栈当中
* 通过 history.state 添加任意类型的数据到记录中
* 可以额外设置 title 属性, 以便后续使用
* 通过 pushState replaceState 来实现无刷新跳转的功能
  
5. 存在问题

对于history 来说, 确实解决了不少 hash 存在的问题, 但是也带来了新的问题, 具体如下
* 使用 history 模式时, 在对当前的页面进行刷新时, 此时浏览器会重新发起请求. 如果 后端没有匹配的到当前的 url, 就会出现 404 的页面
* 而对于 hash 模式来说, 它虽然看着是改变了 url, 但不会被包括在 http 请求中. 所以, 它算是被用来指导浏览器的动作, 并不影响服务器端,因此, 改变 hash 并没有真正的改变 url, 所以页面路径还是之前的路径, nginx 也就不会拦截

6. 两者的选择
   

- - -

#进阶

## 导航守卫

什么是导航守卫?

实际应用的场景有哪些

正如其名, vue-router 提供的导航守卫主要用来跳转或取消的方式守卫导航. 这里有很多方式植入路由导航中: 全局的, 单个路由独享的, 或者组件级的.

记住参数或查询的改变并不会触发进入/离开的导航守卫, 你可以通过 观察 $route 对象来应对这些变化, 或使用

beforeRouteUpdate 的组件内守卫.

守卫的几种形式?应用场景是哪些

全局前置守卫
 你可以使用 router.beforeEach 注册一个全局前置守卫
 当一个导航触发时, 全局前置守卫按照创建顺序调用, 守卫是异步解析执行,此时导航在所有守卫 resolve 完之前一直处于等待中

 每个守卫方法接收两个参数

 * to 即将要进入的目标 用一种标准化的方式
 * from 当前导航正要离开的路由 用一种标准化的方式
 * next:Function : 一定要调用该方法来 resolve 这个钩子,执行效果依赖 next 方法的调用参数.
     * next(): 进行管道中的下一个钩子. 如果全部钩子执行完了, 则导航的状态就是 confirmed
     * next(false):中断当前的导航,如果浏览器的 url 改变了(可能是用户手动或者浏览器后退按钮),那么 url 地址会重置到 from 路由对应的地址
     * next('/')



可以返回的值如下:
 * false: 取消当前的导航, 如果浏览器的 URL 改变了(可能是用户手动或者浏览器后退按钮), 那么 URL 地址会重置到 From 路由对应的地址
 * 一个路由地址: 通过一个路由地址跳转到一个不同的地址, 就像你调用 router.push() 一样, 你可以设置诸如 replace:true 或 name:'home' 之类的配置, 当前的导航被中断, 然后进行一个新的导航,就和 from 一样

### 全局路由钩子

beforeEach(to,from ,next) , beforeResolve(to,from,next)
afterEach(to,from)

问题 全局前置守卫和全局解析守卫的区别?应用场景

### 路由独享的守卫

### 组件内的守卫


完整的导航解析流程

1. 导航被激活
2. 在失活的组件里调用 beforeRouteLeave 守卫
3. 调用全局的 beforeEach 守卫
4. 在重用的组件里调用 beforeRouteUpdate
5. 在路由配置里调用 beforeEnter
6. 解析已补路由组件
7. 在被激活的组件里调用 beforeRouteEnter
8. 调用全局的 beforeResolve 守卫
9. 导航被确认
10. 调用全局的 afterEach 钩子
11. 触发 DOM 更新
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数, 创建好的组件实例会作为回调函数的参数传入
    


## 路由元信息

什么是路由元信息?
vue-router 路由元信息说白了就是通过 

使用的场景有哪些?


## 过渡动效


## 数据获取


## 滚动行为



## 路由懒加载

当打包构建应用时, js 包会变得非常大,影响页面加载. 如果我们能把不同路由对应的组件分割成不同的代码块,然后当路由被访问的时候才加载对应组件, 这样就更加高效了

- - -
# api 解析:

## router-link props

* to

表示目标路由的连接, 当被点击后, 内部会立刻把 to 的值传到 router.push(), 所以这个值可以是一个 string 或者是描述目标位置的对象

* replace 
    设置 replace 属性的话, 当点击时, 会调用 router.replace(), 而不是 router.push(),所以导航后不会留下历史记录


* active-class
   链接激活时, 应用与渲染的 <a> 的 class


## router-view props

 如果 <router-view> 设置了名称, 则会渲染对应的路由配置中. components 下的相应组件

 Router 构建选项

 mode: 

配置路由模式:
* hash:使用 URL hash 值来作为路由,支持所有浏览器,包括不支持 HTML 5 History Api 的浏览器
  


## Router 实例属性



# vue-router 面试题


1. vue-router 是什么? 有哪些组件?

* Vue Rounter 是 Vue.js 官方的路由管理器. 它和 Vue.js 的核心深度集成. 让构建单页面应用变的易如反掌.
* rounter-linkv   rounter-view keep-alive

2. 怎么定义 vue-router 的动态路由? 怎么获取传过来的值?

* 动态路由的创建, 主要是使用 path 属性过程中, 使用动态路径参数, 以冒号开头, 如下:

```js
{
  path: '/details/:id',
  name:'Details',
  components:Details
}
```

3. vue-router 有哪几种导航钩子

* 全局

 全局前置守卫

* 路由独享的
* 组件内导航钩子

4. $route 和$router 的区别是什么?

* router 为 vueRouter 的实例, 是一个全局路由对象, 包含了路由跳转的方法, 钩子函数等.
* route 是路由信息对象|| 跳转的路由对象, 每一个路由都会有一个 route 对象, 是一个局部对象, 包含 path params hash query fullPath ,matched,name 等路由信息参数

5. vue-router 响应路由参数的变化

* 用 watch 检测 监听当前路由发生变化的时候执行
* 组件内导航钩子函数

6. vue-router 传参
 
   * Params
    1. 只能使用 name , 不能使用 path
    2. 参数不会显示在路径上
    3. 浏览器强制刷新参数会被清空
  * Query
    1. 参数会显示在路径上, 刷新不会被清空
    2. name 可以使用 path 路径

7. vue-router 的两种模式
  * hash
     原理是 onhashchange 可以在 window 对象上监听这个事件
  ```js
    window.onhashchange = function(event){
        console.log(event.oldURL, event.newURL)
  let hash = location.hash.slice(1)
    }
  ```
  * history
     利用了HTML5 History Interface 中新增的pushState()和replaceState()方法。
     需要后台配置支持。如果刷新时，服务器没有响应响应的资源，会刷出404

8. vue-router 实现路由懒加载(动态加载路由)
  把不同路由对应的组件分割成不同的代码块，然后当路由被访问时才加载对应的组件即为路由的懒加载，可以加快项目的加载速度，提高效率

  ```js
  const router = new VueRouter({
  routes: [
    {
      path: '/home',
      name: 'Home'，
      component:() = import('../views/home')
		}
  ]
})
  ```
  找出几个面试题背一下就可以了


  


  













 

























































