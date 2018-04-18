---
title: JS30-14-JavaScript-References-VS-Copying
date: 2018-04-18 14:14:32
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js14.jpg %}

<font style="font-size:20px;">陣列與物件的<font color="red">傳址(reference)</font>及<font color="red">複製(Copying)</font>。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 了解陣列與物件的<font color="red">傳址(reference)</font>及<font color="red">複製(Copying)</font>。


## 成品

>[[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/14%20-%20JavaScript%20References%20VS%20Copying/index.html)


***
## JS學習紀錄

### 型別介紹

基本型別：<font color="red">string、number、boolean、null、undefined</font>
除了以上幾種之外，其他都可以歸類至<font color="red">物件型別 (Object)</font>

基本型別 => 傳<font color="red">值(value)</font>
物件型別 => 傳<font color="red">址(reference)</font>

### 陣列複製

列出可完成陣列複製的方式

- `Array.prototype.Slice()`
``` js
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
const team2 = players.slice();
```

- `Array.prototype.concat()`
``` js
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
const team3 = [].concat(players);
```

- `ES6 展開運算子( Spread Operator )`
``` js
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
const team4 = [...players];
```

- `Array.from()`
``` js
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
const team5 = Array.from(players);
```

### 物件複製

列出可完成物件複製的方式，以及<font color="red">有雷</font>的地方。

- `Object.assign()`
``` js 淺拷貝
const person = {
  name: 'Wes Bos',
  age: 80
};

const cap2 = Object.assign({}, person);
```

- `JSON.parse()` & `JSON.stringify()`
``` js JSON轉換
const wes = {
  name: 'Wes',
  age: 100,
  social: {
    twitter: '@wesbos',
    facebook: 'wesbos.developer'
  }
};

// Object.assign 只能淺複製一層，若第二層以上依舊是 傳址(reference)
const dev = Object.assign({}, wes);

// 透過JSON轉字串後，利用傳值的特性複製給新變數後，然後轉回物件型態，達到可複製二層以上的物件。
const dev2 = JSON.parse(JSON.stringify(wes));
```


<span id="inline-purple">特例：</span>
像 <font color="red">function</font> 沒辦法轉成 JSON 再轉回來，複製的function會直接消失

``` js JSON轉換失敗案例
var obj1 = {
  fun: function(){
    console.log(123)
    }
};
var obj2 = JSON.parse(JSON.stringify(obj1));

console.log(typeof obj1.fun); // 'function'
console.log(typeof obj2.fun); // 'undefined' <-- 沒複製
```

***
<div class="note info">舊文參考：
[JS-淺拷貝(Shallow Copy) VS 深拷貝(Deep Copy)](https://kanboo.github.io/2018/01/27/JS-ShallowCopy-DeepCopy/)
[JS-陣列(Array)-淺拷貝作法](https://kanboo.github.io/2018/02/02/JS-Array/)</div>