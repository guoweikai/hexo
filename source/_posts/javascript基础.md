---
title: javascript基础
date: 2022-03-10 11:20:34
tags:
---

### 1. new 操作符的实现原理
* 首先创建一个空对象
* 设置原型，将对象的原型设置为函数的 prototype对象
* 让函数的this 指向这个对象，执行构造函数的代码
* 判断函数的返回值类型，如果是值类型，返回创建的对象，如果是引用类型，就返回这个引用类型的对象(这个地方有疑惑)

具体实现

```js
    function ojbectFactory(){
        let newObject = null;
        let constructor = Array.prototype.shift.call(arguments);
        let result = null;
        if(typeof constructor ! == "function"){
            console.error("type error");
            return
        }
        newObject = Object.create(constructor.prototype);
        result = constructor.apply(newObject,arguments);
          // 判断返回对象
  let flag = result && (typeof result === "object" || typeof result === "function");
  // 判断返回结果
  return flag ? result : newObject;
    }
    // 使用方法
objectFactory(构造函数, 初始化参数);

```

### 2 map 和 Object 的区别

|          | Map                                                                        | Object                                                                      |
| -------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| 意外的健 | Map默认情况不包含任何健，只包含显示插入的健                                | Object 有一个原型, 原型链上的键名有可能和自己在对象上的设置的键名产生冲突。 |
| 键的类型 | Map的键可以是任意值，包括函数、对象或任意基本类型。                        | Object 的键必须是 String 或是Symbol。                                       |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候， Map 对象以插入的顺序返回键值。 | Object 的键是无序的                                                         |
| Size     | Map 的键值对个数可以轻易地通过size 属性获取                                | Object 的键值对个数只能手动计算                                             |
| 迭代     | Map 的键值对个数可以轻易地通过size 属性获取                                | Object 的键值对个数只能手动计算                                             |

### 3 map和weakMap的区别
(1) Map map 本质上就是键值对的集合, 但是普通的 Object 的键值对中的键只能是字符串,而 es6 提供的 map 数据结构类似于对象, 但是它的键不限制范围, 可以是任意类型,
是一种更加完美的 hash 结构,如果 map 的键是一个原始数据类型,只要两个键严格相同,就视为是同一个键.

实际上 map 是一个数组, 它的每一个数据也都是一个数组, 其形式如下

```js
    const map =[
        ["name","张三"],
        ["age",18]
    ]
```
map 数据结构有以下操作方法

> size map.size 返回 map 结构的成员总数
> set(key,value) 设置键名 key 对应的键值 value, 然后返回整个 map 结构, 如果 key 已经有值, 则键值会被更新, 负责就新生成该键,(因为返回的是当前 map 对象, 所以可以链式调用)
> get(key) 该方法读取 key 对应的键值,如果找不到 key , 返回 undefined
> has(key) 该方法返回一个布尔值, 表示某个键是否在当前 map 对象中
> delete(key) 该方法删除某个键, 返回 true ,如果删除失败, 返回 false
> clear() map.clear() 清楚所有成员, 

（2）WeakMap WeakMap 对象也是一组键值对的集合，其中的键是弱引用的。其键必须是对象，原始数据类型不能作为key值，而值可以是任意的
该对象也有以下几种方法：

set(key,value)：设置键名key对应的键值value，然后返回整个Map结构，如果key已经有值，则键值会被更新，否则就新生成该键。（因为返回的是当前Map对象，所以可以链式调用）
get(key)：该方法读取key对应的键值，如果找不到key，返回undefined。
has(key)：该方法返回一个布尔值，表示某个键是否在当前Map对象中。
delete(key)：该方法删除某个键，返回true，如果删除失败，返回false。
其clear()方法已经被弃用，所以可以通过创建一个空的WeakMap并替换原对象来实现清除。
WeakMap的设计目的在于，有时想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用。一旦不再需要这两个对象，就必须手动删除这个引用，否则垃圾回收机制就不会释放对象占用的内存。
而WeakMap的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
总结：

Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap 只接受对象作为键名（ null 除外），不接受其他类型的值作为键名。而且 WeakMap 的键名所指向的对象，不计入垃圾回收机制。


### 4 JavaScript有哪些内置对象

全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在 全局作用域里的对象。全局作用域中的其他对象可以由用户的脚本创建或由宿主程序提供。

标准内置对象的分类：

（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。例如 Infinity、NaN、undefined、null 字面量
（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。例如 eval()、parseFloat()、parseInt() 等
（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等
（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date
（5）字符串，用来表示和操作字符串的对象。例如 String、RegExp
（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array
（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。
例如 Map、Set、WeakMap、WeakSet
（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。
例如 SIMD 等
（9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等
（10）控制抽象对象
例如 Promise、Generator 等
（11）反射。例如 Reflect、Proxy
（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等
（13）WebAssembly
（14）其他。例如 arguments

总结：
js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般经常用到的如全局变量值 NaN、undefined，全局函数如 parseInt()、parseFloat() 用来实例化对象的构造函数如 Date、Object 等，还有提供数学计算的单体内置对象如 Math 对象


### 5. 常用的正则表达式有哪些?
```js
// （1）匹配 16 进制颜色值
var regex = /#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})/g;

// （2）匹配日期，如 yyyy-mm-dd 格式
var regex = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;

// （3）匹配 qq 号
var regex = /^[1-9][0-9]{4,10}$/g;

// （4）手机号码正则
var regex = /^1[34578]\d{9}$/g;

// （5）用户名正则
var regex = /^[a-zA-Z\$][a-zA-Z0-9_\$]{4,16}$/;
```

### 6. 对JSON的理解

JSON 是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。
在项目开发中，使用 JSON 作为前后端数据交换的方式。在前端通过将一个符合 JSON 格式的数据结构序列化为
JSON 字符串，然后将它传递到后端，后端通过 JSON 格式的字符串解析后生成对应的数据结构，以此来实现前后端数据的一个传递。
因为 JSON 的语法是基于 js 的，因此很容易将 JSON 和 js 中的对象弄混，但是应该注意的是 JSON 和 js 中的对象不是一回事，JSON 中对象格式更加严格，比如说在 JSON 中属性值不能为函数，不能出现 NaN 这样的属性值等，因此大多数的 js 对象是不符合 JSON 对象的格式的。
在 js 中提供了两个函数来实现 js 数据结构和 JSON 格式的转换处理，

JSON.stringify 函数，通过传入一个符合 JSON 格式的数据结构，将其转换为一个 JSON 字符串。如果传入的数据结构不符合 JSON 格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为 JSON 格式的字符串。
JSON.parse() 函数，这个函数用来将 JSON 格式的字符串转换为一个 js 数据结构，如果传入的字符串不是标准的 JSON 格式的字符串的话，将会抛出错误。当从后端接收到 JSON 格式的字符串时，可以通过这个方法来将其解析为一个 js 数据结构，以此来进行数据的访问


### 7 js 脚本延迟加载的方法有哪些(js 默认是不延迟加载的, 默认就是执行到那里就会执行 js 代码)

延迟加载就是等页面加载完成之后再加载 javascript 文件, js 延迟加载有助于提高页面加载速度
一般有以下几种方式(5 种)
> defer 属性  给 js 脚本添加 defer 属性，这个属性会让脚本的加载与文档的解析同步解析，然后在文档解析完成后再执行这个脚本文件，这样的话就能使页面的渲染不被阻塞。多个设置了 defer 属性的脚本按规范来说最后是顺序执行的，但是在一些浏览器中可能不是这样
> async 属性：给 js 脚本 添加 async 属性, 这个属性会使脚本异步加载,不会阻塞页面的解析过程,但是当脚本加载完成后立即执行 js 脚本, 这个时候如果文档没有解析完成的话同样会阻塞. 多个 async 属性的脚本的执行顺序是不可预测的,一般不会按照代码的顺序依次执行.
> 动态创建  DOM 方式; 动态创建 DOM 标签的方式, 可以对文档的加载事件进行监听, 当文档加载完成后在动态的创建 script 标签来进入 js脚本
> 使用 setTimeout 延迟方法： 设置一个定时器来延迟加载js脚本文件
> 让 JS 最后加载： 将 js 脚本放在文档的底部，来使 js 脚本尽可能的在最后来加载执行。

### 8 js 类数组对象的定义

一个拥有 length 属性 和 若干索引属性的对象就可以被称为类数组对象, 类数组对象和数组类似, 但是不能调用数组的方法,常见的类数组对象有 arguments 和 DOM 方法的返回结果, 还有一个函数也可以被看作是类数组对象,因为它含有 length 属性值,代表可接受的参数个数
常见的类数组对转为数组的方法有这么几种(因为 类数组对象不能调用数组对象的方法,所以需要将类数组对象转为数组对象)

1. slice
    最经典的房价,使用 Array 的 slice 方法, 此方法如果不传递参数的话会返回原数组的一个拷贝, 因此可以使用此方法转换类数组到数组
    var arr = Array.prototype.slice.call(arguments);

2. Array.from() 天生的可以对类数组对象操作
 Array.from() 方法对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例


### 9 数组有哪些原生方法
> 数组和字符串的转换方法,toString(), toLocalString(), join() 其中 join() 方法可以指定转换为字符串的分隔符.
> 数组尾部操作的方法 pop() 和 push() ,push 方法可以传入多个参数
> 数组首部操作的方法 shift() 和 unshift 重排序的方法 reverse() 和 sort(), sort() 方法可以传入一个函数来进行比较,传入前后两个值,如果返回值为正数,则交换两个参数的位置
> 数组连接的方法 concat(), 返回的是拼接好的数组,不影响原数组,传入的参数可以是数组也可以是字符串,可以传入多个参数
> 数组截取方法 slice,用于截取数组中的一部分返回,
> 数组插入方法 splice(), 影响原数组
### 10 Unicode、UTF-8、UTF-16、UTF-32的区别？

### 12. 为什么函数的 arguments 参数是类数组而不是数组？如何遍历类数组?

### 13. 什么是 DOM 和 BOM？
dom 指的是文档对象模型,它指的是把文档当做一个对象,这个对象主要定义了处理网页内容的方法和接口
bom 指的是浏览器对象模型, 它指的是把浏览器当做一个对象来对待, 这个对象主要定义了与浏览器进行交互的方法和接口. bom 的核心是 window , 而 window 对象具有双重角色, 它既是 通过 js 访问浏览器窗口的一个接口, 有事一个 global(全局)对象
### 14 对类数组对象的理解,如何转化为数组

  具体可以看问题 8 

###  15 escape、encodeURI、encodeURIComponent 的区别

* encodeURI 是对整个 URI 进行转义，将 URI 中的非法字符转换为合法字符，所以对于一些在 URI 中有特殊意义的字符不会进行转义。
encodeURIComponent 是对 URI 的组成部分进行转义，所以一些特殊字符也会得到转义。
escape 和 encodeURI 的作用相同，不过它们对于 unicode 编码为 0xff 之外字符的时候会有区别，escape 是直接在字符的 unicode 编码前加上 %u，而 encodeURI 首先会将字符转换为 UTF-8 的格式，再在每个字节前加上 %。

### 16 对 ajax 的理解,实现一个 ajax 请求

AJAX是 Asynchronous JavaScript and XML 的缩写,指的是通过 javascript 异步通信,从服务器获取 xml文档从中提取数据,再更新到当前页面的对应部分,而不用刷新这个页面

步骤:

1. XMLHttpRequest对象 (xhr):浏览器提供的 js 对象,可以请求服务器上的数据
2. 使用 xhr 发送 get 请求
   (1) 创建 一个 xhr 对象,一个请求的主体
   (2) 调用 xhr 的 open函数,参数分别为请求类型和请求主体
   (3) 调用 xhr 的 send 函数,发起请求
   (4) 监听 onreadstatechange 事件(请求状态被改变时)
   请求状态：readyState(4)
   响应状态：status(200)

   ```js
    var  xhr = var XMLHttpRequest()
        xhr.open("方式",url)
        xhr.send()
        xhr.onreadstatechange =function()=>{
            if(xhr.readyState == 4  &&  xhr.status == 200){
                 console.log(xhr.responseText)
            }
        }
   ```

3、xhr对象的readyState属性：当前Ajax请求所处的状态

0	UNSENT	XMLHttpRequest对象已被创建，但未调用open方法
1	OPENED	open方法已调用
2	HEADERS_RECEIVED	send方法已调用，响应头已经被接收
3	LOADING	数据接受中，此时response属性中已经包含部分数据
4	DONE	Ajax请求完成，数据已经彻底完成或失败

4、使用xhr对象发起带参数的get请求
(1) 为 url 地址指定参数即可, url 拼接到后面的参数称为"查询字符串"
(2) 查询字符串的格式为: 在 url之后添加问号(?),参数以键值对方式存在
（3）get请求方式的本质，需要携带参数时，都是以查询字符串的方式提交参数

```js
var xhr = new XMLHttpRequest()
xhr.open('get', '请求路径?id=1')
xhr.send()
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText)
  }
}
```
5、URL编码（URL只包括英文字母、符号、数字）
（1）中文需要编码（转义）
（2）编码原则：使用安全字符(英文字符等)表示不安全字符(中文等)，中文字符使用英文字符表示的话是三组%
（3）对URL编码与解码
* 编码：encodeURI(要编码的内容)
* 解码：decodeURI(要解码的内容)
* 浏览器会自己对中文进行编码和解码
6、使用xhr发起post请求

（1）创建xhr对象

（2）调用open()函数，指定请求方式和URL地址

（3）调用setRequestheader()，设置Content-Type属性（固定写法，必须写在open之后）

（4）xhr.send()，指定要发送的数据

```js
var xhr = new XMLHttpRequest()
xhr.open('post', '请求路径')
xhr.setRequestheader('Content-Type', 'application/x-www-form-urlencoded')
xhr.send('数据=值&数据=值)
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText)
  }
}
```
7、数据交换格式
服务器端返回的数据格式是字符串，为了更好的提取返回的信息，返回的字符串有一些特定的格式：XML、JSON
* XML：标签格式
* JSON：像数组、对象的格式
  
8、XML可扩展的标记语言
HTML和XML没有关系

HTML是网页内容的载体，XML是用来传输数据的，是数据的载体

```js
<note>
  <to>ls</to>
  <from>zs</from>
  <heading>通知</heading>
  <body>开会</body>
</note>

```
XML缺点：格式臃肿，和数据无关代码多，传输效率低；在JS中解析XML比较麻烦

9、JSON（本质是字符串，是一种具有对象或者数组格式的字符串，也是一种轻量级的文本数据交换格式，更小更快更易解析）

1）JSON的两种结构：

数组&字符串(key必须用英文双印包裹，value只有数字、字符串、布尔值、null、数组、对象6种类型)；数组取值也只有这6种类型

（2）JSON语法注意事项

属性名必须使用双引号包裹；字符串都必须使用双引号；JSON中不能写注释；JSON最外层必须是数组或对象格式；JSON中不允许有undefined和函数等数据

（3）（序列化）JSON字符串转化为JS对象：JSON.parse(json字符串)

```js
var str = '{"a": "hello", "b": "world"}'  //注意格式
console.log(JSON.parse(str))  // {a: "hello", b: "world"}
```
（4）（反序列化）JSON对象转化为JSON字符串：JSON.stringify(js对象)

```js
var str = {a: "hello", b: "world"}
console.log(JSON.stringify(str))  // {"a":"hello","b":"world"}
```

11、XMLHttpRequest Level2  
 新功能: 可以设置HTTP请求时限;可使用FormData对象管理表单数据；可以上传文件;可以获取数据传输的进度信息;
 (1). 设置http请求时限（timeout属性）
```js
    xhr.timeout = 3000
// 请求超时后，会触发超时回调函数，告诉用户请求超时
xhr.ontimeout = function(event) {
  alert('请求超时了')
}
```

（2）FormData对象管理表单数据

```js
// post请求
// 创建FormData实例
var fd = new FormData()
fd.append('name', 'zs')
fd.append('age', '20')
xhr.send(fd)
```

```js
var fd = new FormData()
// 向fd中添加数据
fd.append('name', 'zs')
// fd.get(数据名)  获取表单fd中对应的值
console.log(fd.get('name')) // zs
// 给一个键值对添加多个值
fd.append('name', 'ls')
// fd中一个数据有多个值时，fd.get(数据名)获取到第一个值
console.log(fd.get('name')) // zs
// fd中获取同一个数据的多个值时：fd.getAll(数据名)
console.log(fd.getAll('name')) // ['zs', 'ls']
// 使用set可重置fd中对应数据的值
fd.set('name', 'ww')
console.log(fd.get('name')) // ww
// 删除fd中某个元素
fd.delete('name')
console.log(fd.get('name')) // null
```
(3) 上传文件功能

* 定义 ui 结构;
* 验证是否选择了文件;
* 向 fromData 中追加文件;
* 使用 xhr 进行上传文件请求;
* 监听 onreadystatechange 

获取表单提交的文件：dom.files，如果有文件选中，则长度不为0

(4) 显示文件上传进度：监听xhr.upload.onprogress事件

e.lengthComputable 若为true，才可计算长度

e.loaded 已经传输的字符

e.total 需传输的总字节

传输进度 = Math.ceil(e.loaded / e.total * 100) + '%'

(5）jquery实现文件上传
上传文件时必须要设置两个参数：

```js
// 是否转化为查询字符串
processData: false,
// 发送到服务器端的数据类型
contentType: false,
```
可用ajaxStart和ajaxStop设置加载效果

ajaxStart()：请求开始时运行的函数（只能被$(document)调用）

ajaxStop()：请求完成时运行的函数（只能被$(document)调用）

也可用beforeSend和complete代替，更好用

12、axios：网络数据请求，比jquery更轻量，只专注于网络请求

（1）axios发起get请求

```js
axios.get('url', { params: { /*参数*/ } }).then(callback)
```
（2）axios发起post请求

```js
axios.post('url', { data: { /*参数*/ } }).then(callback)
```


### 17 javascript 为什么要进行变量提升,它导致了什么问题? 

变量提升的表现,无论在函数中任何位置声明的变量,都被提升到函数的首位,可以在变量声明前访问到而不会报错.

造成变量声明提升的本质是 js 引擎在代码执行前有一个解析的过程,创建了执行上下文,初始化了一些代码执行时需要用到的对象.当访问一个变量
时,会到当前执行上下文中的作用域链中去查找,而作用域链的首端指向的是当前执行上下文的变量对象,这个变量对象是执行上下文的一个属性,它包含了函数的形参、所有的函数和变量声明，这个对象的是在代码解析的时候创建的.

首先要知道，JS在拿到一个变量或者一个函数的时候，会有两步操作，即解析和执行。

> 在解析阶段, js会检查语法,并对函数进行预编译,编译的时候会先创建一个全局执行上下文环境, 先把代码中即将执行的变量,函数声明都拿出来,变量先赋值为 undefined,函数先声明好可使用,在一个函数执行之前,也会创建一个函数执行上下文环境,跟全局执行上下文类似,不过函数执行上下文会多出 this arguments 和函数的参数

> 全局上下文,变量定义，函数声明

那为什么会进行变量提升呢？主要有以下两个原因：

* 提高性能
* 容错性更好
  
(1) **提高性能** 在JS代码执行之前，会进行语法检查和预编译，并且这一操作只进行一次。这么做就是为了提高性能，如果没有这一步，那么每次执行代码前都必须重新解析一遍该变量（函数），而这是没有必要的，因为变量（函数）的代码并不会改变，解析一遍就够了。

(2) 在解析的过程中，还会为函数生成预编译代码。在预编译时，会统计声明了哪些变量、创建了哪些函数，并对函数的代码进行压缩，去除注释、不必要的空白等。这样做的好处就是每次执行函数时都可以直接为该函数分配栈空间（不需要再解析一遍去获取代码中声明了哪些变量，创建了哪些函数），并且因为代码压缩的原因，代码执行也更快了。

(2) **容错性更好**

变量提升可以在一定程度上提高JS的容错性，看下面的代码：



```js
a = 1;var a;console.log(a);
```
虽然，在可以开发过程中，可以完全避免这样写，但是有时代码很复杂的时候。可能因为疏忽而先使用后定义了，这样也不会影响正常使用。由于变量提升的存在，而会正常运行。


**总结** 

* 解析和预编译过程中的声明提升可以提高性能，让函数可以在执行时预先为变量分配栈空间
* 声明提升还可以提高JS代码的容错性，使一些不规范的代码也可以正常执行

变量提升虽然有一些优点，但是他也会造成一定的问题，在ES6中提出了let、const来定义变量，它们就没有变量提升的机制。下面看一下变量提升可能会导致的问题：

```js
ar tmp = new Date();

function fn(){
	console.log(tmp);
	if(false){
		var tmp = 'hello world';
	}
}

fn();  // undefined

```
在这个函数中，原本是要打印出外层的tmp变量，但是因为变量提升的问题，内层定义的tmp被提到函数内部的最顶部，相当于覆盖了外层的tmp，所以打印结果为undefined

```js
var tmp = 'hello world';

for (var i = 0; i < tmp.length; i++) {
	console.log(tmp[i]);
}

console.log(i); // 11
```
由于遍历时定义的i会变量提升成为一个全局变量，在函数结束之后不会被销毁，所以打印出来11。

### 18. 什么是尾调用，使用尾调用有什么好处？
   尾调用指的是函数的最后一步调用另一个函数。代码执行是基于执行栈的，所以当在一个函数里调用另一个函数时，会保留当前的执行上下文，然后再新建另外一个执行上下文加入栈中。使用尾调用的话，因为已经是函数的最后一步，所以这时可以不必再保留当前的执行上下文，从而节省了内存，这就是尾调用优化。但是 ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。
   <!-- 可以看一下其他文章 -->

### 19. ES6模块与CommonJS模块有什么异同？

ES6 Module和CommonJS模块的区别：

* CommonJS 模块输出的是一个值的拷贝,es6 模块输出的是值的引用
* CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
* import的接⼝是read-only（只读状态），不能修改其变量值。 即不能修改其变量的指针指向，但可以改变变量内部指针指向，可以对commonJS对重新赋值（改变指针指向），但是对ES6 Module赋值会编译报错。
  

  ES6 Module和CommonJS模块的共同点：
  * CommonJS和ES6 Module都可以对引⼊的对象进⾏赋值，即对对象内部属性的值进⾏改变。
<!-- 具体可以看 module 模块 -->



### 20. 常见的 dom 操作有哪些?

1）DOM 节点的获取
DOM 节点的获取的API及使用：
```js
    getElementById // 按照 id 查询
    getElementsByTagName // 按照标签名查询
    getElementsByClassName // 按照类名查询
    querySelectorAll // 按照 css 选择器查询
    // 按照 id 查询
    var imooc = document.getElementById('imooc') // 查询到 id 为 imooc 的元素
    // 按照标签名查询
    var pList = document.getElementsByTagName('p')  // 查询到标签为 p 的集合  类数组对象
    console.log(divList.length)
    console.log(divList[0])
    // 按照类名查询
    var moocList = document.getElementsByClassName('mooc') // 查询到类名为 mooc 的集合
    // 按照 css 选择器查询
    var pList = document.querySelectorAll('.mooc') // 查询到类名为 mooc 的集合
```
2）DOM 节点的创建

```js
// 首先获取父节点
var container = document.getElementById('container')
// 创建新节点
var targetSpan = document.createElement('span')
// 设置 span 节点的内容
targetSpan.innerHTML = 'hello world'
// 把新创建的元素塞进父节点里去
container.appendChild(targetSpan)

```
3)DOM 节点的删除
<!-- 让父元素删除子元素 -->

```js
// 获取目标元素的父元素
var container = document.getElementById('container')
// 获取目标元素
var targetNode = document.getElementById('title')
// 删除目标元素
container.removeChild(targetNode)
```


或者通过子节点数组来完成删除：
```js
 //获取目标元素的父元素
 var container = document.getElementById('container')
// 获取目标元素
var targetNode = container.childNodes[1]
// 删除目标元素
container.removeChild(targetNode)
```

修改 DOM 元素这个动作可以分很多维度，比如说移动 DOM 元素的位置，修改 DOM 元素的属性等。

将指定的两个 DOM 元素交换位置， 已知的 HTML 结构如下：

```js
<html>
  <head>
    <title>DEMO</title>
  </head>
  <body>
    <div id="container"> 
      <h1 id="title">我是标题</h1>
      <p id="content">我是内容</p>
    </div>   
  </body>
</html>
```
现在需要调换 title 和 content 的位置，可以考虑 insertBefore 或者 appendChild：

```js
// 获取父元素
var container = document.getElementById('container')   
 
// 获取两个需要被交换的元素
var title = document.getElementById('title')
var content = document.getElementById('content')
// 交换两个元素，把 content 置于 title 前面
container.insertBefore(content, title)

```

### 21. use strict是什么意思 ? 使用它区别是什么？
use strict 是一种 ECMAscript5 添加的（严格模式）运行模式，这种模式使得 Javascript 在更严格的条件下运行。设立严格模式的目的如下
* 消除 Javascript 语法的不合理、不严谨之处，减少怪异行为;
* 消除代码运行的不安全之处，保证代码运行的安全；
* 提高编译器效率，增加运行速度；
* 为未来新版本的 Javascript 做好铺垫。

区别：

* 禁止使用 with 语句。
* 禁止 this 关键字指向全局对象。
* 对象不能有重名的属性。

### 22. 如何判断一个对象是否属于某个类？  考察数据类型,在数据类型有详细的说明

instanceof  Object.prototye.toString()


### 23. 强类型语言和弱类型语言的区别  类型检查

* 强类型语言：强类型语言也称为强类型定义语言，是一种总是强制类型定义的语言，要求变量的使用要严格符合定义，所有变量都必须先定义后使用。Java和C++等语言都是强制类型定义的，也就是说，一旦一个变量被指定了某个数据类型，如果不经过强制转换，那么它就永远是这个数据类型了。例如你有一个整数，如果不显式地进行转换，你不能将其视为一个字符串。

*  弱类型语言：弱类型语言也称为弱类型定义语言，与强类型定义相反。JavaScript语言就属于弱类型语言。简单理解就是一种变量类型可以被忽略的语言。比如JavaScript是弱类型定义的，在JavaScript中就可以将字符串'12'和整数3进行连接得到字符串'123'，在相加的时候会进行强制类型转换。

两者对比：强类型语言在速度上可能略逊色于弱类型语言，但是强类型语言带来的严谨性可以有效地帮助避免许多错误。

但是强类型语言, 也可以强制进行类型转化


### 24. 解释性语言和编译型语言的区别

（1）解释型语言
使用专门的解释器对源程序逐行解释成特定平台的机器码并立即执行。是代码在执行时才被解释器一行行动态翻译和执行，而不是在执行之前就完成翻译。解释型语言不需要事先编译，其直接将源代码解释成机器码并立即执行，所以只要某一平台提供了相应的解释器即可运行该程序。其特点总结如下

解释型语言每次运行都需要将源代码解释称机器码并执行，效率较低；
只要平台提供相应的解释器，就可以运行源代码，所以可以方便源程序移植；
JavaScript、Python等属于解释型语言。

（2）编译型语言
使用专门的编译器，针对特定的平台，将高级语言源代码一次性的编译成可被该平台硬件执行的机器码，并包装成该平台所能识别的可执行性程序的格式。在编译型语言写的程序执行之前，需要一个专门的编译过程，把源代码编译成机器语言的文件，如exe格式的文件，以后要再运行时，直接使用编译结果即可，如直接运行exe文件。因为只需编译一次，以后运行时不需要编译，所以编译型语言执行效率高。其特点总结如下：

一次性的编译成平台相关的机器语言文件，运行时脱离开发环境，运行效率高；
与特定平台相关，一般无法移植到其他平台；
C、C++等属于编译型语言。

两者主要区别在于： 前者源程序编译后即可在该平台运行，后者是在运行期间才编译。所以前者运行速度快，后者跨平台性好


### 25 for...in和for...of的区别  
<!-- 可以查看阮一峰 es6 的迭代器接口和 for of -->

for...of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

for of 是 es6 新增的遍历方法, 允许遍历一个含有 迭代器接口 [iterator接口]的数据结构(数组,对象等),并且返回各项的值,和 es3中的 for...in 的区别如下

* for…of 遍历获取的是对象的键值，for…in 获取的是对象的键名；
* for… in 会遍历对象的整个原型链，性能非常差不推荐使用，而 for … of 只遍历当前对象不会遍历原型链；
* 对于数组的遍历，for…in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；
  
总结： for...in 循环主要是为了遍历对象而生，不适用于遍历数组；for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象。


### 如何使用for...of遍历对象

for...of 是作为es6新增的遍历方式,允许遍历一个 含有 iterator 接口的数据接口,并且返回各项的值,普通的对象用for..of遍历是会报错的

如果需要遍历的对象是类数组对象，用Array.from转成数组即可。

```js
var obj = {
    0:'one',
    1:'two',
    length: 2
};
obj = Array.from(obj);
for(var k of obj){
    console.log(k)
}
```
如果不是类数组对象，就给对象添加一个[Symbol.iterator]属性，并指向一个迭代器即可。

```js

//方法一：
var obj = {
    a:1,
    b:2,
    c:3
};

obj[Symbol.iterator] = function(){
	var keys = Object.keys(this);
	var count = 0;
	return {
		next(){
			if(count<keys.length){
				return {value: obj[keys[count++]],done:false};
			}else{
				return {value:undefined,done:true};
			}
		}
	}
};

for(var k of obj){
	console.log(k);
}


// 方法二
var obj = {
    a:1,
    b:2,
    c:3
};
obj[Symbol.iterator] = function*(){
    var keys = Object.keys(obj);
    for(var k of keys){
        yield [k,obj[k]]
    }
};

for(var [k,v] of obj){
    console.log(k,v);
}
```
###   ajax、axios、fetch的区别

（1）AJAX
Ajax 即“AsynchronousJavascriptAndXML”（异步 JavaScript 和 XML），是指一种创建交互式网页应用的网页开发技术。它是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。其缺点如下：
* 本身是针对MVC编程，不符合前端MVVM的浪潮
* 基于原生XHR开发，XHR本身的架构不清晰
* 不符合关注分离（Separation of Concerns）的原则
* 配置和调用方式非常混乱，而且基于事件的异步模型不友好。
(2)Fetch
fetch号称是AJAX的替代品，是在ES6出现的，使用了ES6中的promise对象。Fetch是基于promise设计的。Fetch的代码结构比起ajax简单多。fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象

fetch的优点：

* 语法简洁，更加语义化
* 基于标准 Promise 实现，支持 async/await
* 更加底层，提供的API丰富（request, response）
* 脱离了XHR，是ES规范里新的实现方式

fetch的缺点：
fetch只对网络请求报错，对400，500都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
fetch默认不会带cookie，需要添加配置项： fetch(url, {credentials: 'include'})
fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
fetch没有办法原生监测请求的进度，而XHR可以

（3）Axios
Axios 是一种基于Promise封装的HTTP客户端，其特点如下：

1. 浏览器端发起XMLHttpRequests请求
2. node端发起http请求
3. 支持Promise API
4. 监听请求和返回
5. 对请求和返回进行转化
6. 取消请求
7. 自动转换json数据
8. 客户端支持抵御XSRF攻击


### 28. 数组的遍历方法有哪些

| 方法                      | 是否改变原数组 | 特点                                                                                                                                       |
| ------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| forEach()                 | 否             | 数组方法，不改变原数组，没有返回值 ,如果原数组是数组对象,可以改变数组独享的属性                                                            |
| map()                     | 否             | 数组方法，不改变原数组，有返回值，可链式调用                                                                                               |
| filter()                  | 否             | 数组方法，过滤数组，返回包含符合条件的元素的数组，可链式调用                                                                               |
| for...of                  | 否             | for...of遍历具有Iterator迭代器的对象的属性，返回的是数组的元素、对象的属性值，不能遍历普通的obj对象，将异步循环变成同步循环                |
| every() 和 some()         | 否             | 数组方法，some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false.                                                     |
| find() 和 findIndex()     | 否             | 数组方法，find()返回的是第一个符合条件的值；findIndex()返回的是第一个返回条件的值的索引值                                                  |
| reduce() 和 reduceRight() | 否             | 数组方法，reduce()对数组正序操作；reduceRight()对数组逆序操                                                                 作 求乘积,求和 |
### 29   forEach和map方法有什么区别
这方法都是用来遍历数组的，两者区别如下：

forEach()方法会针对每一个元素执行提供的函数，对数据的操作不会改变原数组，该方法没有返回值；
map()方法不会改变原数组的值，返回一个新数组，新数组中的值为原数组调用函数处理之后的值；
### 疑惑点

### 1.  new 最后一步

### 2. 迭代是什么意思，迭代器是什么？  

    迭代是遍历的意思

### 3.  数据的传输是json 格式的字符串

### 4. 问题 uri 和 url 的区别

url +urn   =uri

uri 是 url 的超级
### 5. module 模块的学习

### 6. for  for in  和 for of 的区别

### 7. this 指向经典面试题

```js
var x = 20;
var a = {
 x: 15,
 fn: function() {
 var x = 30;
 return function() {
  return this.x
 }
 }
}
console.log(a.fn());
console.log((a.fn())());
console.log(a.fn()());
console.log(a.fn()() == (a.fn())());
console.log(a.fn().call(this));
console.log(a.fn().call(a));
```

1.console.log(a.fn());
对象调用方法，返回了一个方法。
# function() {return this.x}

2.console.log((a.fn())());
a.fn()返回的是一个函数，()()这是自执行表达式。this -> window
//20

3.console.log(a.fn()());
a.fn()相当于在全局定义了一个函数，然后再自己调用执行。this -> window
//20

4.console.log(a.fn()() == (a.fn())());
//true

5.console.log(a.fn().call(this));
这段代码在全局环境中执行，this -> window
//20

6.console.log(a.fn().call(a));
this -> a
//15

### iterator接口是什么?  迭代器接口

* [文档1](https://www.yisu.com/zixun/632393.html)
* [文档2](https://www.csdn.net/tags/Mtjakg0sNjk5NzgtYmxvZwO0O0OO0O0O.html)


### 8 Generator 对象 是什么?



#### 9 js 中数组遍历的使用场景

map 的使用场景可以查看[参考文章](https://zhuanlan.zhihu.com/p/481795339)


foreach
遍历数组中的每一项，没有返回值，对原数组没有影响，不支持IE

map
有返回值
map的回调函数中支持return返回值；return的是啥，相当于把数组中的这一项变为啥（并不影响原来的数组，只是相当于把原数组克隆一份，把克隆的这一份的数组中的对应项改变了）；

filter
不会改变原始数组,返回新数组

every
every()是对数组中的每一项运行给定函数，如果该函数对每一项返回true,则返回true。返回布尔值

find
find()方法返回数组中符合测试函数条件的第一个元素。否则返回undefined

```js
let  bb =[{a:1},{a:2}]
let cc = bb.find((item)=>{return item.a == 1})

```

findIndex
返回索引值，如果没有符合条件的元素则返回-1

findIndex 不会改变数组对象。