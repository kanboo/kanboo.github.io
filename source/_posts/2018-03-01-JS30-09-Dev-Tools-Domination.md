---
title: JS30-09-Dev-Tools-Domination
date: 2018-03-01 16:55:55
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img group.png %}

<font style="font-size:20px;">學習Chrome <font color="red">Debug</font> 的工具</font>

{% endcq %}

<!-- more -->
***

## 目標

- 如何觀察 DOM 的變化與查看狀態
- console 的各種用法

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/09%20-%20Dev%20Tools%20Domination/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/09%20-%20Dev%20Tools%20Domination/index.html)

<div class="note info">[六角學院-Chrome 網頁除錯功能大解密](https://www.udemy.com/chrome-devtools)
[官方-Console API 參考](https://developers.google.com/web/tools/chrome-devtools/console/console-reference?hl=zh-tw#top_of_page)</div>

***
## DOM BREAK ON

介紹 `DOM` 的中斷點模式，分別有三種觸發模式可選（可複選）

1. subtree modifications: 當子元素點發生變化時
2. arrtibute modifications: 當元素發生變化時
3. node removal: 當元素被移除時

{% asset_img DOM_Break_on.png %}

***
## CONSOLE

### console.log()

除了一般我們最常用的 `log` 之外，還可另外新增 變數 ，增加訊息的變化

- %s：可帶入 指定的參數
- %c：可新增 css樣式

{% asset_img log.png %}

### console.warn()

log 多顯示為 <font color="orange">警告icon</font>

{% asset_img warn.png %}

### console.error()

log 多顯示為 <font color="red">錯誤icon</font>

{% asset_img error.png %}

### console.info()

<font color="red">失效</font>，沒有顯示「帶白色“i”的藍色圓圈」

{% asset_img info.png %}

### console.assert()

可額外用來判斷，若條件為 `false` 時，才會顯示 <font color="red">錯誤訊息</font>。

{% asset_img assert.png %}

### console.clear()

清除全部log。

{% asset_img clear.png %}

### console.dir()

可顯示出物件的<font color="red">細節資料</font>，如：DOM元素、Function...

{% asset_img dir.png %}

### console.groupCollapsed() & console.groupEnd()

可將一群相關訊息打成一包，這樣較易 檢查確認。

{% asset_img group.png %}

### console.count()

可顯示 累加出現的次數。

{% asset_img count.png %}

### console.time() & console.timeEnd()

可用於計算一段程式碼執行時，所花費的時間。

{% asset_img time.png %}

### console.table()

可將陣列的資料，用 table 方式顯示，易於觀看。

{% asset_img table.png %}

