---
title: JS30-02-JS-and-CSS-Clock
date: 2018-02-14 23:48:20
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img JS_CSS_Clock.png %}

<font style="font-size:20px;">利用 JS 與 CSS 搭配作出時鐘效果</font>

{% endcq %}

<!-- more -->
***

## 目標

- 顯示目前時間
- 利用 JS 與 CSS 搭配作出時鐘效果

## 實踐步驟

1. 先調整時鐘的 CSS樣式

    修改前：
    {% asset_img JS_CSS_Clock_01.png %}

    新增CSS修改語法：
    ``` css
    transform: rotate(90deg); /* 將 時、分、秒針 從 45分 旋轉至 12點 方向 */
    transform-origin: 100% 50%; /* 將旋轉的原點移位 */
    transition: all 0.05s cubic-bezier(0, 2.95, 1, 1); /* 讓秒針有跳動的效果 */
    border-radius: 50%; /* 原先是 長方形 ，修改成 圓角 ，比較像 時針 */
    ```

    修改後：

    {% asset_img JS_CSS_Clock_02.png %}

2. 取得目前時間，並每秒更新一次
    - 利用 `setInterval(setDate, 1000)` 每秒更新
    - 使用 `new Date()` 取得目前 時、分、秒

3. 一個圓共 `360deg`，分別依照 時、分、秒 計算出各自的角度
    - 秒針：(360 / 60) * seconds + 90
    - 分針：(360 / 60) * seconds + 90
    - 時針：(360 / 12) * seconds + 90
    - 補充說明：最後有加 90 是因原先都指向45分的位置，為了指向12點方向，所以有先用CSS旋轉90度 `rotate(90deg)`。

4. 最後用 JS 透過 `element.style` 修改 時、分、秒 的角度
    - `element.style.tranform = "roate( 角度 )"`

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/02%20-%20JS%20and%20CSS%20Clock/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/02%20-%20JS%20and%20CSS%20Clock/index.html)

***
## JS學習紀錄

### setInterval()

如果需要一直持續循環作動，使用 `setInterval`，例如網頁上每一秒鐘就更新一次時間。

語法：`setInterval(callback, time)`

``` js
//每秒刷新時間
setInterval(setDate,1000);
```

***
### Date()

取得時間的函數

``` js
const nowDate = new Date();

nowDate.getSeconds(); //取得當前秒
nowDate.getMinutes(); //取得當前分鐘
nowDate.getHours();   //取得當前小時
```

***
### element.style

一開始想說要改 CSS 屬性，就很直覺的使用 `第一種` 方法，學習過程中，得知 `第二種` 方法也行。

``` js 修改CSS：第一種
secondHand.setAttribute("style", `transform: rotate(${secondDegrees}deg)`)
```

``` js 修改CSS：第二種
secondHand.style.transform = `rotate(${secondDegrees}deg)`
```

***
## 延伸：transform:rotate的彈跳問題

當秒針旋轉一圈之後，要開始旋轉第二圈時，角度的計算變化會是 444°→90°→96°，重點就在 444° 變成 90° 時，本來rotate 都是順時針旋轉，但 444°→90° 那一瞬間會是 逆時針 轉回去，而造成秒針跳動有點怪異，所以解決方式如下

在角度計算 444°→90° 重新循環時，將 `transition` 屬性關掉，由於距離短，時間短，將逆時針迴旋的過程瞬間完成。

``` js 修正秒針跳動的效果
const nowDate = new Date();

//秒針
const seconds = nowDate.getSeconds();
const secondDegrees = (360 / 60) * seconds + 90;

// 修正秒針跳動的效果
if(secondDegrees === 90){
    secondHand.style.transition = 'all 0s';
}else{
    secondHand.style.transition = 'all 0.05s cubic-bezier(0, 2.95, 1, 1)';
}

secondHand.style.transform = `rotate(${secondDegrees}deg)`
```

<div class="note info">延伸問題參考：[soyaine](https://github.com/soyaine/JavaScript30/tree/master/02%20-%20JS%20%2B%20CSS%20Clock)、[GuaHsu](https://guahsu.io/2017/05/JavaScript30-02-JS-and-CSS-Clock/)</div>