---
title: ES6-箭頭函式 (Arrow functions)
date: 2018-01-23 22:47:20
categories: 
- JS
- ES6
tags:
- JS
- ES6
- 箭頭函式
- Arrow functions
---

{% cq %}

{% asset_img ES6.jpg %}

<font style="font-size:20px;">箭頭函式 () => {} </font>

{% endcq %}

<!-- more -->
***


## 簡短的語法

一般使用箭頭函式與 function 的用法大致一致，可以傳入參數、也有大括號包起來，
除此之外箭頭函式也有更簡短的寫法如下：

``` js
// 正常寫法
var callSomeone = (someone) => {
  return someone + '上工了'
}
console.log(callSomeone('伙計'))

// 縮寫，單一行陳述不需要 {}
var callSomeone = (someone) => someone + '上工了'
console.log(callSomeone('伙計'))

// 只有一個參數可以不加括號
var callSomeone = someone => someone + '上工了'
console.log(callSomeone('伙計'))

// 沒有參數時，一定要有括號
var callSomeone = () => '伙計' + '上工了'
console.log(callSomeone('伙計'))
```

<span id="inline-toc">注意</span>
不過這個上述有個小地方也要注意一下，在大括號內的 <font color="red">{}</font> 是需要自行加入 <font color="red">return</font>，
如果沒有傳入值則會出現 <font color="red">undefined</font>。

``` js
var callSomeone = (someone) => { someone + '上工了' }
console.log(callSomeone('伙計')) // undefined
```

***
## 綁定的 this 不同

口訣： 
箭頭函式裡面的this 等於 外面的this

白話文： 
箭頭函式裡的this 主要是依據 <font color="red">外層函式(function)裡的this</font> 是什麼就跟著是什麼。


``` js
function x(){
  this <= 外層函式的this，規則參考「一般函式的this」
  const a = () => {
    this <= 依據外層函式的this
  }
}
```

<span id="inline-purple">範例</span>

callName  是使用 一般函式
callName2 是使用 箭頭函式

``` js
var name = '全域阿婆'
var auntie = {
  name: '漂亮阿姨',
  callName: function () { 
    // 注意，這裡是 function，以此為基準產生一個作用域
    console.log('1', this.name); // 1 漂亮阿姨(外層函式)
    setTimeout(() => {
      console.log('2', this.name); // 2 漂亮阿姨(依據外層函式的this)
      console.log('3', this); // 3 auntie 這個物件(依據外層函式的this)
    }, 10);
  },
  callName2: () => { 
    // 注意，如果使用箭頭函式，this 依然指向 window
    console.log('4', this.name); // 4 全域阿婆(依據外層函式的this)
    setTimeout(() => {
      console.log('5', this.name); // 5 全域阿婆(依據外層函式的this)
      console.log('6', this); // 6 window 物件(依據外層函式的this)
    }, 10);
  }
}

auntie.callName();
auntie.callName2();
```
<div class="note primary">補充：
因為 <font color="red">callName2</font> 的 <font color="red">4</font> 使用箭頭函式，所以 <font color="red">依據外層函式的this</font> 的規則，就指向最外層window。</div>



***
## 沒有 arguments 參數

注意:箭頭函數裡沒有argument物件，可使用 <font color="red">其餘參數(Rest Operator)</font> 替代

``` js
const other = (...others) => others;
console.log(other(10, 20, 30, 40, 50)) // [10, 20, 30, 40, 50]
```

***
## 不可使用的情況

**apply, call, bind**

<font color="red">this</font> 在 Arrow function 中是被綁定的，所以套用 <font color="red">call</font> 的方法時是無法修改 <font color="red">this</font>。

***
## 不能用在建構式

由於 this 的是在物件下建立，所以箭頭函式不能像 function 一樣作為建構式的函式，
如果嘗試使用此方法則會出現錯誤 (<font color="red">... is not a constructor</font>)。


***
## 參考來源
<div class="note info">[邁向 JavaScript 勇者之路](https://ithelp.ithome.com.tw/users/20083608/ironman/1354)</div>