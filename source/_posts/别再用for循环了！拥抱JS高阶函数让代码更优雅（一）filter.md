---
title: 别再用for循环了！拥抱JS高阶函数让代码更优雅（一）filter
date: 2018-12-03 10:32:39
tags: 技术
---
两年前kaivon老师说，重复做一件事，第一时间应该想到用for-loop，比如最简单的数组求和。
```javascript
let arr = [1, 2, 3, 4, 5]
let sum = new Number
for(var i = 0; i < arr.length; i++){
  sum += arr[i]
}
return sum // 15
```
对于初学者（当时的我）来说，for循环更能理清思路，循环条件与要做的事情分开很清楚。但是在mvvm的框架，for的格式显得格格不入，甚至有点别扭了。比如vue里的一个method:
```javascript
getTextLine (ctx, obj) {
  ctx.setFontSize(obj.size)
  let arrText = obj.text.split('')
  let line = ''
  let arrTr = []
  for (let i = 0; i < arrText.length; i++) {
    var testLine = line + arrText[i]
    var metrics = ctx.measureText(testLine)
    var width = metrics.width
    if (width > obj.width && i > 0) {
        arrTr.push(line)
        line = arrText[i]
    } else {
        line = testLine
    }
    if (i === arrText.length - 1) {
        arrTr.push(line)
    }
  }
  return arrTr
}
```

for循环让本来的函数体多了几个缩进，有没有更加优雅的写法呢？

肯定是有的，比如一开始的求和,我们可以改造一下
```javascript
let arr = [1, 2, 3, 4, 5]
let sum = arr.reduce((result,item) => result += item, 0)
console.log(sum) // 15
```
三行变一行，舒服了！reduce是一个非常好用，也非常博大精深的高阶函数，以后会更详细的解释。今天我们先从简单的filter说起。

<!-- more -->

# Array.filter()

下面的例子（及后续系列）均引用mpjme的youtube频道fun fun function，讲得很好，推荐大家关注：[fun fun function](https://www.youtube.com/channel/UCO1cgjhGzsSYb1rsB4bFe4Q)

filter是一个数组的高阶函数，如它的字面意思，起到一个为数组过滤的作用。
```js
var animals = [
  { name: 'Dolce', species: 'rabbit' },
  { name: 'Caro', species: 'cat' },
  { name: 'Hamliton', species: 'dog' },
  { name: 'Harold', species: 'dog' },
  { name: 'Jimmy', species: 'cat' },
  { name: 'Ursula', species: 'fish' },
  { name: 'TJ', species: 'dog' },
]
```
现在有一组动物，分别记录了名字和种类，现在要求把里面的所有狗找出来，筛选出新数组。用for循环一般是这样做的。

```js
var animals = [
  { name: 'Dolce', species: 'rabbit' },
  { name: 'Caro', species: 'cat' },
  { name: 'Hamliton', species: 'dog' },
  { name: 'Harold', species: 'dog' },
  { name: 'Jimmy', species: 'cat' },
  { name: 'Ursula', species: 'fish' },
  { name: 'TJ', species: 'dog' },
]

let dogs = []
for (var i = 0; i < animals.length; i++) {
  if (animals[i].species === 'dog') {
    dogs.push(animals[i])
  }
}
console.log(dogs) /* ​​​​​[  { name: 'Hamliton', species: 'dog' },​​​​​
                      ​​​​​  { name: 'Harold', species: 'dog' },​​​​​
                      ​​​​​  { name: 'TJ', species: 'dog' } ]​​​ */​​
```

filter会遍历数组，执行传入的callback函数，将return为true的那一项返回到新数组当中。
如果用filter来实现上面的需求：
```js
var animals = [
  { name: 'Dolce', species: 'rabbit' },
  { name: 'Caro', species: 'cat' },
  { name: 'Hamliton', species: 'dog' },
  { name: 'Harold', species: 'dog' },
  { name: 'Jimmy', species: 'cat' },
  { name: 'Ursula', species: 'fish' },
  { name: 'TJ', species: 'dog' },
]

let dogs = animals.filter(function(animal) {
  return animal.species === 'dog'
})
console.log(dogs)/* ​​​​​[  { name: 'Hamliton', species: 'dog' },​​​​​
                      ​​​​​ { name: 'Harold', species: 'dog' },​​​​​
                      ​​​​​ { name: 'TJ', species: 'dog' } ]​​​ */​​
```
满足 species是dog 为true 的 animal都会加入到dogs这个数组中。用ES6的箭头函数我们可以进一步简化为：

```js
let dogs = animals.filter((x) => x.species === 'dog')
```
结果是一样的。

由于filter接收的是一个回调函数，我们可以把其中的条件方法抽象出来写成函数封装起来，这样会更加灵活。

```javascript
let isDog = (x) => x.species === 'dog'
let isCat = (x) => x.species === 'cat'
let dogs = animals.filter(isDog)
let cats = animals.filter(isCat)
console.log(dogs)
console.log(cats)
```

# 进一步用法
filter的callback总共可以接受三个参数，element, index, array

>element<br>
>当前在数组中处理的元素。

>index (可选)<br>
>正在处理元素在数组中的索引。

>array (可选)<br>
>调用了filter的数组。

上面只用到了element，配合3个元素可以很方便的实现数组去重。

```js
var arr = [4, 4, 4, 6, 6, 6, 8, 12, 165, 12, 12, 444, 555]

var newArr = arr.filter((el,index,self) => {
  return self.indexOf(element) === index
})
console.log(newArr) // ​​​​​[ 4, 6, 8, 12, 165, 444, 555 ]​​​​​
```

刚刚使用高阶函数，思路会转不过来，但是习惯以后，你会爱上它的。