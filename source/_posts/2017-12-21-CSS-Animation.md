---
title: CSS-Animation 動畫
date: 2017-12-21 20:52:13
categories: 
- CSS
tags:
- CSS
- animattion
- 動畫
---


{% cq %}

CSS的Animation分為兩個部分，
一個是決定動畫該如何跑的 <font style="color:blue;font-size:20px;">Keyframe</font>，
另一個是將動畫載入元素的 <font style="color:red;font-size:20px;">Animation</font>。

{% endcq %}

<!-- more -->

## Keyframes 語法

``` scss
//第一種寫法
@keyframes 自訂的name {
  from { ... }
  to { ... }
}

//第二種寫法
@keyframes 自訂的name {
  0% { ... }
  50% { ... }
  100% { ... }
}
```


## Animation 屬性

``` scss
//完整語法
animation: 自訂的name duration timing-function delay iteration-count direction fill-mode play-state;


//常用寫法
animation: 自訂的name duration timing-function iteration-count;
animation: time 3s linear infinite;
```

- Name：<span id="inline-purple">@keyframes</span> 動畫名稱。
- Duration：動畫時間。
- Timing Function：動畫進行的速度曲線。
 - ease：是<font color="red">預設</font>的。慢進 → 加速 → 減速到結束。
 - ease-in：慢進 → 加速到結束。
 - ease-out：快進 → 減速到結束。
 - ease-in-out：開始跟結束都是慢的狀態。
 - linear：以相同速度前進。
 - steps()：無連續的動作，直接跳至各步 ( step ) 的指定 css。
 - cubic-bezier()：指定動畫滑動的曲線。
- Delay：延遲動畫開始的時間。
- Iteration Count：動畫重複次數(<font color="red">預設一次</font>)。 
 - infinite(無限) | 次數
- Direction：動畫播放方向。
 - normal ：每次播放都是從 0% 至 100%
 - reverse ：每次播放都是從 100% 至 0%
 - alternate ：播放兩次以上的話，會從 0% 至 100% ，再從 100% 回到 0% ，以此類推
 - alternate-reverse ：跟 alternate 相反，會先從 100% 開始播放
- Fill Mode：控制動畫播放完後的最終狀態。 
 - none ：回到未播放動畫效果前的狀態
 - forwards ：停在動畫的最後一個狀態上
 - backwards ：停在動畫的第一個狀態上 (實測不出來)
 - both ：視 animation-direction 來決定停在哪一個狀態上。
- Play State：指定動畫播放或暫停。
 - 可以的選項有 running|pause，與影片的播放、暫停是同樣的意思；這在與 JS 搭配時，可妥善控制動畫。

***

## 範例

翻轉效果

<p data-height="149" data-theme-id="0" data-slug-hash="opzgRa" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="CSS Animation-範例1" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/opzgRa/">CSS Animation-範例1</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

滾動效果

<p data-height="202" data-theme-id="0" data-slug-hash="vpXOOw" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="CSS Animation-範例2" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/vpXOOw/">CSS Animation-範例2</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***

## 參考文章

<div class="note info">[CSS3 動畫基礎](https://www.openfoundry.org/tw/tech-column/9233-css3-animation)
[CSS3 Animation](https://cythilya.github.io/2017/08/27/css-animation/)
[Animation動畫效果](http://carlos-studio.com/2017/03/13/css-animation-%E5%8B%95%E7%95%AB%E6%95%88%E6%9E%9C/)
</div>