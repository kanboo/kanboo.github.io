---
title: ES6-展開運算子(Spread Operator)、其餘參數(Rest Operator)
date: 2018-01-26 09:19:00
categories: 
- JS
- ES6
tags:
- JS
- ES6
- Spread Operator
- Rest Operator
---

{% cq %}

{% asset_img ES6.jpg %}

<font style="font-size:20px;">展開運算子(Spread Operator)、其餘參數(Rest Operator)</font>

{% endcq %}

<!-- more -->
***

## 展開運算子( Spread Operator )

展開運算子是"<font color="red">把一個陣列展開(expand)成個別數值</font>"的速寫語法，簡單來說，就是把陣列裡面的值，<font color="red">拆解成一個一個</font>。


``` js 基本用法
//陣列
let number = [1,2,3,4,5];
console.log(...number);  // 1,2,3,4,5

//字串
let str = 'kanboo'
console.log(...str);  // "k","a","n","b","o","o"
```


``` js 將二個Array合併(新舊寫法比較)
//===========過去寫法===========
let groupA = ['爸爸', '媽媽'];
let groupB = ['老大','老二','老么'];

const groupAll = groupA.concat(groupB);
console.log(groupAll);
// ["爸爸", "媽媽", "老大", "老二", "老么"];


//===========新的寫法===========
let groupA = ['爸爸', '媽媽'];
let groupB = ['老大','老二','老么'];

const groupAll = [...groupA, ...groupB];  //拆解完後，再用陣列[]包起來，產生新的陣列
console.log(groupAll);
// ["爸爸", "媽媽", "老大", "老二", "老么"];
```


你也可以用來把陣列展開，傳入函式之中，例如下面**加總函式**的範例：

``` js  將 陣列展開 傳入函式
function sum(a, b, c) {
  return a + b + c;
}

var args = [1, 2, 3];
console.log(sum(...args)); // 6
```

## 淺層複製

另外<font color="red">陣列</font>與<font color="red">物件</font>相同都有著<font color="red">傳參考</font>的特性，所以當把陣列賦予到另一個值上時，修改其中一個另一個也會跟著變動。


``` js 傳址：更動groupB 會 影響groupA
// 由於傳參考的關係，所以將一個陣列傳到另一個上時
// 兩個的值其實是一樣的
let groupA = ['老大','老二','老么'];
let groupB = groupA;
groupB.push('小花');

//輸出 groupA
console.log(groupA); // ['老大','老二','老么', '小花'];
```

由於 <font color="red">展開運算子</font> 它是<font color="red">一個一個將值寫入</font>，所以他也有<font color="red">淺層的複製</font>(shallow copy) 。

``` js 淺層複製：更動groupB 不會 影響groupA
// 這個屬於淺拷貝，所以不會影響到另一個物件
let groupA = ['老大','老二','老么'];
let groupB = [...groupA];
groupB.push('小花');

//輸出 groupA
console.log(groupA); // ['老大','老二','老么'];
```

## 類陣列轉成純陣列

JavaScript 中有許多<font color="red">類</font>陣列，這類陣列有著陣列的外皮，但卻<font color="red">不能使用陣列的方法</font>，這類陣列由於原型不同，所以 <font color="red">不能</font> 使用許多的陣列方法，如： map(), concat() 等等。

其中一種很常見的就是 <font color="red">DOM 陣列</font>，此時我們就可以用 <font color="red">展開運算子</font> 轉為 <font color="red">純陣列</font>，這樣就可以使用陣列的各種方法。

``` js
// 可以將類陣列轉成陣列
let doms = document.querySelectorAll('p');
console.log(doms);
let spreadDom = [...doms];
console.log(spreadDom);
```

<span id="inline-purple">範例 arguments</span>

以前要使用函式的 <font color="red">arguments</font> 時，寫法僅可用 <font color="red">for</font> 或 <font color="red">for..in</font> 依序取得參數值。

``` js arguments傳統寫法
function sum() {
    var total = 0;
    for( var i=0; i<arguments.length; i++ ) {
        total += arguments[i];
    }
    return total;
}

console.log( sum(1, 2, 3, 4, 5) ); // 15
```

但是若將 <font color="red">arguments</font> 改寫用 **陣列的方法**(<font color="red">forEach、map、reduce</font>) 的話，程式碼就會<font color="red">出錯</font>。

``` js 使用陣列方法，會Error
function sum() {
    var total = 0;
    arguments.forEach(function(element) {
        total += element;
    });
    return total;
}

console.log(sum(1, 2, 3, 4, 5));  //error TypeError: arguments.forEach is not a function
```

現在可透過 <font color="red">展開運算子</font> 轉為 <font color="red">純</font>陣列，就可使用 **陣列的方法**(<font color="red">forEach、map、reduce</font>)。

``` js 透過「展開運算子」轉換
function sum() {
    let arg = [...arguments];  //透過 展開運算子 來轉成純陣列
    let total = 0;
    arg.forEach(function(element) {
      total += element;
    });
    return total;
}

console.log(sum(1, 2, 3, 4, 5)); //15
```

## 其餘參數(Rest Operator)

有時候，Function 接受的參數數量<font color="red">不固定</font>，而 **其餘參數(Rest Operator)** 的功能就是把<font color="red">多的參數</font>併成<font color="red">一個 Array</font>。

<span id="inline-purple">範例1</span>

可傳入不固定的參數，<font color="red">1個 or 多個</font> 皆可..

``` js
function sum(...numbers) {
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}

//傳入一個值
console.log(sum(1)); // 1
//傳入多個值
console.log(sum(1, 2, 3, 4, 5)); // 15
```

<span id="inline-purple">範例2</span>

如果function有先定義別的參數，就會將傳入的參數值<font color="red">先給</font>定義好的參數，剩下的就全部塞入<font color="red">其餘參數</font>。

``` js
function restArray(x, y, ...others) {
  console.log("x",x);  // x： 1
  console.log("y",y);  // y： 2
  console.log("others",others);  // others： [3, 4, 5]
}

//拆解陣列
restArray(1, 2, 3, 4, 5); 
```

其餘參數(Rest parameters)有一個<font color="red">限制</font>，就是這個參數一定是函式的「<font color="red">最後一個</font>」。

你如果放在其餘的參數<font color="red">前</font>，就會<font color="red">產生錯誤</font>。

``` js 反過來寫就會Error
function restArray(...others, x, y) {
  console.log("x",x);
  console.log("y",y);
  console.log("others",others);
}

//拆解陣列
restArray(1, 2, 3, 4, 5); //error SyntaxError: Rest parameter must be last formal parameter
```


<div class="note primary">其餘的參數 與 arguments 不同的是：
- arguments 不是真的陣列，其餘參數則「<font color="red">是</font>」。
- arguments 不能混用自訂傳入的參數。</div>


***
## 參考來源
<div class="note info">[邁向 JavaScript 勇者之路](https://ithelp.ithome.com.tw/users/20083608/ironman/1354)
[Eddy-展開運算值、其餘參數](http://eddychang.me/blog/16-javascript/45-spread-operator-rest-parameters.html)</div>