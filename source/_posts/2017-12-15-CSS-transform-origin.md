---
title: CSS-transform-origin 軸心
date: 2017-12-15 16:19:18
categories: 
- CSS
tags:
- CSS
- transform
- transform-origin
---

{% cq %}

{% asset_img transform-origin.png %}

<font style="font-size:18px;">
在學習 transform 之前，先了解 transform-origin 軸心 怎麼變化？
</font>

{% endcq %}
<!-- more -->
***

## 說明

在撰寫CSS transform 時，預設的 **軸心** 是在Box的正中央位置(如下圖)，

所以要製作一些比較特別的效果的話，如：[時間](https://codepen.io/Kanboo/pen/VyLggQ)、[開門](https://codepen.io/Kanboo/pen/ypNZrY) …等

就利用變更軸心的位置，再配合 transform 來達成。

``` scss
transform-origin: x y;
```

{% asset_img transform-origin_1.png %}

## 範例：

利用 transform: rotate(90deg) 旋轉 90度，呈現因 <span id="inline-yellow"> 軸心位置不同 </span> ，而旋轉的結果有何不一樣。

<font style="color:red;font-size:20px;"> C </font> 軸心： 正中間(預設)
<font style="color:red;font-size:20px;"> F </font> 軸心： 右下角
<font style="color:red;font-size:20px;"> E </font> 軸心： 左邊中間

<p data-height="440" data-theme-id="0" data-slug-hash="YYzyrX" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="transform-origin 更改軸心" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/YYzyrX/">transform-origin 更改軸心</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


<div class="note primary">其他範例
[teaching transform-origin](https://codepen.io/Kanboo/pen/vpObQj)
[Transform Origin Examples](https://codepen.io/Kanboo/pen/QaboLW)
</div>


***

## 參考文章

<div class="note info">[CSS沒有極限 - CSS transform-origin](https://wcc723.github.io/css/2013/10/10/css-transform-origin/)
</div>