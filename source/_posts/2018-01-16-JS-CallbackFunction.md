---
title: JS-Callback Function
date: 2018-01-16 22:41:30
categories: 
- JS
- 重新認識 JavaScript
tags:
- JS
- 重新認識 JavaScript
- Callback Function
- Callback Hell
- Promise
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">Callback Function & Promise</font>

{% endcq %}

<!-- more -->
***

## 前言

在 2018 iT 邦幫忙鐵人賽，看到Kuro大的重新認識 JavaScript 系列文，仔細的閱讀後，紀錄自己觀念不足的部份，也非常推薦給大家觀看此系列文。

<div class="note info">[Kuro-重新認識 JavaScript 系列](https://ithelp.ithome.com.tw/users/20065504/ironman/1259?page=1)</div>

***
## Callback Function

概念就如同：

_**辦公室電話響了 (事件被觸發 Event fired) -> 接電話 (處理事件 Event Handler)**_

而寫成程式碼就類似：

``` js 
// 註：這裡只是比喻，並沒有電話響這個事件 XD
Office.addEventListener( '電話響', function(){ /* 接電話 */ }, false);
```

可以看到，<font color="red">Office</font> 透過 <font color="red">addEventListener</font> 方法註冊了一個事件，當這個事件被觸發時，它會去執行我們所指定的第二個參數，也就是某個「函式」(接電話)。

換句話說，這個函式只會在滿足了某個條件才會被動地去執行，我們就可以說這是一個 Callback function。

***
## 波動拳 (a.k.a. "Callback Hell")

除了事件以外，還有另一個會需要用到 Callback function 的場景，就是「控制多個函式間執行的順序」。

下面舉例從簡單的事情慢慢演變成複雜時，會發生什麼情形

這裡定義了兩個 <font color="red">function</font>：

``` js
var funcA = function(){
  console.log('function A');
};

var funcB = function(){
  console.log('function B');
};

funcA();
funcB();
```

因為 <font color="red">funcA</font> 與 <font color="red">funcB</font> 都會立即執行，所以執行結果必定為：

``` js
"function A"
"function B"
```


但是，假設我們改成這樣，加上一個<font color="red">隨機生成的等待時間</font>：

``` js
var funcA = function(){
  var i = Math.random() + 1;

  window.setTimeout(function(){
    console.log('function A');
  }, i * 1000);
};


var funcB = function(){
  var i = Math.random() + 1;

  window.setTimeout(function(){
    console.log('function B');
  }, i * 1000);
};

funcA();
funcB();
```

這時候就沒辦法確定是 <font color="red">"function A"</font> 會先出現還是 <font color="red">"function B"</font> 會先出現了對吧？

像這種時候，為了確保執行的順序，就會透過 Callback function 的形式來處理：

``` js
// 為了確保先執行 funcA 再執行 funcB
// 我們在 funcA 加上 callback 參數
var funcA = function(callback){
  var i = Math.random() + 1;

  window.setTimeout(function(){
    console.log('function A');

    // 如果 callback 是個函式就呼叫它
    if( typeof callback === 'function' ){
      callback();
    }

  }, i * 1000);
};

var funcB = function(){
  var i = Math.random() + 1;

  window.setTimeout(function(){
    console.log('function B');
  }, i * 1000);
};

// 將 funcB 作為參數帶入 funcA()
funcA( funcB );
```

像這樣，無論 <font color="red">funcA</font> 在執行的時候要等多久， <font color="red">funcB</font> 都會等到 <font color="red">console.log('function A');</font> 之後才執行。

不過需要注意的是，當函式之間的相依過深，callback 多層之後產生的「波動拳」維護起來就會很可怕！

``` js
getData(function (a) {
  getMoreData(a, function (b) {
    getMoreData(b, function (c) {
      getMoreData(c, function (d) {
        getMoreData(d, function (e) {
          ...
        });
      });
    });
  });
});
```

{% asset_img js_02.jpg %}

***
## 再見 Callback Hell



執行順序的問題是一個，還有另一個常見的狀況是這樣，再回到 「Overcooked」 的場景。

<div class="note info">用 Overcooked (遊戲名稱：煮過頭)  比喻 同步(Synchronous)的概念
假設邊緣人如我，只能自己一人玩 Overcooked，在領完食材原料之後，一樣會有青菜、番茄需要處理。

因為只有<font color="red">一個廚師</font>，所以要嘛先處理青菜、要嘛先處理番茄，必須先弄完一項之後再去處理另一項，整個流程會被前一個步驟卡住。

像這樣「<font color="red">先完成 A 才能做 B、C、D ...</font>」的運作方式我們就會把它稱作「<font color="red">同步</font>」(Synchronous) 。</div>

當我要確保「切青菜、切番茄、擺盤」三個動作<font color="red">都完成</font>之後，我才能繼續「<font color="red">上菜</font>」這個動作。 

在面臨這種問題的時候，我要怎麼確保三個動作都完成之後，才繼續執行後面的程式呢？

最直覺的方式是新增一個變數來管理狀態：

``` js
var result = []; //紀錄已完成的事件
var step = 3;    //全部完成的總數量

// 假設 funcA、funcB、funcC 分別代表「切青菜、切番茄、擺盤」
function funcA(){
  window.setTimeout(function(){
    result.push('A'); //紀錄 A 完成
    console.log('A');

    if( result.length === step ){
      funcD();
    }

  }, (Math.random() + 1) * 1000);
}

function funcB(){
  window.setTimeout(function(){
    result.push('B'); //紀錄 B 完成
    console.log('B');

    if( result.length === step ){
      funcD();
    }
  }, (Math.random() + 1) * 1000);
}

function funcC(){
  window.setTimeout(function(){
    result.push('C');  //紀錄 C 完成
    console.log('C');

    if( result.length === step ){
      funcD();
    }
  }, (Math.random() + 1) * 1000);
}

function funcD(){
  console.log('上菜！');
  result = [];
}

funcA();
funcB();
funcC();
```


像上面這樣，當我們依序執行了 <font color="red">funcA()</font>、<font color="red">funcB()</font>、<font color="red">funcC()</font>，由於內部 <font color="red">setTimeout</font> 會等待亂數時間的關係，我們無法得知誰先誰後。 但可以確定的是，當這三個函式執行的時候就會去檢查 <font color="red">result.length === step</font> ，如果成立，就表示三個任務都已經完成，那麼就可以再去呼叫 <font color="red">funcD</font> 執行後續的事情。

如果不希望使用<font color="blue">全域變數來污染執行環境</font>的話，甚至可以包裝成一個通用的函式：

``` js 閉包的概念
function serials(tasks, callback) {
  var step = tasks.length;
  var result = [];

  // 檢查的邏輯寫在這裡
  function check(r) {
    result.push(r);
    if( result.length === step ){
      callback();
    }
  }

  tasks.forEach(function(f) {
    f(check);
  });
}
```

那麼改寫一下 <font color="red">funcA()</font>、<font color="red">funcB()</font>、<font color="red">funcC()</font>:

``` js
function funcA(check){
  window.setTimeout(function(){
    console.log('A');
    check('A');
  }, (Math.random() + 1) * 1000);
}

function funcB(check){
  window.setTimeout(function(){
    console.log('B');
    check('B');
  }, (Math.random() + 1) * 1000);
}

function funcC(check){
  window.setTimeout(function(){
    console.log('C');
    check('C');
  }, (Math.random() + 1) * 1000);
}

function funcD(){
  console.log('上菜！');
}
```

最後呼叫的時候，我們就可以透過這樣呼叫 <font color="red">serials()</font> ：

``` js
serials([funcA, funcB, funcC], funcD);
```

把想要提前執行的函式以<font color="red">陣列的方式</font>傳進 <font color="red">serials()</font> 作為第一個參數，當<font color="red">陣列中的函式都執行完畢</font>後，才會呼叫第二個參數的 <font color="red">funcD()</font>。

***
## Promise 物件

為了解決同步/非同步的問題，自從 <font color="red">ES6</font> 開始新增了一個叫做 <font color="red">Promise</font> 的特殊物件。

簡單來說，<font color="red">Promise</font> 按字面上的翻譯就是「承諾、約定」之意，回傳的結果要嘛是「**完成**」，要嘛是「**拒絕**」。

實際寫成 Promise 的程式碼大概像這樣：

``` js
const myFirstPromise = new Promise((resolve, reject) => {
  resolve(someValue);         // 完成
  reject("failure reason");   // 拒絕
});
```

要提供一個函式 promise 功能，讓它回傳一個 promise 物件即可：

``` js
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    // resolve() or reject()
  });
};
```

當 Promise 被<font color="red">完成</font>的時候，我們就可以呼叫 <font color="red">resolve()</font>，然後將取得的資料傳遞出去。 或是說想要<font color="red">拒絕</font> Promise ， 那麼就呼叫 <font color="red">reject()</font> 來拒絕。


***
一般來說， <font color="red">Promise</font> 物件會有這幾種狀態：

- pending: 初始狀態，不是 fulfilled 或 rejected。
- fulfilled: 表示操作成功地完成。
- rejected: 表示操作失敗。

{% asset_img js_03.png %}

整個 <font color="red">Promise</font> 流程可以用這張圖表示：

{% asset_img js_04.png %}

***

如果我們需要<font color="red">**依序**</font>串連執行多個 promise 功能的話，可以透過 <font color="red">.then()</font> 來做到。

以剛剛的 <font color="red">funcA()</font>、<font color="red">funcB()</font>、<font color="red">funcC()</font> 來當範例，我們將這三個函式分別透過 Promise 包裝：

``` js
function funcA(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('A');
      resolve('A'); // 完成
    }, (Math.random() + 1) * 1000);
  });
}

function funcB(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('B');
      resolve('B'); // 完成
    }, (Math.random() + 1) * 1000);
  });
}

function funcC(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('C');
      resolve('C'); // 完成
    }, (Math.random() + 1) * 1000);
  });
}
```

最後透過呼叫

``` js
funcA().then(funcB).then(funcC);
```

就可以做到等 <font color="red">funcA()</font> 被 「**resolve**」之後再執行 <font color="red">funcB()</font>，然後 resolve 再執行 <font color="red">funcC()</font> 的順序了。

***

如果我們不在乎 <font color="red">funcA()</font>、<font color="red">funcB()</font>、<font color="red">funcC()</font> 誰先誰後，只關心這三個是否已經完成呢？

那就可以透過 <font color="red">Promise.all()</font> 來做到：

``` js Promise.all()
// funcA, funcB, funcC 的先後順序不重要
// 直到這三個函式都回覆 resolve 或是「其中一個」 reject 才會繼續後續的行為

Promise.all([funcA(), funcB(), funcC()])
       .then(function(){ console.log('上菜'); });
```