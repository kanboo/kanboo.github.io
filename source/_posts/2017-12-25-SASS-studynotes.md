---
title: SASS-基礎用法
date: 2017-12-25 23:47:46
categories: 
- SASS
tags:
- SASS
---

{% cq %}

{% asset_img sass_01.png %}

{% endcq %}

<!-- more -->
***

## 變數命名(前面加個 $ 字號)

``` scss
//變數
$font-color: bule;
$font-m: 16px;
$font-l: $font-m * 1.2;
$font-s: $font-m * 0.8;

.box01 {
    color: $font-color;
    font-size: $font-l;
}

.box02 {
    color: $font-color;
    font-size: $font-s;
}
```

***
## @import

``` scss 常見的分類
@import mixin   //放置所有Sass全域變數與Mixin
@import reset   //reset.css
@import layout  //共同框架(如：表頭、表尾)
```

{% asset_img sass_02.png %}

***
## @mixin + @include

建立：
@mixin + 名稱 { 語法內容} ;

插入：
@include + 名稱 ;


``` scss 建立
//圓角效果
@mixin circle($size,$bgcolor){
    border-radius: 50%;
    width: $size;
    height: $size;
    background-color: $bgcolor;
}// @mixin可搭配變數應用

```

``` scss 插入
.circlebox{
    @include circle(100px,red)
}
```
***
## @mixin + @content

``` scss 各種載具斷點
//iPad - 768px
@mixin pad {
    @media(max-width: 768px){
        @content;
    }
}
//iPad以下 - 767px
@mixin mobile {
    @media(max-width: 767px){
        @content;
    }
}
//iPhone 6 Plus - 414px (視專案族群)
@mixin i6plue {
    @media(max-width: 414px){
        @content;
    }
}
//iPhone 6 - 375px (視專案族群)
@mixin i6 {
    @media(max-width: 768px){
        @content;
    }
}
//iPhone 5、SE - 320px
@mixin i5 {
    @media(max-width: 320px){
        @content;
    }
}
```

``` scss 使用方式
.header{
    width: 100px;
    height: 100px;

    //iPad - 768px
    @include pad(){
        height: auto;
    }
}
```

***
## Sass顏色函數

``` scss 常用函數
$bg-color: #ff0000;

.darken:hover {
  background: darken($bg-color, 20%); //將紅色調暗10%
}

.lighten:hover {
  background: lighten($bg-color, 20%); //將紅色調亮10%
}

.invert:hover {
  background: invert($bg-color); //返回一個反相色
}
```

範例
<p data-height="199" data-theme-id="0" data-slug-hash="XVprKR" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="Sass-顏色函數" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/XVprKR/">Sass-顏色函數</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***
## 參考文章

<div class="note info">[30天掌握Sass語法](https://ithelp.ithome.com.tw/users/20040221/ironman/562)
[SassMe顏色函數](http://jim-nielsen.com/sassme/)
</div>