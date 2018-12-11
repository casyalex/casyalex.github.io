---
title: 别再用for循环了！拥抱JS高阶函数让代码更优雅（三）reduce
date: 2018-12-11 11:12:18
category: 技术
tags: 前端
---
reduce相比于filter，map有更强大的能力。实际上你可以用reduce实现map与filter的能力。

具一个最简单的例子，订单求和：
```js
var orders = [
    { amount: 250 },
    { amount: 400 },
    { amount: 100 },
    { amount: 325 },
]
```
我们只需要遍历orders里的amount加起来。for循环如下：

```js
var totalAmount = 0
for (var i = 0; i < orders.length; i++) {
    totalAmount += orders[i].amount
}
console.log(totalAmount)
```

那如果用reduce呢？ Array.reduce 回调接收两个参数

```js
var totalAmount = orders.reduce(() => {
    
})
```