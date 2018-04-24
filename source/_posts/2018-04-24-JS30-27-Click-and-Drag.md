---
title: JS30-27-Click-and-Drag
date: 2018-04-24 22:47:00
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js27.jpg %}

<font style="font-size:20px;">可用滑鼠點擊拖曳水平的畫面</font>

{% endcq %}

<!-- more -->
***

## 目標

- 可用滑鼠點擊拖曳水平的畫面。

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/27%20-%20Click%20and%20Drag/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/27%20-%20Click%20and%20Drag/index.html)

***
## 學習紀錄

重點在於如何計算**滑鼠點擊拖移畫面**時，是往左或往右，以及如何計算出要移動的定位點是多少。

### 一、取得相關元素及監聽滑鼠事件

``` js 取得相關元素及監聽滑鼠事件
  const slider = document.querySelector('.items');
  let isDown = false;  //紀錄 滑鼠是否點擊的狀態
  let startX;  // 紀錄 滑鼠最初點擊的位置
  let scrollLeft;

  // 滑鼠 點擊
  slider.addEventListener('mousedown', () => {});
  // 滑鼠 超出範圍
  slider.addEventListener('mouseleave', () => {});
  // 滑鼠 按鍵放開
  slider.addEventListener('mouseup', () => {});
  // 滑鼠 移動
  slider.addEventListener('mousemove', () => {});
```

### 二、分別針對不同滑鼠事件撰寫

主要處理的重點在 <font color="red">滑鼠點擊</font> 和 <font color="red">滑鼠移動</font> 這二個事件。

<span id="inline-blue">滑鼠 點擊</span>

``` js
// 滑鼠點擊
slider.addEventListener('mousedown', (e) => {
  isDown = true;
  slider.classList.add('active');

  // 紀錄 點擊初始位置
  // e.pageX：整個頁面的 x軸 距離
  // slider.offsetLeft：目前DOM位於父元素的X座標
  startX = e.pageX - slider.offsetLeft;

  // 紀錄 目前捲軸的左距
  scrollLeft = slider.scrollLeft;
});
```
<span id="inline-toc">Q</span>
其中 `startX` 這個變數的計算，為什麼不可直接使用 e.pageX 呢？
為何還要扣掉slider.offsetLeft？
<span id="inline-toc">A</span>
原因是 滑動捲軸 是出現在 `<div class="items">` 身上，而非在整個頁面，
所以當然只能計算在 `<div class="items">` 區塊裡，已移動了多少距離。

<span id="inline-blue">滑鼠 移動</span>

``` js
// 滑鼠移動
slider.addEventListener('mousemove', (e) => {
  // 非點擊狀態時，不作用
  if (!isDown) return;
  // 取消預設行為(點擊且拖移的動作，預設行為是 選取範圍)
  e.preventDefault();

  // 目前位置 = 整個頁面x軸距離 - 目前items的左邊距離
  const x = e.pageX - slider.offsetLeft;

  // 移動距離 = 目前位置 - 點擊初始位置
  const walk = x - startX;
  // const walk = (x - startX) * 3;  //乘3倍的概念，感覺像增加滑鼠的敏感度

  // 設定 .items區塊 的 水平捲軸偏移量
  slider.scrollLeft = scrollLeft - walk;
});
```

<span id="inline-blue">滑鼠 超出範圍</span>

``` js
// 滑鼠超出範圍
slider.addEventListener('mouseleave', () => {
  // 取消狀態及樣式
  isDown = false;
  slider.classList.remove('active');
});
```

<span id="inline-blue">滑鼠 點擊</span>

``` js
// 滑鼠按鍵放開
slider.addEventListener('mouseup', () => {
  // 取消狀態及樣式
  isDown = false;
  slider.classList.remove('active');
});
```
<div class="note info">[MouseEvent.pageX](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/pageX)
[Element.scrollLeft](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollLeft)</div>
