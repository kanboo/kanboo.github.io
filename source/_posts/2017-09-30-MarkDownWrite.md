---
title: MarkDown常用寫法(置頂)
date: 2017-09-30 00:03:49
categories:
- Hexo
tags:
- Hexo
- MarkDown
top: 900
---

{% cq %}
將常用的 MarkDown 紀錄在這，

<span id="inline-blue"> Ctrl + C </span> 、 <span id="inline-green"> Ctrl + V </span>

比較方便。
{% endcq %}

<!-- more -->

---

## 文字加顏色

<font color="blue">藍色字</font>

```
<font color="blue">藍色字</font>
```

<font color="red">紅色的字</font>

```
<font color="red">紅色的字</font>
```

<font style="color:#f90;font-size:20px;">20px 的字</font>

```
<font style="color:#f90;font-size:20px;">20px的字</font>
```

> [MARKDOWN/HTML 常用語法小結](https://xiwan.io/archive/markdown-html-common-syntax-summary.html)

## 文字增加背景色塊

<span id="inline-blue">站点配置文件</span>

<span id="inline-purple">主题配置文件</span>

<span id="inline-yellow">站点配置文件</span>

<span id="inline-green">主题配置文件</span>

```html
<span id="inline-blue">站点配置文件</span>
<span id="inline-purple">主题配置文件</span>
<span id="inline-yellow">站点配置文件</span>
<span id="inline-green">主题配置文件</span>
```

---

## 自定義樣式

<div class="note info">下列 自定義樣式，參考 [超深度优化](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#好玩的写作样式)
</div>

### 文本置中引用

{% cq %}

我不是一個偉大的程式設計師,我只是一個具有良好習慣的優秀程式設計師。

{% endcq %}

```
{% cq %}
我不是一個偉大的程式設計師,我只是一個具有良好習慣的優秀程式設計師。
{% endcq %}
```

### 数字塊

<span id="inline-toc">1.</span>左邊是效果。
<span id="inline-toc">2.</span>我是第二行。

``` html 客制 CSS 文件位置：~/blog/themes/next/source/css/\_custom/custom.styl
<span id="inline-toc">1.</span>
<span id="inline-toc">2.</span>

````
### label標籤

{% label default@default %}

``` dust
{% label default@default %}
````

{% label primary@primary %}

```dust
{% label primary@primary %}
```

{% label success@success %}

```dust
{% label success@success %}
```

{% label info@info %}

```dust
{% label info@info %}
```

{% label warning@warning %}

```dust
{% label warning@warning %}
```

{% label danger@danger %}

```dust
{% label danger@danger %}
```

### note 標籤

<div class="note default no-icon">default no-icon</div>

```html
<div class="note default no-icon">default no-icon</div>
```

<div class="note default">default</div>

```html
<div class="note default">default</div>
```

<div class="note primary">primary</div>

```html
<div class="note primary">primary</div>
```

<div class="note success">success</div>

```html
<div class="note success">success</div>
```

<div class="note info">info</div>

```html
<div class="note info">info</div>
```

<div class="note warning">warning</div>

```html
<div class="note warning">warning</div>
```

<div class="note danger">danger</div>

```html
<div class="note danger">danger</div>
```

### 引用

<blockquote class="question">内容</blockquote>

``` html 客制 CSS 文件位置：~/blog/themes/next/source/css/\_custom/custom.styl

<blockquote class="question">内容</blockquote>
```

---

## 程式碼

### 行內

`height: 50px;`

```md
`height: 50px;`
```

### 區段

````md 區段寫法
```[language] [title] [url] [link-text]
- [language] 是代碼語言的名稱，用來設置代碼塊顏色高亮，非必須；
- [title] 是頂部左邊的說明，非必須；
- [url] 是頂部右邊的超鏈接地址，非必須；
- [link text] 如它的字面意思，超鏈接的名稱，非必須。
```
````

上述 4 項應該是根據 {% label success@空格 %} 來分隔，而不是 `[]`，故請不要加 `[]`。
除非如果你想寫後面兩個，但不想寫前面兩個，那麼就必須加 `[]` 了，
要這樣寫： `[] [] [url] [link text]`。

MarkDown 寫法：

````[] CSS
``` CSS
.container {
    max-width: 960px;
    margin: 0 auto;
    /* 起手式 */
    margin-top: 10px;`
}
```
````

````[] js
``` js
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
````

<div class="note info">可支援的語法： [連結](https://almostover.ru/2016-07/hexo-highlight-code-styles/)</div>

---

## 插入圖片

---

### 內部圖片

{% asset_img logo.png logo %}

```md
{% asset_img logo.png logo %}
```

### 外部圖片

![Mario](https://goo.gl/2Dty9K)

```md
![Mario](https://goo.gl/2Dty9K)
```

## 文字超連結

---

More info: [Writing](https://hexo.io/docs/writing.html)

Google 連結：[Google](https://www.google.com.tw/)

```md
More info: [Writing](https://hexo.io/docs/writing.html)

Google 連結：[Google](https://www.google.com.tw/)
```

## 項目標籤

---

### 符號

- 序列 1
- 序列 2
- 序列 3

```md
- 序列 1
- 序列 2
- 序列 3
```

### Check

- [ ] 序列 1
- [ ] 序列 2
- [ ] 序列 3
- [ ] 序列 4

```md
- [ ] 序列 1
- [ ] 序列 2
- [ ] 序列 3
- [ ] 序列 4
```

### 數字

1.  序列 1
2.  序列 2
3.  序列 3
4.  序列 4

```md
1.  序列 1
2.  序列 2
3.  序列 3
4.  序列 4
```

## 重點標示

Strong emphasis, aka bold, with **asterisks** or **underscores**.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

```md
Strong emphasis, aka bold, with **asterisks** or **underscores**.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```
