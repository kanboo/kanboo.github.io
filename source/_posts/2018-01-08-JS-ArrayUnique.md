---
title: JS-ArrayUnique
date: 2018-01-08 22:59:00
categories: 
- JS
tags:
- JS
- Array
- Unique
- Reduce
- Set
- Spread
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">紀錄 ArrayUnique 從早期`indexOf`的方式到現今各種優化的寫法。</font>

{% endcq %}

<!-- more -->
***

## 前言

下列的各種範例，統一使用的 **Array資料**，如下

``` js ArrayData
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
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

console.log("result_01", result_01); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
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

console.log("result_02", result_02); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```

<div class="note info">[MDN-Array.prototype.includes()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)</div>

***
## forEach

將原本使用的 `for迴圈` ，改使用 `forEach` 執行，省略掉定義 `i`、`source.length`...等動作。

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
let result_03 = [];

source.forEach((el) => {
    if (!result_03.includes(el)) result_03.push(el);
});

console.log("result_03", result_03); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```

<div class="note info">[MDN-arr.forEach](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)</div>

***
## reduce()

遍歷每個元素，依序<font color="red">組合、加總</font>，然後丟給下個元素，最終會回傳<font color="red">一個結果</font>

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];


let result_04 = source.reduce((p, c) => {
    //includes 判斷是否已存在
    if (!p.includes(c)) p.push(c);
    return p;
}, []);

console.log("result_04", result_04); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```
<div class="note info">[MDN-reduce()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)</div>

***
## Set() + Array.from()

`Set` 對象允許你存儲任何類型的<font color="red">唯一值</font>，無論是原始值或者是對象引用。

`Array.from()` 會從類陣列(array-like)或是可迭代的物件<font color="red">建立一個新的陣列</font>。

根據上述二種方法的特性，快速達成產生一個 <font color="red">**已去除重覆值的新陣列**</font>

``` js
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

//1. 將 source資料 丟進 new set，使其產生一個新的 set集合 ，並且已去除重覆的值
//2. 然後再將 set集合 丟進 Array.from，將 set集合的資料 轉化成 Array型態。(註：產生新陣列，不影響舊資料)
let result_05 = Array.from(new Set(source));

console.log("result_05", result_05); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```
<div class="note info">[MDN-Array.from()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
[MDN-Set()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)
[阮一峰-Set()](http://es6.ruanyifeng.com/#docs/set-map#Set)</div>

***
## Set() + Spread

`...` 為 ES6的展開運算子（spread operator），把一個陣列展開(expand)成個別數值

``` js 簡寫
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

//1. 將 source資料 丟進 new set，使其產生一個新的 set集合 ，並且已去除重覆的值
//2. 用 ...(展開運算子)，將 Set 轉換為 Array(註：...set外圈有個 中框號[]，用來轉換陣列型態)
let result_06 = [...new Set(source)];

console.log("result_06", result_06); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```

<span id="inline-green">補充說明</span>

`...` 只是將 `陣列`、`set` 的值拆解一個一個的值，並<font color="red">無額外產生新陣列</font>。

``` js 
let number = [1,2,3,4,5];
let mySet = new Set([1,2,3,4]);

console.log(...number);  // 1,2,3,4,5
console.log(...mySet);  // 1,2,3,4
```

<div class="note info">[PJ-...](https://pjchender.blogspot.tw/2017/01/es6-spread-operatorrest-operator.html)
[eddy-...](http://eddychang.me/blog/16-javascript/45-spread-operator-rest-parameters.html)</div>