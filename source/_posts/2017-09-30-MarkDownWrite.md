---
title: MarkDown常用寫法
date: 2017-09-30 00:03:49
categories: 
- Hexo
tags:
- Hexo
- MarkDown
---


## 文字加顏色

<font color="blue">藍色字</font>
<font color="red">紅色的字</font>
<font style="color:green;font-size:20px;">20px - 綠色的字</font>

``` html html
<font color="blue">藍色字</font>
<font color="red">紅色的字</font>
<font style="color:green;font-size:20px;">20px - 綠色的字</font>

 ```

>[MARKDOWN/HTML常用語法小結](https://xiwan.io/archive/markdown-html-common-syntax-summary.html)


## 文字增加背景色塊

<span id="inline-blue"> 站点配置文件 </span>

<span id="inline-purple"> 主题配置文件 </span>

<span id="inline-yellow"> 站点配置文件 </span>

<span id="inline-green"> 主题配置文件 </span>

``` html
<span id="inline-blue"> 站点配置文件 </span>
<span id="inline-purple"> 主题配置文件 </span>
<span id="inline-yellow"> 站点配置文件 </span>
<span id="inline-green"> 主题配置文件 </span>
```

***
## 自定義樣式

<div class="note info">下列 自定義樣式，參考 [超深度优化](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#好玩的写作样式)
</div>

### 自定義樣式 数字塊

<span id="inline-toc">1.</span>左邊是效果。
<span id="inline-toc">2.</span>我是第二行。

``` html 客制CSS文件位置：~/blog/themes/next/source/css/_custom/custom.styl
<span id="inline-toc">1.</span>
<span id="inline-toc">2.</span>
```

### 主题自带样式 label標籤

{% label default@default %}

``` dust
{% label default@default %}
```

{% label primary@primary %}

``` dust
{% label primary@primary %}
```

{% label success@success %}

``` dust 
{% label success@success %}
```

{% label info@info %}

``` dust 
{% label info@info %}
```

{% label warning@warning %}

``` dust 
{% label warning@warning %}
```

{% label danger@danger %}

``` dust 
{% label danger@danger %}
```

### 主题自带样式 note 標籤

<div class="note default no-icon"><p>default no-icon</p></div>

``` html
<div class="note default no-icon"><p>default no-icon</p></div>
```

<div class="note default"><p>default</p></div>

``` html
<div class="note default"><p>default</p></div>
```

<div class="note primary"><p>primary</p></div>

``` html
<div class="note primary"><p>primary</p></div>
```

<div class="note success"><p>success</p></div>

``` html
<div class="note success"><p>success</p></div>
```

<div class="note info"><p>info</p></div>

``` html
<div class="note info"><p>info</p></div>
```

<div class="note warning"><p>warning</p></div>

``` html
<div class="note warning"><p>warning</p></div>
```

<div class="note danger"><p>danger</p></div>

``` html
<div class="note danger"><p>danger</p></div>
```

### 自定義樣式 引用

<blockquote class="question">内容</blockquote>

``` html 客制CSS文件位置：~/blog/themes/next/source/css/_custom/custom.styl
<blockquote class="question">内容</blockquote>
```

## 程式碼區段

``` html [language] [title] [url] [link-text]
- [language] 是代碼語言的名稱，用來設置代碼塊顏色高亮，非必須；
- [title] 是頂部左邊的說明，非必須；
- [url] 是頂部右邊的超鏈接地址，非必須；
- [link text] 如它的字面意思，超鏈接的名稱，非必須。
```

### inline code
`height: 50px;`

### code block

#### CSS型態
``` CSS
.container {
    max-width: 960px;
    margin: 0 auto;
    /* 起手式 */
    margin-top: 10px;`
}
```

<!-- more -->

#### javascript型態
``` javascript
.container {
    function checkList(e) {
        var num = e.target.dataset.num;
        // console.log(e.target.nodeName);
        if (e.target.nodeName !== 'LI') {
            return
        };
        country.splice(num, 1);
        updateList();
    }
}
```

## 插入圖片
---

### 內部圖片
{% asset_img logo.png logo %}

### 外部圖片
![Mario](https://goo.gl/2Dty9K)


## 文字超連結
---

More info: [Writing](https://hexo.io/docs/writing.html)

Google連結：[Google](https://www.google.com.tw/)




## 項目標籤
---

### 符號
* 序列1
* 序列2
* 序列3
* 序列4

### Check
- [ ] 序列1
- [ ] 序列2
- [ ] 序列3
- [ ] 序列4

### 數字
1. 序列1
2. 序列2
3. 序列3
4. 序列4


## 重點標示
---

Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

---

