---
title: JS-物件、陣列以及型別判斷
date: 2018-01-15 22:33:14
categories: 
- JS
- 重新認識 JavaScript
tags:
- JS
- 重新認識 JavaScript
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">物件、陣列以及型別判斷。</font>

{% endcq %}

<!-- more -->
***

## 前言

在 2018 iT 邦幫忙鐵人賽，看到Kuro大的重新認識 JavaScript 系列文，仔細的閱讀後，紀錄自己觀念不足的部份，也非常推薦給大家觀看此系列文。

<div class="note info">[Kuro-重新認識 JavaScript 系列](https://ithelp.ithome.com.tw/users/20065504/ironman/1259?page=1)</div>

***
## 基本型別與物件型別

JavaScript 內建的型別主要可以分成<font color="blue">基本型別 (Primitives)</font> 與<font color="blue">物件型別 (Object)</font> 兩大類。
而基本型別又分成 <font color="red">string</font>、<font color="red">number</font>、<font color="red">boolean</font>、<font color="red">null</font>、<font color="red">undefined</font> 幾種，
除了以上幾種之外，其他都可以歸類至

<font color="blue">物件型別 (Object)</font>
<font color="red">物件型別 (Object)</font>
<font color="orange">物件型別 (Object)</font>

***
## 判斷屬性是否存在

方法1：物件中<font color="red">不存在</font>的屬性，此時會回傳 <font color="red">undefined</font>
方法2：<font color="red">hasOwnProperty()</font> 方法<font color="red">不會</font>往上檢查物件的原型鏈(prototype chain)，而 <font color="red">in</font> 運算子，則<font color="red">會</font>繼續往物件原型鏈上檢查。

<span id="inline-purple">方法1</span>

物件中<font color="red">不存在</font>的屬性，此時會回傳 <font color="red">undefined</font>

``` js 
var obj = {};

console.log( obj.name );      // undefined
```

但這麼做會有個例外，就是當該屬性剛好就是 <font color="red">undefined</font> 時，這招就沒用了。

除了檢查 undefined 之外，還有 <font color="red">in</font> 運算子 與 <font color="red">hasOwnProperty()</font> 方法。


<span id="inline-purple">方法2</span>

雖然兩者都可以檢查物件的屬性是否存在，
但<font color="red">hasOwnProperty()</font> 方法<font color="red">不會</font>往上檢查物件的原型鏈(prototype chain)，，只會檢查物件本身是否存在這個屬性，
而 <font color="red">in</font> 運算子，則<font color="red">會</font>繼續往物件原型鏈上檢查。

``` js 範例1
var obj = {
  name: 'Object'
};

// 透過 in 檢查屬性
console.log( 'name' in obj );     // true
console.log( 'value' in obj );    // false

// 透過 hasOwnProperty() 方法檢查
obj.hasOwnProperty('name');       // true
obj.hasOwnProperty('value');      // false
```

上述範例1，雖然二種檢查結果都一樣，但我們可看下列的範例2，當檢查某一屬性是在原型鋉上時，會有什麼不一樣

``` js 範例2
//toString 為 原型鍊上的 屬性

// 透過 in 檢查屬性
console.log( 'toString' in obj );     // true

// 透過 hasOwnProperty() 方法檢查
obj.hasOwnProperty('toString');       // false
```

***
## typeof: 型別判斷

檢查變數型別 (正確來講應該是值，變數沒有型別，值才有)，可以透過 <font color="red">typeof</font> 運算子來處理：

``` js
typeof  true;         // 'boolean'
typeof  'Kanboo';     // 'string'
typeof  123;          // 'number'
typeof  NaN;          // 'number'
typeof  { };          // 'object'
typeof  [ ];          // 'object'
typeof undefined;     // 'undefined'

typeof window.alert;  // 'function'
typeof null;          // 'object'
```

要注意的是，透過 <font color="red">typeof</font> 運算子回傳的東西都是「<font color="red">字串</font>」。


### typeof null 為什麼是 "object" ?

其實這只是一個 <font style="color:red;font-size:20px;">Bug</font> ，所以小心使用 `null` 。

> [參考1: Null and typeof](https://javascriptrefined.io/null-and-typeof-9330e475d272)
> [參考2: The history of “typeof null”](http://2ality.com/2013/10/typeof-null.html)

### 如何判別是否為陣列

當我們利用 <font color="red">typeof</font> 去檢查一個「<font color="red">陣列</font>」時，會得到 "<font color="red">object</font>" 的結果，

`typeof [ ];  // 'object'`

但如果在實務上仍會有需要判斷某變數是否為一個陣列而非物件的時候，可用 <font color="red">isArray()</font> 方法，

``` js 
Array.isArray([]);            // true
Array.isArray([1]);           // true
Array.isArray(new Array());   // true

Array.isArray();              // false
Array.isArray({});            // false
Array.isArray(null);          // false
Array.isArray(undefined);     // false
```
