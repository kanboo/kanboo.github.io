---
title: JS30-03-CSS-Variables
date: 2018-02-15 16:39:51
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img Scoped_CSS_Variables_and_JS.png %}

<font style="font-size:20px;">利用 <font color="red">CSS變數</font> 與 <font color="red">JS</font> 即時更新圖片的 內距、邊框色、模糊 效果</font>

{% endcq %}

<!-- more -->
***

## 目標

- 使用 CSS變數 功能
- 透過 JS 更改 CSS變數值，達到即時更新 內距、邊框色、模糊 的效果

## 實踐步驟

1. 在 CSS 的 :root 宣告 CSS變數
    - 宣告方法：使用兩個`-`符號，代表「變數」，如：\--spacing
    - 使用方法：使用var()代表「使用變數」，如：var(\--spacing)

2. 分別監聽(change、mousemove)三個 input 的值
    - 利用 `dataset` 取得自定義的資料，如：this.dataset.sizing
    - 透過 `document.documentElement.style.setProperty('--base', '#fff');` 更改CSS的變數

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/03%20-%20CSS%20Variables/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/03%20-%20CSS%20Variables/index.html)

***

## CSS學習紀錄

使用 CSS的變數 功能，不過在 [IE](https://caniuse.com/#feat=css-variables) 上，好像還不支援。

``` css CSS變數說明
/* 在CSS 的 :root(全局)設定 變數 */
:root {
    --base: #ffc600;
    --spacing: 50px;
    --blur: 10px;
}

/* 使用CSS變數方法：var(變數名稱) */
img {
    padding: var(--spacing);
    background: var(--base);
    filter: blur(var(--blur));  /* CSS濾鏡效果：模糊 */
}

.hl {
    color: var(--base);
}
```

### :root 偽元素(全局)

`:root` 這個偽元素是文檔的根元素，等同於 `<html>` 標籤，所以常用於聲明<font color="red">全局</font>的CSS變量：

``` css 設定變數(全局)
：root {
   --color：#fff ;
}
```

在CSS style要使用時，用 `var(變數名稱)`，如下：

``` css 在CSS使用變數
img {
   background： var(--color);
}
```

若是要在 JS 使用的話，語法如下：

``` js 用JS更改CSS變數
// ：root = 文檔的根元素 = <html> = document.documentElement
document.documentElement.style.setProperty('--color', '#000');
```
<div class="note info">[CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)</div>

***
### filter 濾鏡

CSS3的濾鏡功能，其中 `blur` 是高斯模糊，參數越高越模糊

<div class="note info">[MDN-filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)</div>

***
## JS學習紀錄

### dataset

利用 `dataset` 可取得自定義的資料，另外也可使用 `getAttribute` 取得資料。

``` html HTML
<input id="blur" type="range" name="blur" value="10" data-sizing="px">
```

``` js JS取得dataset
//方法1
document.querySelector('#blur').dataset.sizing  // 輸出：px
document.querySelector('#blur').dataset[sizing] // 輸出：px

//方法2
document.querySelector('#blur').getAttribute('data-sizing'); // 輸出：px
```

<div class="note info">[MDN-dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)</div>

***
### style.setProperty

下列三種方法，皆可達到修改CSS的效果，不過實務上用`方法2`，會比較容易帶 `參數` 及 `可讀性` 較佳。

``` js 修改CSS
const DOM = document.querySelector('img');

//方法1
DOM.setAttribute("style", `padding: 10px`)

//方法2
DOM.style.setProperty('padding', '10px');

//方法3
DOM.style.padding = '10px';
```

<div class="note info">[MDN-setProperty](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration/setProperty)</div>