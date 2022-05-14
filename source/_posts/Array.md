---
title: Array
date: 2022-05-14 09:42:44
tags:
---


### 1. 有两个数组, 找出 a 数组存在, b 数组不存在的数据, 找出 b 数组存在 a 数组不存在的数组,组成一个数组

<!--  js 中有 includes 方法 -->

```js
const a = [1,2,3,4]
const b = [2,3,4,5]
// 仅存在 a 中
function getOnlyA(){
  return  a.filter((item)=>!b.includes(item))
}
// 仅存在 b 中
function getOnlyB(){
    return  b.filter((item)=>a.includes(item))
}
```

### 2. js 中找出两个数组中不同的元素
<!-- 一般都是用到了数组的过滤方法 -->

```js
const  a = [1,2,3,4]
const b = [1,2,3,4]

fucntion getNewArray(a,b){
    const array = [...a,..b]
    const newArray = array.filter((item)=>!(a.includes(item)&&b.includes(item)))

}

```
### 3. js 中找出两个数组中相同的元素

```js

const a =[1,2,3,4]
const b = [2,3,4,5]

funciton getNewArray(a,b){
    const array = [...a,...b]
    const newArray = array.filter((item)=>a.includes(item)&&b.includes(item))
}

```

### 4. js 中数组去重的方法,你能说出几种(经典面试题)
1. 对象的方法,将数组作为对象的下标,(数组对象也一样)

### 5.  如何判断一个变量是否为数组(数据类型的考察)

(1)  为什么不用 typeof
```js
var list = [1,2,3];
typeof list  //"object"
Array继承与Object，所以typeof 会直接返回object，所以不可以用typeof方法来检测
```

(2) 为什么不用 instanceof
```js
var list = [1,2,3];
list instanceof Array    //true

```

instanceof 表面上看确实是返回了true，但其实并不可靠。原因是Array实质是一个引用，用instanceof方法（包括下面的constructor方法）都是利用和引用地址进行比较的方法来确定的，但是在frame嵌套的情况下，每一个Array的引用地址都是不同的，比较起来结果也是不确定的，所以这种方法有其局限性

（3）为什么不用constructor方法？

```js
var list = [1,2,3];
list.constructor === Array;    //true
原因已经解释过了，不再赘述。
```
可靠的检测数组方式

（1）利用Object的toString方法
var list = [1,2,3];
Object.prototype.toString.call(list); //[object Array]

(2）利用ES6的Array.isArray()方法
var list = [1,2,3];
Array.isArray(list); //true

### 6.  数组的原生方法有哪些？(数组的方法考察)
### 7. 如何将一个类数组变量转化为数组？(类数组对象转换为数组) 类数组对象包括 arguments, 获取的dom 元素, 知道的应该是两种方案

### 8. 说一说ES6中对于数组有哪些扩展(对 es6 的考察)

### 9. 你知道Array.prototype的类型是什么吗？ 对构造函数的原型考察

### 10.  如何“打平”一个嵌套数组，如[1,[2,[3]],4,[5]] => [1,2,3,4,5]?你能说出多少种方法？ 对数组 api 的理解

### 11. 如何克隆一个数组？你能说出多少种？

### 12. 说一说Array.prototype.sort()方法的原理？（追问：不传递参数会如何？）

### 13. 找出Array中的最大元素，你能说出几种方法

### 14. 将字符串反向 'abc123' => '321cba'

### 15. 打平嵌套数组 [1, [2, [3], 4], 5] => [1, 2, 3, 4, 5]  和问题 10 相同

```js

// 这个题面试官想考查的应该是递归和深度优先遍历，

```

### 16  打印数组全排列
// [1,2,3] => [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
// 没有明确的遍历界限时都使用递归吧，实际应用场景，若非尾递归形式需要考虑是否可能栈溢出。
// 采用递归需要重点考虑参数及返回值的设定，本例使用有副作用的方式实现，这样实现逻辑上比较好理解，只有当满足条件时添加一条排列

### 17.寻找两个有序数组最小相同元素

### 18 有序二维数组寻找某元素坐标


### 19 数组打平并去重 [1,2,3,4,5,6,7,8]

```js
const arr = [[1, 2, 3], [3, 2, 1], [4, 5, 6, 7, 8, 8], [[2, 3, 4, 5], [[[4, 5, 6, 1, 2, 4]]]]];
```
解：
```js
const list = Array.from(new Set(arr.flat(Infinity)));
console.log(list); // [1,2,3,4,5,6,7,8]
```






### 参考

[参考文章一](https://zhuanlan.zhihu.com/p/29514159)
[参考文章二](https://blog.csdn.net/lj745280746/article/details/70880809)
[参考文章三](https://blog.csdn.net/weixin_45368324/article/details/105427638)

