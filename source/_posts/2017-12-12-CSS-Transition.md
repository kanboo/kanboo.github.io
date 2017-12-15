---
title: CSS-transition 轉場效果
date: 2017-12-12 15:37:29
categories: 
- CSS
tags:
- CSS
- transition
- 動畫
- 轉場效果
---

{% asset_img transition_01.png %}

<font style="font-size:18px;">
轉場是從 A 狀態，轉變成 B 狀態，中間的過程，就叫轉場，在 CSS 中叫做 transition。
</font>

<!-- more -->

## 語法

``` scss

transition: property  duration  timing-function  delay;
transition: 套用的屬性 花費時間   時間速率         等待時間;

transition: all 2s ease-out 1s;

// 等同於：
transition-property: all; // default: all
transition-duration: 2s;  // default: 0；2s 表示 2 秒；2ms 表示 2 毫秒。
transition-timing-function: ease-out;
transition-delay: 1s; // 開始進行轉場效果之前，所要等待的時間。

```

## transition-timing-function 時間速率

{% asset_img transition-timing-function.png %}

<br>

<p data-height="265" data-theme-id="dark" data-slug-hash="dJyMQw" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transition 各種速率" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/dJyMQw/">transition 各種速率</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## 範例

### 範例 1：hover

<p data-height="265" data-theme-id="0" data-slug-hash="NXWrBw" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="Transition - hover" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/NXWrBw/">Transition - hover</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### 範例 2：設定Two CSS properties 

<p data-height="265" data-theme-id="0" data-slug-hash="QaWEJm" data-default-tab="css,result" data-user="Kanboo" data-embed-version="2" data-pen-title="Transition - 設定Two CSS properties" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/QaWEJm/">Transition - 設定Two CSS properties</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

特別的點：
``` scss
//設定二個 CSS屬性，設定不同 時間、速率..等
transition: background .2s linear, border-radius 1s ease-in 1s;
```

上面案例，特別於在 **:hover** 時，有改變 **背景色** 和 **圓角** 效果，
不過在 transition 的設定，分別針對 二個屬性設定不同的時間、速率…等

***

## 參考資料

[[CSS][Transition] 轉場效果](http://carlos-studio.com/2017/02/23/css-transition-%E8%BD%89%E5%A0%B4%E6%95%88%E6%9E%9C/)

[CSS3轉場效果(transitions)](https://eyesofkids.gitbooks.io/css3/contents/transitions.html#css3轉場效果transitions)

[CSS transition 各種速率](https://wcc723.github.io/css/2013/08/24/css-transtion-speed/)