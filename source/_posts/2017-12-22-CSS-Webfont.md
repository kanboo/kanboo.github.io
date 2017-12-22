---
title: CSS-webfont 字體
date: 2017-12-22 15:08:18
categories: 
- CSS
tags:
- CSS
- webfont
- 字體
---

{% cq %}

{% asset_img webfont_01.png %}

{% endcq %}

<!-- more -->
***

## 使用方法

``` scss
//引入字體
@import url(//fonts.googleapis.com/earlyaccess/notosansscsliced.css);

//設定字體
font-family: 'Noto Sans SC Sliced', sans-serif;
```

## 範例

<p data-height="286" data-theme-id="0" data-slug-hash="dJpzZm" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="Webfont - 思源體" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/dJpzZm/">Webfont - 思源體</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

***

## 問題紀錄

雖然已有在css設定 <font color="blue">font-family: 'Noto Sans SC Sliced', sans-serif;</font> ，
不過還是沒套用到新的 **font-family** ，因為<font color="red">CSS權重</font>的問題，還是使用預設的 <font color="blue">微體正黑體</font>，


{% asset_img webfont_02.png %}

可能是因為 codepen 有額外加 font-family 設定，導致 `<style>`的CSS權限 大於在 CSS檔裡面的屬性設定

{% asset_img webfont_03.png %}

所以若要強制轉換的話，就需要加 <font color="red">!important</font> 

``` scss
font-family: 'Noto Sans SC Sliced', sans-serif !important;
```

***

## 參考文章

<div class="note info">[谷歌提出字体切片方案](https://www.landiannews.com/archives/38885.html)
[Noto Sans SC Sliced](https://fonts.google.com/earlyaccess#Noto+Sans+SC+Sliced)
</div>