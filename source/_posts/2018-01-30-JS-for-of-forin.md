---
title: JS-for...in與for...of的差別
date: 2018-01-30 23:01:02
categories: 
- JS
tags:
- JS
- ES6
- for of
- for in
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">for...of與for...in的差別</font>

{% endcq %}

<!-- more -->
***

## 總結

1. 建議：在迭代<font color="red">物件屬性</font>時，使用 `for...in`；在迭代<font color="red">陣列</font>時，使用 `for...of`。
2. `for...in` 輸出的是<font color="red">屬性名稱(key)</font>，`for...of` 輸出的是<font color="red">值(value)</font>
3. `for...of` 是 ES6 的新語法。修復了ES5 for...in 的不足
4. `for...of` <font color="red">不能</font>迭代<font color="red">物件</font>，需要透過和 Object.keys() 搭配使用

## 範例說明

參考下列三個範例變化與說明，即可了解 `for...of` 與 `for...in` 之間的差別

<span id="inline-purple">範例1</span>

單純迭代陣列的話，`for...in` 輸出的是<font color="red">屬性名稱(key)</font>，`for...of` 輸出的是<font color="red">值(value)</font>

``` js 
let iterable = [3, 5, 7];

// 回傳「key」
for (let i in iterable) {
  console.log(i); // "0", "1", "2"
}

// 回傳「value」
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```

<span id="inline-purple">範例2</span>

再來我們在原本的陣列，新增一個屬性 `foo`，可看到 `for...in` 有將此屬性 `foo` 也輸出。

``` js 新增陣列的屬性
let iterable = [3, 5, 7];
iterable.foo = 'hello';   //新增foo屬性名稱

// 回傳「key」，且會讀取到陣列新增的屬性名稱
for (let i in iterable) {
  console.log(i); // "0", "1", "2", "foo"
}

// 回傳「value」
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```

<span id="inline-purple">範例3</span>

再來我們這次在**物件和陣列**的<font color="red">原型鍊</font>上，分別各新增function，一樣可看到 `for...in` 也將原型鋉上的function名稱也輸出了。

``` js 在 原型鍊上 新增function
// 在 原型鍊上 新增function
Object.prototype.objCustom = function(){}; //物件 原型鋉
Array.prototype.arrCustom = function(){};  //陣列 原型鋉

let iterable = [3, 5, 7];
iterable.foo = 'hello';

// 回傳「key」，且同時會讀取到 物件、陣列 原型鍊上的function
for (let i in iterable) {
  console.log(i); // "0", "1", "2", "foo", "arrCustom", "objCustom"
}

// 回傳「值」
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```

由上面三個案例來看，如果只是想單純的迭代取出陣列<font color="red">值</font>的話，建議使用 `for...of` 會比較好，不過注意此語法為 ES6 新語法。


***
## for..of 迭代 物件(object)

如果想用 `for...of` 來遍歷物件的屬性的話，可以通過和 `Object.keys()` 搭配使用，先取得物件的所有key的數組，然後再遍歷。

``` js
var student={
    name:'kanboo',
    age:16,
    locate:{
        country:'tw',
        city:'taipei',
        school:'CCC'
    }
}

for(var key of Object.keys(student)){
    //使用Object.keys()方法取得物件的Key的陣列
    console.log(key+": "+student[key]);
}

// "name: kanboo"
// "age: 16"
// "locate: [object Object]"
```

***
## 參考來源
<div class="note info">[MDN-for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
[javascript-for of和for in的區別？](https://segmentfault.com/q/1010000006658882)</div>