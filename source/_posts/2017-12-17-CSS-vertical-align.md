---
title: CSS-垂直置中的方法
date: 2017-12-17 14:37:42
categories: 
- CSS
tags:
- CSS
- vertical-align
- 垂直置中
---

{% cq %}

整理常用CSS常用的 <span id="inline-blue">垂直置中</span> 的方法。

{% endcq %}

<!-- more -->
***

## 文字的垂直置中方法

此方法適用 <font style="color:#f90;font-size:20px;">單行</font>，因為是行高，所以會在行內元素的上下都加上行高的 1/2 ，
如果<font style="color:#f90;font-size:20px;">多行</font>，第二行與第一行的間距會變超大，就會導致沒有垂直置中的效果。

``` diff
// height 和 line-height 要一樣高

.div0,.div1 {
  width: 300px;
+ height: 250px;
+ line-height: 250px;
  text-align: center;
  border: 1px solid #000;
}
```

<p data-height="326" data-theme-id="0" data-slug-hash="BJoMKJ" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="垂直置中 - line-height" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/BJoMKJ/">垂直置中 - line-height</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***

## calc & transform

使用 <font style="color:red;font-size:20px;">calc</font> 動態計算的能力，讓要置中的 div 的 top 屬性，
與上方的距離是<font style="color:#f90;font-size:20px;">「50% 的外框高度 + 50% 的 div 高度」</font>，就可以做到垂直置中。

範例1：設定top：50%，再扣掉 div 的 高度/2
- 方法1：top: calc( 50% - (高度/2) )
- 方法2：top: 50%; margin-top: -(高度/2);
- 方法3：top: 50%; transform: translateY(-50%);  <font style="color:red;font-size:16px;"><i class="fa fa-thumbs-o-up" aria-hidden="true"></i>自行計算50%的div高度</font>

``` diff
.redbox {
    background: #c00;
    float: left;
+   position: relative; //要宣告position，才能使用 top、left
    width: 30px;
-   //計算方法1
+   height: 30px;
+   top: calc(50% - 15px); //高：30/2=15
  }
  .greenbox {
    background: #0c0;
    float: left;
    position: relative;
    width: 30px;
-   //計算方法2
+   height: 80px;
+   top: 50%;
+   margin-top: -40px;  //高：80/2=40
  }
  .bluebox {
    background: #00f;
    float: left;
    position: relative;
    width: 30px;
    height: 50px;
-   //計算方法3
+   top:50%;
+   transform: translateY(-50%);
  }
```

<p data-height="280" data-theme-id="0" data-slug-hash="ppjYBR" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="垂直置中 - calc 動態計算1" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/ppjYBR/">垂直置中 - calc 動態計算1</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

範例2：將三個div設定寬高各30px，將滑鼠移到黑框內，可觀看效果。

<p data-height="277" data-theme-id="0" data-slug-hash="xpwozp" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="垂直置中 - calc 動態計算2" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/xpwozp/">垂直置中 - calc 動態計算2</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***

## 絕對定位

利用絕對位置來指定，要將 <font style="color:red;font-size:20px;">上下</font> 的數值都設為 <font style="color:red;font-size:20px;">0</font>，再搭配一個 <font style="color:red;font-size:20px;">margin:auto</font>，就可以辦到垂直置中。

<div class="note info">邏輯：
這個方法同時設定top和bottom為0，使得這個div完全不可能符合，最後再透過margin這個指令，讓它達到垂直置中的效果
</div>

``` diff
  .redbox {
-   // 垂直置中
    background: #c00;
    position: absolute; //要宣告，才能使用 top、left
+   top: 0;
+   bottom: 0;
+   margin: auto;
  }
  .bluebox {
-   // 垂直置中 + 水平置中
    background: blue;
    position: absolute; //要宣告，才能使用 top、left
    top: 0;
    bottom: 0;
+   left: 0;
+   right: 0;
    margin: auto;
  }

```
<p data-height="230" data-theme-id="0" data-slug-hash="baVzBM" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="垂直置中 - 絕對定位" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/baVzBM/">垂直置中 - 絕對定位</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***

## Flexbox

CSS3 最威的盒子模型：Flexbox，使用 <font style="color:#f90;font-size:20px;">align-items</font> 或 <font style="color:#f90;font-size:20px;">align-content</font> 的屬性，

輕輕鬆鬆就可以做到垂直置中的效果喔，Bootstrap 4 也有用喔。

``` diff
.div0 {
+ display: flex;
+ align-items: center;
+ justify-content: center;
  width: 150px;
  height: 150px;
  border: 1px solid #000;

  .redbox {
    width: 30px;
    height: 30px;
    background: #c00;
  }
}
```

<p data-height="225" data-theme-id="0" data-slug-hash="Zvbger" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="垂直置中 - Flexbox" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/Zvbger/">垂直置中 - Flexbox</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***
## 參考文章

<div class="note info">[CSS 垂直置中的七個方法](http://www.oxxostudio.tw/articles/201502/css-vertical-align-7methods.html)
[CSS垂直置中的方法](https://pjchender.blogspot.tw/2015/04/css_15.html)
</div>