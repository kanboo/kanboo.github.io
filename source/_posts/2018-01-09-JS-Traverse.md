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

{% asset_img js_01.jpg %}

<font style="font-size:20px;">紀錄各種<font color="red">迴圈</font>的用法。</font>

{% endcq %}

<!-- more -->
***

## 前言

JS常常需要針對 <font color="blue">陣列(Array)</font>、<font color="blue">物件(Object)</font> 這二種資料進行分析及修改，
故整理下列幾種方法，

- 單純遍歷陣列元素
    - for
    - forin
    - forEach

- 判斷是否符合資格
    - every：遍歷每個元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">不符</font>，就回傳 `false`
    - some： 遍歷每個元素<font color="blue">判斷</font>是否皆符合條件，若其一<font color="red">符合</font>，就回傳 `true`

- 產生新的陣列
    - map：遍歷每個元素，進行 <font color="red">加工、校正</font>
    - filter：遍歷每個元素，保留<font color="red">符合條件(true)</font>的值，不符合，則去除掉。
    - reduce：遍歷每個元素，依序<font color="red">組合、加總</font>，然後丟給下個元素，最終會回傳一個結果。

***
## for

最常見的用法，不多加說明。

<span id="inline-purple">範例</span>

``` js
let arr = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];

for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

***
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

<span id="inline-blue">語法</span>

``` js
arr.forEach( callback(currentValue, index, array){
    /* your code */
});
```

callback
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 forEach() 方法的陣列。


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

<span id="inline-blue">語法</span>

``` js
arr.every( callback(currentValue, index, array){
    /* your code */
});
```

callback
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 every() 方法的陣列。

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

<span id="inline-blue">語法</span>

``` js
arr.some( callback(currentValue, index, array){
    /* your code */
});
```

callback
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 some() 方法的陣列。

<span id="inline-purple">範例</span>

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

<div class="note warning">原先的陣列與後來新產生出的陣列，<font color="red">個數(Array.length)</font> 會一樣多。</div>

<span id="inline-blue">語法</span>

``` js
arr.map( callback(currentValue, index, array){
    /* your code */
});
```

callback
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 map() 方法的陣列。

<span id="inline-purple">範例1</span>


``` js 加工
var myArr = [ 1, 2, 3 ];

var newArr = myArr.map(function(element) {
    // 每個數都加1
    return element + 1;
});

console.log(newArr); // [ 2, 3, 4 ]
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

console.log(newArr); // [1, 100, 3, 100, 100, 6, 7, 100, 9, 100]
```

<div class="note info">[MDN-map()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/map)</div>


***
## filter

遍歷每個元素，保留<font color="red">符合條件(true)</font>的值，不符合，則去除掉。

<div class="note warning">原先的陣列與後來新產生出的陣列，<font color="red">個數(Array.length)</font>可能不一樣多。</div>

<span id="inline-blue">語法</span>

``` js
arr.filter( callback(currentValue, index, array){
    /* your code */
});
```

callback
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 filter() 方法的陣列。

<span id="inline-purple">範例</span>

``` js
var myArr = [ 1, 20, 3, 40, 50, 6, 7, 80, 9, 10 ];

var newArr = myArr.filter(function(element) {
    // 取得大於50的數
    return element >= 50;
});

console.log(newArr); // [50, 80]
```

<div class="note info">[MDN-filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)</div>

***
## reduce

遍歷每個元素，依序組合、加總，然後丟給下個元素，最終會回傳一個結果。

<span id="inline-blue">語法</span>

``` js
arr.reduce( callback(accumlator, currentValue, index, array){
    /* your code */
}, initialValue)
```

callback
- accumulator(<font color="red">常用</font>)：用來累積回呼函式回傳值的累加器。
- currentValue(<font color="red">常用</font>)：原陣列目前所迭代處理中的元素。
- index(選用)：原陣列目前所迭代處理中的元素之<font color="blue">索引</font>。
- array(選用)：呼叫 reduce() 方法的陣列。

initialValue(<font color="red">常用</font>)：於第一次呼叫 callback 時要傳入的累加器初始值。


<span id="inline-purple">範例1</span>

注意reduce第二個參數 `0` , 代表的意思是開始執行遍歷前的<font color="red">初始值</font>。

``` js 有設定初始值
var myArr = [ 1, 2, 3 ];

// 處理每個元素後等待回傳結果，第一次處理時代入初始值 0
var result = myArr.reduce(function(prev, element) {
    console.log(prev,element);
    
    // 與之前的數值加總，回傳後代入下一輪的處理
    return prev + element;
}, 0);

/* 
prev,element 的執行log，共執行三次
第一次：0 1
第二次：1 2
第三次：3 3
*/


// 最終結果
console.log(result); // 6
```

<span id="inline-purple">範例2</span>

注意此次<font color="red">沒有設定</font>reduce第二個參數, 此時就會自動抓取陣列<font color="red">第一個元素</font>當作初始值。

``` js 無初始值
var myArr = [ 1, 2, 3 ];

// 處理每個元素後等待回傳結果，第一次處理時代入初始值 0
var result = myArr.reduce(function(prev, element) {
    console.log(prev,element);
    
    // 與之前的數值加總，回傳後代入下一輪的處理
    return prev + element;
});

/* 
prev,element 的執行log，共執行二次
第一次：1 2
第二次：3 3
*/


// 最終結果
console.log(result); // 6
```

<span id="inline-purple">範例3</span>

除了一般數字的加總，也可配合判斷式，最終產出一個新的陣列。

``` js 
let source = ["Kanboo", "Jack", "Rabbit", "Lucas", "Jack", "Lucas", "Rabbit"];
let result_04 = source.reduce((p, c) => {
    //includes 判斷是否已存在
    if (!p.includes(c)) p.push(c);
	console.log("p", p);
    return p;
}, []);


/*
迴圈的執行log，共執行七次

p ["Kanboo"]
p ["Kanboo", "Jack"]
p ["Kanboo", "Jack", "Rabbit"]
p ["Kanboo", "Jack", "Rabbit", "Lucas"]
p ["Kanboo", "Jack", "Rabbit", "Lucas"]
p ["Kanboo", "Jack", "Rabbit", "Lucas"]
p ["Kanboo", "Jack", "Rabbit", "Lucas"]
 */

//最終結果
console.log("result_04", result_04); // ["Kanboo", "Jack", "Rabbit", "Lucas"]
```

<div class="note info">[MDN-Reduce()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)</div>
