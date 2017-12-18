---
title: HTML5 - 語義元素
date: 2017-12-18 11:40:28
categories: 
- HTML5
tags:
- HTML5
- 語義架構
---

{% cq %}

{% asset_img HTML5_01.jpg %}
重新認識HTML5的語義元素

{% endcq %}

<!-- more -->
***

## 常用元素

### ``<header>``


區塊標題，不要把它只用來取代 `<div class=”header”>`
它也可以是文章的標題，一頁可以有好多個 `<header>`，`<header>` 裡面至少要有一個 h1~h6。

### `<nav>`

導覽列。裡面裝的東西應該只有 **_主要_** 的navigation links，不要把各種link都丟到`<nav>`裡面。
舉例來說，footer裡面常常會有一排link，那個就不需要包進`<nav>`。

### `<main>`

一個頁面只有一個! 任何 global 都不能放在 main 裡面( e.g. `<header>` `<footer>` logo)

### `<section>`

通常用來把一些**_相關的元素組合在一起_**，一般來說，裡面都會包含heading。
如果這個區塊的內容可以分成幾個部分的話，那應該使用article。

``` html
<section>
  <h2>Section title</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec viverra nec nulla vitae mollis.</p>
</section>
```

### `<article>`

文章中可以脫離其他部分，獨立出來而又完整，甚至可以復用的一部分，通常有自己的標題，當article內嵌article時，裡外層的內容應該是相關的，比如一篇文章和它的留言，
而section雖然也具有獨立表達內容的能力，但是對外層有一定的相依性，例如這篇文章中的一個章節。

``` html
<article>
  <header>
    <h3>
      <a href="/my-blog-post">My blog post</a>
    </h3>
  </header>
  <section>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec viverra nec nulla vitae mollis.</p>
  </section>
  <footer>
    <small>
      Posted on <time datetime="2017-04-29T19:00">Apr 29</time> in <a href="/category/code">Code</a>
    </small>
  </footer>
</article>
```

### `<aside>`

通常用來包含一些和當前頁面內容有關的額外信息，比如廣告、個人資料信息或相關連結。

### `<footer>`

通常包含作者、版權信息或者相關鏈接等。

## 簡易分辦 `<section><article>`

`<article>`: 就算脫離了整體也是一個可以獨立存在、具有完整內容的區塊，例如這篇文章。
`<section>`: 具有獨立表達內容的能力，但是對外層有一定的相依性，例如這篇文章中的一個章節。

下面是簡單的問題，讓我們決定要用 `<section> / <article> / <div>`

- 具有完整內容的區塊，而且可以出現在你的**閱讀器**內嗎? 是的話就是 `<article>`
- 跟主要內容是有相關並且列入 outline 裡不會奇怪的? 是的話就是 `<section>`
- 跟內容無關或只用來 styling 的? 是的話就是 `<div>`


## 建議的架構範例

{% asset_img HTML5_02.jpg %}

{% asset_img HTML5_03.png %}

***

## 參考文章

<div class="note info">[结构性元素](http://techbrood.com/h5b2a?p=html-structure)
[HTML5 Semantic Elements](http://apolkingg8.logdown.com/posts/2014/02/04/note-of-semantic-html)
[HTML5 Semantics](http://www.hannahpun.me/performance-optimize2/)
[Semantic in HTML5](http://htmlreference.io/semantic/)
</div>