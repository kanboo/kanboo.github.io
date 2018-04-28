---
title: JS-淺拷貝(Shallow Copy) VS 深拷貝(Deep Copy)
date: 2018-01-27 08:37:20
categories:
- JS
tags:
- JS
- Shallow Copy
- Deep Copy
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;"> 淺拷貝(Shallow Copy) VS 深拷貝(Deep Copy) </font>

{% endcq %}

<!-- more -->
***


## 基本與物件型別傳值的差異

{% asset_img js_02.png %}

JavaScript 內建的型別主要可以分成<font color="blue">基本型別 (Primitives)</font> 與<font color="blue">物件型別 (Object)</font> 兩大類。

而基本型別又分成 <font color="red">string</font>、<font color="red">number</font>、<font color="red">boolean</font>、<font color="red">null</font>、<font color="red">undefined</font> 幾種，
除了以上幾種之外，其他都可以歸類至<font color="red">物件型別 (Object)</font>

這二種型別之間的差異，就是在他們的<font color="red">傳值方式</font>

- 基本型別 => 傳**值**(value)
- 物件型別 => 傳**址**(reference)

下面範例來解說他們的差異

<span id="inline-purple">基本型別</span>

基本型別是傳 <font color="red">value</font>

``` js 基本型別，傳value
let a = "爸爸";
let b = a;

b = "媽媽";

console.log(a); //爸爸
console.log(b); //媽媽
```

在修改 **b** 時並不會改到 **a** 的值

<span id="inline-purple">物件型別</span>

但<font color="red">物件</font>就不同，物件傳的是 <font color="red">reference</font>

``` js 物件型別，傳reference
let objA = { name: '王大頭' }
let objB = objA

objB.name = '盧卡斯'

console.log(objA); //"盧卡斯"
console.log(objB); //"盧卡斯"
```

***
## 淺拷貝(Shallow Copy) VS 深拷貝(Deep Copy)

{% asset_img js_03.png %}

- <font color="red">淺</font>拷貝：
只能達到淺層的複製(<font color="blue">第一層</font>)，若有<font color="red">第二層</font>以上的資料的話，
就無法達到實際的複製，而是會與舊物件一起<font color="red">共用</font>同一塊記憶體。

- <font color="red">深</font>拷貝：
會另外創造一個一模一樣的物件，新物件跟原物件<font color="red">不</font>共用記憶體，修改新物件不會改到原物件。

***
## Object.assign

Object.assign 是 ES6 的新函式，我們可以用來達成複製的功能

``` js 複製物件
let obj = {name: '王康寶', age:{child: 18}}
let copy = Object.assign({}, obj);

//更改 copy.name 的值
copy.name = '盧卡斯';

//輸出
console.log(obj);  //{name: "王康寶", age:{child: 18}}
console.log(copy); //{name: "盧卡斯", age:{child: 18}}
```

<font style="color: red;font-size:22px;"> But </font>Object.assign 並不是那麼的完美，請再看下例

``` js 淺拷貝
let obj = {name: '王康寶', age:{child: 18}}
let copy = Object.assign({}, obj);

copy.name = '盧卡斯';
copy.age.child = 99; //更改 copy.age.child 的值

//輸出
console.log(obj);  //{name: "王康寶", age:{child: 99}}
console.log(copy); //{name: "盧卡斯", age:{child: 99}}
```

我們可以看到更改 <font color="red">**copy**</font>.age.child 值以後，發現 <font color="red">**obj**</font>.age.child 值也跟著被變掉了，
所以 Object.assign 能處理深度，只有<font color="red">一層</font>的物件，沒辦法做到真正的 <font color="red">深拷貝(Deep Copy)</font>，
不過如果要複製的物件只有一層的話可以考慮使用他。

另外也可以使用 <font color="red">展開運算子(Spread Operator)</font> 達成複製，不過一樣是 <font color="red">淺層的複製</font>。

``` js 展開運算子( Spread Operator )
let obj = {name: '王康寶', age:{child: 18}}
let copy = {...obj};  //展開運算子(Spread Operator)

copy.name = '盧卡斯';
copy.age.child = 99; //更改 copy.age.child 的值

//輸出
console.log(obj);  //{name: "王康寶", age:{child: 99}}
console.log(copy); //{name: "盧卡斯", age:{child: 99}}
```

***
## 深拷貝的作法

### jQuery

jquery 有提供一個 <font color="red">$.extend</font> 可以用來做 Deep Copy

``` js jquery.extend
let obj = {name: '王康寶', age:{child: 18}}
let copy = $.extend(true, {}, obj); //使用 jquery.extend

copy.name = '盧卡斯';
copy.age.child = 99; //更改 copy.age.child 的值

//輸出
console.log(obj);  //{name: "王康寶", age:{child: 18}}
console.log(copy); //{name: "盧卡斯", age:{child: 99}}
```

### lodash

lodash 也有提供 <font color="red">_.cloneDeep</font> 用來做 Deep Copy

``` js lodash.cloneDeep
let obj = {name: '王康寶', age:{child: 18}}
let copy = _.cloneDeep(obj);  //使用 lodash.cloneDeep

copy.name = '盧卡斯';
copy.age.child = 99; //更改 copy.age.child 的值

//輸出
console.log(obj);  //{name: "王康寶", age:{child: 18}}
console.log(copy); //{name: "盧卡斯", age:{child: 99}}
```

***
## 參考來源
<div class="note info">[關於 JS 中的淺拷貝和深拷貝](http://larry850806.github.io/2016/09/20/shallow-vs-deep-copy/)
[js中的深拷贝和浅拷贝](https://www.jianshu.com/p/70dc5b968767)</div>

***
## FB討論串

2018/04/26 社團剛好有人詢問此問題，也有大大回覆一些見解，留個紀錄可以回頭查。

簡單重點整理：

- 可用 `JSON.parse(JSON.stringify({}))` 偽深拷貝
 - 純資料，可行。
 - 若遇 Function、Set、Map..等型態，失效。
- 為何要複製function? 目的？
 - 站在節省記憶體的角度，function能重複利用就重複利用
 - 若是要寫物件導向風格
   - ES5：寫 Function + prototype
   - ES6：寫 Class

<span id="inline-yellow">JSON複製範例</span>

``` js JSON複製範例
let a = {o:{v:1}}
let b = JSON.parse(JSON.stringify(a));
```
<div class="note primary">[淺拷貝與深拷貝??](https://www.facebook.com/groups/f2e.tw/permalink/1635876266449732)</div>
