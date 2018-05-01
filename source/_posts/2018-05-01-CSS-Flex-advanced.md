---
title: CSS-Flex-進階
date: 2018-05-01 23:41:03
categories:
- CSS
tags:
- CSS
- Flex
- 排版
---


{% cq %}

{% asset_img flex-property.png 常用屬性 %}

<font style="font-size:20px;">這次主要介紹 <font color="red">較不常用</font> 的屬性</font>

{% endcq %}

<!-- more -->
***
## 前言

在Youtube直播看到 Amos大大 在解說Flex各種屬性及運用，
在此紀錄自己較不熟悉的部份，方便以後複習。

<div class="note info">Youtube：[玩轉 CSS FLEX](https://youtu.be/_nCBQ6AIzDU)</div>

***
## 主軸&次軸

{% asset_img flex-direction.png %}

### flex-direction：決定 flex 主軸線 方向
[ row | row-reverse | column | column-reverse ]

在開始說明前，需再次強調一次 <font color="red">主軸與次軸(交錯軸)</font> 的重要性，
因為當主軸設定為 `row` 或 `column` 的話，
都會連帶影響其他屬性的設定，而產生不一樣的排版出來。

舉例來說，我們拿 `flex-basis` 這個屬性出來說明，
當主軸的方向有所不同時，會影響什麼呢？

當`flex-direction: row`時，`flex-basis`是影響<font color="red">寬度</font>
當`flex-direction: column`時，`flex-basis`是影響<font color="red">高度</font>


<div class="note warning">整理幾個時間點，方便以後回頭再複習。
[align-items(1h13m12s)](https://youtu.be/_nCBQ6AIzDU?t=1h13m12s)
[align-self(1h25m19s)](https://youtu.be/_nCBQ6AIzDU?t=1h25m19s)
[flex-order(1h29m13s)](https://youtu.be/_nCBQ6AIzDU?t=1h29m13s)
[flex-grow(1h39m25s)](https://youtu.be/_nCBQ6AIzDU?t=1h39m25s)
[flex-basis(1h43m15s)](https://youtu.be/_nCBQ6AIzDU?t=1h43m15s)
[flex-shrink(1h46m55s)](https://youtu.be/_nCBQ6AIzDU?t=1h46m55s)</div><div class="note info">[舊文參考：CSS - Flex](https://kanboo.github.io/2017/09/30/CSS-Flex/)</div>

***
## flex-order


<span id="inline-blue">重點整理</span>
- 預設值為 0 ，所以要往前就要小於 0，往後就要大於 0。
- 設定值需為 整數，小數無效，如：1.5。

<span id="inline-green">運用情境</span>

有 A B C 三塊資訊要顯示，而 <font color="red">B</font> 為最重要的資訊

在 電腦 排序要 A <font color="red">B</font> C (電腦是横的看)
在 手機 排序要 <font color="red">B</font> A C (手機是直的看)
在 平板 排序要 C <font color="red">B</font> A (←老板來亂指定的)

這時就可以透過 `media query` 和 `flex-order` 的配合，
在不同裝置上，顯示不同的排序。

> 時間點：[flex-order(1h29m13s)](https://youtu.be/_nCBQ6AIzDU?t=1h29m13s)

<div class="note primary">補充：
因為 髒髒 的關係，沒事別出大絕，而且也要一一去設定，怕以後維護不易。</div>

***
## flex-grow

將剩餘的空間，切成 n 份後，再分配出去。

<span id="inline-green">案例說明</span>

當 <font color="red">子</font>元素全部加總的長度(<font color="blue">600px</font>) <font color="red"><</font> 父元素的總長度(<font color="blue">1000px</font>) 時，
此時就有「<font color="red">剩餘的空間(1000 - 600 = 400)</font>」，可以讓`flex-grow`依照比例去分配剩下的空間。


> 時間點：[flex-grow(1h39m25s)](https://youtu.be/_nCBQ6AIzDU?t=1h39m25s)

<div class="note primary">備註：
Amos大提到用在單列使用時，做分配還OK，
但用在多列使用時，分配起來會跟想像中有點不一樣，較不好掌控。
先記一下，以後遇到時，踩到此坑才不會太痛。</div>

<div class="note info">JS30有練習到 => [JS30-05-Flex-Panel-Gallery](https://kanboo.github.io/2018/02/26/JS30-05-Flex-Panel-Gallery/)</div>


***
## flex-basis

<span id="inline-blue">重點整理</span>

- 預設值為 auto
- 控制 主軸的長度
  - flex-direction：決定 flex 主軸線 方向
  - 當主軸是 <font color="red">row</font> 的話，就是控制 <font color="red">寬度</font>
  - 當主軸是 <font color="blue">column</font> 的話，就是控制 <font color="blue">高度</font>

> 時間點：[flex-basis](https://youtu.be/_nCBQ6AIzDU?t=1h43m15s)

<span id="inline-green">額外測試</span>

<span id="inline-toc">Q</span>flex-basis & max-width & width 權重誰大？

<span id="inline-toc">A</span>max-width > flex-basis > width

> 權重測試：[codepen連結](https://codepen.io/Kanboo/pen/deNyXo)

***
## flex-shrink


<span id="inline-blue">重點整理</span>
- 預設值為 1 ，
- 值設定為 0 的話，代表 不給縮，就以原來值為主。
- 不可以為負值

<span id="inline-green">運用情境</span>

頁面切版有 左、右 二區，左邊是 menu，右邊是 內容區，
這時我們就可以設定

左邊的 flex-shrink: 0;
右邊的 flex-shrink: 1;

這樣的話，左邊就會固定寬度，右邊就會自動調整 收縮比。

承上例，
因 flex-shrink 是控制收縮，所以再新增 flex-grow 控制伸展，
這樣不管畫面拉寬拉窄的話，右邊內容區都可以自動伸展收縮。


<span id="inline-purple">公式</span>

先將下列三個值算出

<font color="blue">**收縮比**</font>：flex-shrink

<font color="blue">**總比值**</font>：各子項目寬*收縮值，並全部加總的值

<font color="blue">**超出值**</font>：全部子項目寬度的加總 超過 父層的寬度 的值

最後我們再利用上列的值，針對 每一個的子項目 算出 自己的扣除值

<font color="blue">**扣除值**</font>： (子項目的寬 \* 收縮比 / 總比值) \* 超出值

這樣的話，每個子項目的寬度去減掉自己的扣除值，就完成收縮的作用了。


> 時間點：[flex-shrink](https://youtu.be/_nCBQ6AIzDU?t=1h46m55s)


***

## 排版問題

<span id="inline-toc">Q</span>如何使用space-between，但我最後一列沒滿...我不想中間空一格該怎麼做？

<span id="inline-toc">A</span>

解法1：
- [文章：Flex-box: Align last row to grid](https://stackoverflow.com/questions/18744164/flex-box-align-last-row-to-grid)
- [codepen解法](https://codepen.io/DanAndreasson/pen/ZQXLXj)

解法2：
- [Flexbox - last row in grid](https://codepen.io/tuxsudo/pen/VYERQJ)