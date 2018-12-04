---
title: 别再用for循环了！拥抱JS高阶函数让代码更优雅（二）map
date: 2018-12-04 09:55:34
category: 技术
tags: 前端
---
filter是作为筛选器，把数组按条件过滤一次。那想不过滤，只是把数组每个元素操作加工一遍，用什么方法呢？map就能实现

# Array.map()

map()接收一个回调函数，遍历传入每一个item值，执行回调，将return的值组成新数组。

talk is cheap,show me the code!

现在有一个需求，要把所有动物的名字，单独提取出来组成数组。

```js
var animals = [
  { name: 'Fluffykins', species: 'rabbit' },
  { name: 'Caro',       species: 'dog' },
  { name: 'Hamilton',   species: 'dog' },
  { name: 'Harold',     species: 'fish' },
  { name: 'Ursula',     species: 'cat' },
  { name: 'Jimmy',      species: 'fish' }
]
```
我们for循环是这么做的

```js
var names= []
for (var i = 0; i < animals.length; i++) {
  names.push(animals[i].name)
}
console.log(names)
```

如果用map呢？

```js
var names = animals.map((x) => x.name)
console.log(names)
```

四行变一行。
同样我们可以在返回前，或返回时进行其他修饰操作。

```js
var names = animals.map((x) => x.name + 'is a' + x.species)
console.log(names)
```

如果要用到映射关系的数组操作，用map()就对啦！