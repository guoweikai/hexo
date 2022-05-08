---
title: javascript手写代码
date: 2022-03-10 11:24:20
tags:
---

### 1. 手写 Object.create
   Object.create() 函数传入一个对象, 将此对象作为原型,创建一个新对象


   ```js
    Object.myCreate = function (proto){
      const fn = funtion(){}
      fn.prototype = proto
      return new fn()
    }

   ```

### 手写 instanceof 方法


#### 



问题: 
### Object 有哪些静态方法

Object.assign(target,...source)
把任意多个的源对象可枚举属性拷贝给目标对象,然后返回目标对象

Object.create()


Object.keys(obj)


### Array 有哪些静态方法和属性


### Function 有哪些静态方法和属性



想法:
1. 最少要联系 100 道题(目前有 33 道)
2. 先自己想,然后看答案, 将题目写上自己写
