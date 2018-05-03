---
title: JS-事件效能處理
date: 2018-05-03 15:03:16
categories:
- JS
tags:
- JS
- requestAnimationFrame
- debounce
- throttle
- 事件效能
---

{% cq %}

{% asset_img throttle-vs-debounce.png %}

<font style="font-size:20px;">了解 debounce & throttle 來處理事件效能的問題。</font>

{% endcq %}

<!-- more -->
***

## 前言

在Youtube直播看到 **Alex大大** 線上分享JS的運用，這次分享處理事件效能的問題，
在此紀錄相關資訊，方便以後複習。

<div class="note info">Youtube：[Alex 宅幹嘛 - Javascript 30 #6 捲軸動畫與事件效能處理
](https://youtu.be/PnoZU60qvho)</div>

***
## 連續畫面的渲染 requestAnimationFrame

如果我們有利用 `setTimeout` 或 `setInterval` 要處理連續渲染畫面的需求的話，
建議可以改使用 `requestAnimationFrame` ，
避免有時會因為 `setTimeout` 或 `setInterval` 設定的時間不正確或太密集的話，
導致畫面LAG或不正常的情況發生。

<div class="note info">[requestAnimationFrame](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution?hl=zh-tw)</div>

***
## 事件效能問題

當JS遇到頻繁觸發事件(如：監聽 window的scroll、resize 事件、user瘋狂Click按鈕...等) 的情況，
有時可能會造成效能問題，而且<font color="red">常理來說</font>也不太可能會在幾毫秒間，就需要那麼頻繁觸發Event，
所以我們就可以利用 <font color="red">延遲執行</font> 這種方法，來減緩觸發Event的次數，
下列分別介紹 `debounce` 與 `throttle` 二種差異與用法。

***
## debounce

因為 `debounce` 有很多版本的寫法，所以附上我說明的版本。

``` js debounce
function debounce(func, wait = 20, immediate = true) {
  var timeout;
  return function () {
    var context = this, args = arguments;
    var later = function () {
      timeout = null;
      if (!immediate) func.apply(context, args);
    };
    var callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    if (callNow) func.apply(context, args);
  };
}
```

<span id="inline-green">情境描述：</span>

`debounce(function, wait, [immediate])`

- function = 給糖果
- wait： 需要冷靜的時間
- immediate： 先給或後給

假設我設定 **wait：10秒** 以及 **immediate：先給** 的情況下，

有一個小孩<font color="blue">第一次</font>跟媽媽吵著要糖吃(觸發事件，如Scroll、resize...)，
而debounce媽媽<font color="blue">先給</font>小孩糖果(完成任務)，
接著小孩立馬即時瞬間不間斷的，又要瘋狂吵鬧要吃糖(瘋狂觸發事件)，
媽媽說你要<font color="blue">有達到</font>冷靜10秒後，才會再給糖果，否則就不給糖。

這時有二種情況

1. 小孩依舊瘋狂吵要吃糖，沒有冷靜超過10秒以上，這時媽媽怎樣都不會給糖。
2. 小孩吵了一陣子，終於 <font color="red">有</font>冷靜10秒以上，這時媽媽才再給他一顆糖。

說了那麼多，總結來說就是

當觸發事件完成任務後，下次要再可以成功觸發完成任務的條件，
就是需<font color="red">停止觸發事件且超過wait設定的時間</font>，才可以再觸發完成一次任務。


<div class="note info">[Youtube-debounce解說](https://youtu.be/PnoZU60qvho?t=1h16m3s)
[Youtube-debounce隱性的問題](https://youtu.be/PnoZU60qvho?t=1h20m53s)</div>

***
## throttle

附上解說的版本，若 `limit = 0` 的話，就等於 debounce了。

``` js throttle
function throttle(fun, { wait = 33, limit = 0, immediate = false }) {
  let timer;
  let startTime = null;

  return function () {
    let context = this, args = arguments;
    let currentTime = new Date().getTime();

    if (startTime === null) {
      if (!immediate) startTime = currentTime
      else startTime = currentTime - limit
    }

    let waitFun = function () {
      fun.apply(context, args);
      startTime = null;
    }

    clearTimeout(timer);

    if (limit && currentTime - startTime >= limit) {
      fun.apply(context, args);
      startTime = currentTime;
    } else {
      timer = setTimeout(waitFun, wait);
    }

  }
}
```

<span id="inline-green">情境描述：</span>

一樣是「會吵的孩子有糖吃」的故事。

`throttle(fun, { wait = 33, limit = 0, immediate = false })`

- fun = 給糖果
- wait： 需要冷靜的時間
- limit： 超過最小間隔時間，就給糖
- immediate： 先給或後給

假設我設定 **wait：10秒** 、 **limit：5秒** 以及 **immediate：先給** 的情況下，

小孩第一次要糖吃時，throttle媽媽一樣先給小孩糖果，
但天下小孩一樣白目(有白色的眼球)，立馬又吵要糖吃，
小孩吵鬧依舊沒停止且超過10秒，不過throttle媽媽就比較心軟，
小孩雖然一直吵，但媽媽只要<font color="red">每超過5秒一次</font>就會心軟給小孩一個糖吃。

最後總結來說就是

當觸發事件完成任務後，有二種情況可以再觸發完成一次任務
1. 停止觸發事件 且 超過<font color="red">wait</font>設定的時間。
2. 連續觸發時，中間間隔時間有超過 <font color="red">limit</font>設定的時間。


<div class="note info">[Youtube-throttle解說](https://youtu.be/PnoZU60qvho?t=1h23m48s)
[Alex大-codepen範例](https://codepen.io/achen224/pen/pVRJLw)</div>

***
## 閉包（Closure）

上述 `debounce` 與 `throttle` 這二段程式碼中，都有使用到 <font color="red">閉包（Closure）</font>的用法了。

以 `debounce` 為例，他利用 `timeout` 這變數來紀錄且進行一些判斷，下列為他的生命週期
1. 判斷是否第一次執行，再看 `immediate` 變數，是設定 先執行or後執行。
2. 若是連續觸發時，就清除上一次的`setTimeout`。
3. 當連續觸發的最後一次時，就會執行 `later函式`，並清空`timeout`變數。

這樣下次連續觸發時，就會再回到 `1.`開始重新判斷。

``` js debounce解說
function debounce(func, wait = 20, immediate = true) {
  console.log('debounce~~~~in');
  // 紀錄 setTimeout 給的 timeoutID
  var timeout;

  return function () {
    console.log('function~~~~in');
    var context = this, args = arguments;

    var later = function () {
      // 清空 setTimeout 給的 timeoutID
      timeout = null;
      // 若 immediate 是設定「fals(後執行)」時，就會執行func
      if (!immediate) func.apply(context, args);
    };

    // 判斷是否 先執行 且 第一次
    var callNow = immediate && !timeout;

    // 當連續觸發時，用來清除上次的setTimeout
    clearTimeout(timeout);
    // 連續觸發的最後一次，會執行 later函式
    timeout = setTimeout(later, wait);

    // 若符合「先執行 且 第一次」，就執行func
    if (callNow) func.apply(context, args);
  };
}
```

<div class="note warning">補充：(提醒自己，不要腦補程式碼跑法)
1. 當宣告addEventListener時，會執一次`debounce函式`，並回傳新的函式，此時就完成<font color="red">閉包（Closure）</font>的寫法。
2. 之後每當觸發事件時，都是執行`新的函式`，就是上例的第 6 ~ 27 之間，而 `timeout` 就<font color="red">有點像</font>是新函式的全域變數，但不是真的在window的全域變數。([jsbin-捲動畫面並開console看](http://jsbin.com/dodijijuye/edit?js,output))
3. JS是由上往下逐行執行，記得看到<font color="red">最後一行</font>才算結束。
4. `later函式`一開始只是宣告，沒執行，最後要等`setTimeout`才會執行</div>
