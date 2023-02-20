---
title: 函数async
date: 2023-02-16 17:42:12
tags:
---

1. 含义
ES2017 标准引入了 async 函数，使得异步操作变得更加方便。
async 函数是什么？一句话，它就是 Generator 函数的语法糖。
前文有一个 Generator 函数，依次读取两个文件。
```js
const fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

```

2. 基本用法

