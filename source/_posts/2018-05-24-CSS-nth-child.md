---
title: CSS-:nth-child選取器
date: 2018-05-24 09:20:34
categories:
- CSS
tags:
- CSS
- nth-child
- nth-of-type
- 選取器
---

{% cq %}

{% asset_img nth-child.png %}
CSS :nth-child(n) 筆記

{% endcq %}

<!-- more -->
***

## 前言

因為有時會搞錯 `:nth-child` 怎麼選取，又或者寫完後，怎麼想要的元素沒套用上CSS樣式。


## :nth-child(an+b) 口訣

有時我們選取元素時，會有一個固定的規則性，以下例為說明

<span id="inline-blue">公式</span> `:nth-child( 5n+2 )`

<span id="inline-blue">口訣</span> 要選 <font color="red">N個</font>裡面 的 <font color="red">第幾個</font>


所以若是 <font color="red">5n+2</font> 的話，就是 每<font color="red">5</font>個裡面 的 第<font color="red">2</font>個。

<a class="jsbin-embed" href="http://jsbin.com/xizozalino/2/embed?output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?4.1.4"></script>

> [JS Bin](http://jsbin.com/xizozalino/edit?html,css,output)

若是 <font color="red">6n+3</font> 的話，就是 每<font color="red">6</font>個裡面 的 第<font color="red">3</font>個。

<a class="jsbin-embed" href="http://jsbin.com/kapukikilo/1/embed?output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?4.1.4"></script>

> [JS Bin](http://jsbin.com/kapukikilo/1/edit?html,css,output)


## 注意的事項

使用:nth-child(n)時，必須為<font color="red">相同 且 連續</font>的子代物件

``` html
<h2>選取 div 的 B</h2>
<div class="boxs">
  <span>A</span>
  <div>B</div>
  <div>C</div>
  <div>D</div>
  <div>E</div>
</div>
```
假設我們想取得 `<div>B</div>` 的話，我們會怎麼寫呢？

<span id="inline-yellow">錯誤寫法</span>

因為我們用肉眼看的話， B 是在 .box裡面div排第一個，所以我們可能會這樣寫

``` css
.boxs div:nth-child(1){
  color: #fff;
  background: #25aaff;
}
```


<a class="jsbin-embed" href="http://jsbin.com/zekemopini/1/embed?css,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?4.1.4"></script>

>[JS Bin](http://jsbin.com/zekemopini/1/edit?html,css,output)

<font style="color:#f00;font-size:20px;">But</font>...若這樣寫的話，CSS會毫無作用，
以上述的程式碼解讀，nth-child(n) 我們要<font style="color:#f00;font-size:20px;">反著看</font>才行，
選取條件為

選取 .box 裡面第一個元素之後，再判斷是否為 div 標籤，若是的話，才會套用CSS樣式。

所以根據上面的說法，我們會

1. .box 裡面第一個元素 => 取到 `<span>A</span>`
2. 判斷是否為 div 標籤 => 判斷 `<span>A</span>` 是否為 div 標籤

經過上面判斷，不符合條件，所以就不會套用CSS樣式了。

<span id="inline-green">正確寫法</span>

``` css
/* 正確寫法 */

/*第一種寫法*/
.boxs div:nth-child(2){
  color: #fff;
  background: #25aaff;
}

/*第二種寫法*/
.boxs :nth-child(2){
  color: #fff;
  background: #25aaff;
}

/*第三種寫法*/
.boxs div:nth-of-type(1){
  color: #fff;
  background: #25aaff;
}
```

<a class="jsbin-embed" href="http://jsbin.com/vemawayele/1/embed?css,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?4.1.4"></script>

> [JS Bin](http://jsbin.com/vemawayele/1/edit?html,css,output)


<div class="note warning">依上述的案例，改為選取 <font color="red">class</font> 的話，也是一樣的道理，要符合<font color="red">相同 且 連續</font>的物件。</div>

***

## 參考文章

<div class="note info">[Amos 使用CSS3 :nth-child(n)](http://csscoke.com/2013/09/21/%E4%BD%BF%E7%94%A8css3-nth-childn-%E9%81%B8%E5%8F%96%E5%99%A8%E8%A9%B3%E8%A7%A3/)</div>