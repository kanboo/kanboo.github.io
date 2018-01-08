---
title: JS-ArrayUnique
date: 2018-01-08 22:59:00
categories: 
- JS
tags:
- JS
- Array
- Unique
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">紀錄一下 ArrayUnique 從早期`indexOf`的方式到現今各種優化的寫法。</font>

{% endcq %}

<!-- more -->
***

## 前言

下列的各種範例，統一使用的 **Array資料**，如下

``` js ArrayData
var source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
```

***
## forloop + indexOf

早期使用的 `for迴圈` + `indexOf` 判斷是否已存在的值。

``` js indexOf
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
let result_01 = [];

for (let i = 0; i < source.length; i++) {
    let el = source[i];
    if (result_01.indexOf(el) === -1) result_01.push(el);
}

console.log("result_01", result_01);
```

***
## includes

在ES6提供了一個新方法：`Array.prototype.includes()`，判斷陣列中是否已有相同的值？
- 有相同的值，回傳 `true`
- 無相同的值，回傳 `false`

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
let result_02 = [];

for (let i = 0; i < source.length; i++) {
    let el = source[i];
    if (!result_02.includes(el)) result_02.push(el);
}

console.log("result_02", result_02);
```

<div class="note info">[Array.prototype.includes()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)</div>

***
## forEach

將原本使用的 `for迴圈` ，改使用 `forEach` 執行，省略掉定義 `i`、`source.length`...等動作。

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
let result_03 = [];

source.forEach((el) => {
    if (!result_03.includes(el)) result_03.push(el);
});

console.log("result_03", result_03);
```

<div class="note info">[arr.forEach](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)</div>

***
## reduce

遍歷每個元素，將一個累加器及陣列中每項元素（由左至右）傳入回呼函式，將陣列簡化為單一值。

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

let result_04 = source.reduce((p, c) => {
    if (!p.includes(c)) p.push(c);
    return p;
}, []);

console.log("result_04", result_04);
```
<div class="note info">[reduce()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)</div>

***
## Array.from + Set

`Array.from()` 會從類陣列(array-like)或是可迭代的物件<font color="red">建立一個新的陣列</font>。

`Set` 對象允許你存儲任何類型的<font color="red">唯一值</font>，無論是原始值或者是對象引用。

根據上述二種方法的特性，快速達成產生一個 <font color="red">**已去除重覆值的新陣列**</font>

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

let result_05 = Array.from(new Set(source));

console.log("result_05", result_05);
```

``` js 簡寫
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

let result_06 = [...new Set(source)];

console.log("result_06", result_06);
```

<div class="note info">[Array.from()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
[Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)
[阮一峰-Set](http://es6.ruanyifeng.com/#docs/set-map#Set)</div>