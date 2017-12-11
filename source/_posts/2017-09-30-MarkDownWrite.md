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

``` html
<font color="blue">藍色字</font>
<font color="red">紅色的字</font>
<font style="color:green;font-size:20px;">20px - 綠色的字</font>
 ```

[MARKDOWN/HTML常用語法小結](https://xiwan.io/archive/markdown-html-common-syntax-summary.html)

## 程式碼區段

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

