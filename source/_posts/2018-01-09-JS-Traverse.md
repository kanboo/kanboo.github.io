---
title: JS-各種迴圈的用法
date: 2018-01-09 11:36:41
categories: 
- JS
tags:
- JS
- for
- forin
- forEach
- every
- some
- map
- filter
- reduce
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">紀錄一下 各種<font color="red">迴圈</font>的用法。</font>

{% endcq %}

<!-- more -->
***

## 前言

JS常常需要針對 <font color="blue">陣列(Array)</font>、<font color="blue">物件(Object)</font> 這二種資料進行分析及修改，
故整理下列幾種方法，

單純遍歷陣列元素

- for：
- forin：
- forEach：

產生新的陣列

- map：遍歷每個元素，進行 <font color="red">加工、校正</font>
- every：遍歷每個元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">不符</font>，就回傳 `false`
- some： 遍歷每個元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">符合</font>，就回傳 `true`
- filter：遍歷每個元素，當符合條件(true)時，目前的值會保留在陣列內，不符合，則去除掉不保留。
- reduce：

## for

最常見的用法，不多加說明。

<span id="inline-purple">範例</span>

``` js
let arr = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

## forin

可傳入 `objec` 或 `array` 型態的資料。


<span id="inline-purple">範例</span>

``` js Array型態
var arr = ["zero","one","two"];

for (var key in arr) {
    console.log(arr[key]);
}
```

``` js Object型態
var obj = {a: 1, b: 2, c: 3};
    
for (const item in obj) {
  console.log(obj[item]);
}
```

<div class="note info">[MDN-for...in](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/for...in)</div>


***
## forEach

可用來取代 `for` 的寫法，讓程式碼更簡短，省略掉定義 `i`、`arr.length`…等動作。

<span id="inline-purple">範例</span>

``` js ES5
const arr = ['a', 'b', 'c'];

arr.forEach(function(element) {
    console.log(element);
});
```

``` js ES6
const arr = ['a', 'b', 'c'];

arr.forEach((element) => {
    console.log(element);
});
```

<div class="note info">[MDN-forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)</div>


***
## every

對每個陣列元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">不符</font>，就回傳 `false`

<span id="inline-purple">範例</span>

``` js
function isBigEnough(element, index, array) {
    //是否大於10
    return element >= 10;
}

// 其中 5 沒有大於 10
[12, 5, 8, 130, 44].every(isBigEnough);   // false

// 皆符合大於 10
[12, 54, 18, 130, 44].every(isBigEnough); // true
```

<div class="note info">[MDN-every()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/every)</div>


***
## some

對每個陣列元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">符合</font>，就回傳 `true`

``` js
function isBiggerThan10(element, index, array) {
    //是否大於10
    return element > 10;
}

// 全部都無 大於10
[2, 5, 8, 1, 4].some(isBiggerThan10);  // false

// 其中 12 有大於10
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
```

<div class="note info">[MDN-some()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/some)</div>


***
## map

可對每個陣列元素進行 <font color="red">加工、校正</font> 的處理，最後回傳一個 <font color="red">新的陣列</font>。

<div class="note warning">原始陣列的<font color="red">個數(Array.length)</font>，產生出 新的陣列個數 也會是一樣的。</div>


<span id="inline-purple">範例1</span>

幫每個元素 +1(加工)。

``` js 加工
var myArr = [ 1, 2, 3 ];

var newArr = myArr.map(function(element) {
    return element + 1;
});

// [ 2, 3, 4 ]
console.log(newArr);
```

<span id="inline-purple">範例2</span>

``` js 校正
var myArr = [ 1, 20, 3, 40, 50, 6, 7, 80, 9, 10 ];

var newArr = myArr.map(function(element) {
    // 校正大於10的數字，統一變100
    if (element < 10){
        return element;
    }else{
        return 100;
    }   
});

console.log(newArr);
```

***
## filter

歷每個元素，當符合條件(true)時，目前的值會保留在陣列內，不符合，則去除掉不保留。


***
## reduce

遍歷每個元素，依序組合、加總，然後丟給下個元素，最終會回傳一個結果。