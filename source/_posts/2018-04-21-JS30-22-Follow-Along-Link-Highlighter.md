---
title: JS30-22-Follow-Along-Link-Highlighter
date: 2018-04-21 10:02:05
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js22.jpg %}

<font style="font-size:20px;">滑鼠移動到特定位置產生highlight效果。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 滑鼠移動到特定位置產生highlight效果。


## 實踐步驟

1. 取得所有 a元素，並監聽 mouseenter 事件
2. 建立一個 span 來產生 <font color="red">highlight</font> 效果
3. 更新span的<font color="red">寬高及定位</font>

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/22%20-%20Follow%20Along%20Link%20Highlighter/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/22%20-%20Follow%20Along%20Link%20Highlighter/index.html)

***
## JS學習紀錄

此次的練習主要在於

1. 如何正確計算元素定位座標(考量window.scroll因素)

### 一、取得所有 a元素，並監聽 mouseenter 事件

``` js
/* step 1. 取得所有 a元素，並監聽 mouseenter 事件 */
// 取得HTML中所有的a元素
const triggers = document.querySelectorAll('a');

// 監聽所有 a元素 的 滑鼠移入 事件
triggers.forEach(a => a.addEventListener('mouseenter', highlightLink));
```

### 二、建立一個 span 來產生 highlight 效果

``` js
/* step 2. 建立一個 span 來產生 highlight 效果 */
const highlight = document.createElement('span');
highlight.classList.add('highlight');
// 將建立的 span 加到頁面中
document.body.append(highlight);
```

### 三、更新 span 的 寬高及定位

<span id="inline-blue">初版</span>

一開始雖然有達到 highlight效果，不過因為 <font color="red">window捲軸滾動</font> 的關係，
所以當捲軸有<font color="red">往下滑</font>時，此時再更新 <font color="red">span的定位</font> 後，
highlight效果就會有位置偏差的問題，沒顯示在準確的位置。

``` js
/* step 3. 更新span的「寬高、定位」，讓 highlight效果 在準確的位置 */
function highlightLink() {
  // 目標元素的 大小 與 相對於瀏覽器視窗的位置資訊
  const linkCoords = this.getBoundingClientRect();

  // 設定 highlight效果 的 寬高及定位
  highlight.style.width = `${linkCoords.width}px`;
  highlight.style.height = `${linkCoords.height}px`;
  highlight.style.transform = `translate(${linkCoords.left}px, ${linkCoords.top}px)`;
}
```

<span id="inline-yellow">修正版</span>

因為 window捲軸滾動 的因素，必須加上 <font color="red">scroll移動值</font>，來修正偏差的位置。

``` diff
/* step 3. 更新span的「寬高、定位」，讓 highlight效果 在準確的位置 */
function highlightLink() {
  // 目標元素的 大小 與 相對於瀏覽器視窗的位置資訊
  const linkCoords = this.getBoundingClientRect();
  console.log(linkCoords);

+ // 儲存span的「寬高、定位」 資訊
+ const coords = {
+   width: linkCoords.width,
+   height: linkCoords.height,
+   top: linkCoords.top + window.scrollY, // 定位新增 window.scroll 的影響因素
+   left: linkCoords.left + window.scrollX // 定位新增 window.scroll 的影響因素
+ };

  // 設定 highlight效果 的「寬高、定位」
+ highlight.style.width = `${coords.width}px`;
+ highlight.style.height = `${coords.height}px`;
+ highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
}
```

<div class="note info">[Element.getBoundingClientRect()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)</div>