---
title: JS-Pass by sharing
date: 2018-01-15 23:18:39
categories: 
- JS
- 重新認識 JavaScript
tags:
- JS
- 重新認識 JavaScript
- Pass by sharing
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">JavaScript 是「傳值」或「傳址」？</font>

{% endcq %}

<!-- more -->
***

## 前言

在 2018 iT 邦幫忙鐵人賽，看到Kuro大的重新認識 JavaScript 系列文，仔細的閱讀後，紀錄自己觀念不足的部份，也非常推薦給大家觀看此系列文。

<div class="note info">[Kuro-重新認識 JavaScript 系列](https://ithelp.ithome.com.tw/users/20065504/ironman/1259?page=1)</div>

***
## 「傳值」或「傳址」？
所以我說那個 JavaScript 是「傳值」或「傳址」呢？

在大多數的情況下，基本型別是「<font color="red">傳值</font>」，而物件型別會是「<font color="red">傳址</font>」的方式，但凡事都有例外。

我們來看看下面這個例子：

``` js
var coin1 = { value: 10 };

function changeValue(obj) {
  obj = { value: 123 };
}

changeValue(coin1);
console.log(coin1);   // ？
```

猜猜看，經過 <font color="red">changeValue(coin1)</font> 操作後的 <font color="red">coin1</font> 會是什麼？

答案仍是 <font color="red">{ value: 10 }</font> 。

剛剛說過，物件型別會是「傳址」的方式來更新資料，那應該會是 <font color="red">{ value: 123 }</font> 才對，為什麼依然不變？

事實上，JavaScript 不屬於單純的傳值或傳址。
更準確一點來說，JavaScript 應該屬於透過 <font color="red">pass by sharing</font> 來傳遞資料。

「傳值」或「傳址」對大多數的開發者來說應該都不陌生，那麼「<font color="red">pass by sharing</font>」又是什麼呢？


***
## Pass by sharing

「Pass by sharing」的特點在於，
當 <font color="red">function</font> 的參數，如 function changeValue(obj){ ... } 中的 <font color="red">obj</font> 被<font color="red">重新賦值</font>的時候，
外部變數的內容是不會被影響的。

``` js function裡，重新賦值
var coin1 = { value: 10 };

function changeValue(obj) {
  obj = { value: 123 };
}

changeValue(coin1);
console.log(coin1);   // 此時 coin1 仍是 { value: 10 }
```

如果不是重新賦值的情況，則又會回到大家所熟悉的狀況：

``` js function裡，無 重新賦值
var coin1 = { value: 10 };

function changeValue(obj) {
  // 僅更新 obj.value，並未重新賦值
  obj.value = 123;
}

changeValue(coin1);
console.log(coin1);   // 此時 coin1 則會變成 { value: 123 }
```

<div class="note primary">[JS中是 pass by value 还是 pass by reference](https://objcer.com/2017/02/26/js-pass-by-value-or-by-reference/)</div>