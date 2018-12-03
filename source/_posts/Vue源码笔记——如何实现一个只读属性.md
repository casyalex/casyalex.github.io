---
title: Vue源码笔记——如何实现一个只读属性
date: 2018-12-03 16:56:09
category: 学习笔记
tags: 前端
---

```js
  const dataDef = {}
  dataDef.get = function () { return this._data }
  const propsDef = {}
  propsDef.get = function () { return this._props }
  if (process.env.NODE_ENV !== 'production') { // 判断当为开发环境下
    dataDef.set = function (newData: Object) {
      warn(
        'Avoid replacing instance root $data. ' +
        'Use nested data properties instead.',
        this
      )
    }
    propsDef.set = function () {
      warn(`$props is readonly.`, this)
    }
  }
  Object.defineProperty(Vue.prototype, '$data', dataDef)
  Object.defineProperty(Vue.prototype, '$props', propsDef)
```

利用Object.defineProperty()把变量的setter属性提前用函数覆盖(Vue源码这里抛出一个warn警告)。当试图改写$data,$props，就会报错。

```js
this.$data = 'XXXX' // 报错 'Avoid replacing instance root $data. ' + 'Use nested data properties instead.'
this.$props = 'XXXX' // 报错 `$props is readonly.`
```

用这个方法可以实现为对象添加只读属性
```js
const obj = {}
obj.get = function() {
    return '只能读不能写哦'
}
obj.set = function() {
    console.warn('这是只读属性')
}

let eg = {}
Object.defineProperty(eg, 'readOnly', obj)
console.log(eg.readOnly) // ​​​​​我只能读不能写哦
eg.readOnly = '我就要写' // ​​​这是只读属性​​​
console.log(eg.readOnly) // 我只能读不能写哦
```

that's all