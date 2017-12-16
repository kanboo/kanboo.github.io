---
title: CSS-transform 2D
date: 2017-12-16 23:01:54
categories: 
- CSS
tags:
- CSS
- transform
- 2D
---


{% cq %}

利用 transform 將元素加入 <span id="inline-blue">位移、旋轉、縮放和傾斜</span> 的效果。

{% endcq %}

<!-- more -->

## 基本語法

transform 屬性可以設定 `一個` 或 `多個` 的參數，
若設定多個參數的話，中間的區隔使用 <span id="inline-blue">空白</span> 即可。

``` CSS
transform: 第一個參數 第二個參數 ...

transform: translate(100px) rotate(20deg);
```

常用的參數如下列

- 位移 - translate()、translateX()、translateY()
- 旋轉 - rotate()
- 縮放 - scale()、scaleX()、scaleY()
- 傾斜 - skew()、skewX()、skewY()

***

## 位移translate

語法

- 若設定二個，用 <span id="inline-blue">逗號</span> 區隔
 - translate(tx) → 若僅設定一個參數，代表只 {% label primary@位移X %} 
 - translate(tx, ty)

- 設定 X軸 位移
 - translate{% label danger@X %}()

- 設定 Y軸 位移
 - translate{% label danger@Y %}()


範例

位移 X軸 50px,位移 Y軸 50px

``` scss
.moved {
  transform: translate( 50px, 50px);
  // 上下二段語法，結果一樣
  transform: translateX( 50px) translateY( 50px);
}
```

<p data-height="189" data-theme-id="0" data-slug-hash="PEPREv" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transform-translate" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/PEPREv/">transform-translate</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## 旋轉rotate

語法

rotate( `旋轉角度` ) =>  rotate( 20{% label primary@deg %} )

旋轉30度角 = 30deg
旋轉90度角 = 90deg

範例

旋轉45度角

``` SCSS
.rotate {
  transform: rotate( 45deg);
}
```

<p data-height="181" data-theme-id="0" data-slug-hash="dJYmQQ" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transform-rotate" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/dJYmQQ/">transform-rotate</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## 縮放scale

語法

參數`預設`大小為 `1` ，若設定 `小於` 1 就是 `縮小`，`大於` 1 就是 `放大`。 

- 若設定二個，用 <span id="inline-blue">逗號</span> 區隔
 - scale(sx) → 若僅設定一個參數，代表{% label primary@同時 %}縮放 X 和 Y 
 - scale(sx, sy)

- 縮放 X軸
 - scale{% label danger@X %}()

- 縮放 Y軸
 - scale{% label danger@Y %}()

範例

縮小至0.7倍

``` SCSS
.scaled {
  transform: scale(0.7); 
  // 上下二段語法，結果一樣
  transform: scaleX(0.7) scaleY(0.7);
}
```

<p data-height="235" data-theme-id="0" data-slug-hash="QajrLG" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transform-scale" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/QajrLG/">transform-scale</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## 傾斜skew

語法

- 若設定二個，用 <span id="inline-blue">逗號</span> 區隔
 - skew(ax) → 若僅設定一個參數，代表只 {% label primary@傾斜X %} 
 - skew(ax, ay)

- 傾斜 X軸
 - skew{% label danger@X %}()

- 傾斜 Y軸
 - skew{% label danger@Y %}()

範例

skewed1 傾斜 X , skewed1 傾斜 X 和 Y

 ``` scss
 .skewed1 {
  transform: skew(10deg);
  // 等同 skewX(10deg)
  transform: skewX(10deg);
}

.skewed2 {
  transform: skew(10deg, 10deg);
  // 等同 skewX(10deg) skewY(10deg)
  transform: skewX(10deg) skewY(10deg);
}
```

 <p data-height="318" data-theme-id="0" data-slug-hash="zpvjqL" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transform-skew" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/zpvjqL/">transform-skew</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>