---
title: JS30-08-Fun-with-HTML5-Canvas
date: 2018-03-01 16:55:46
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img canvas.png %}

<font style="font-size:20px;">用滑鼠在 <font color="red">Canvas</font> 上作畫</font>

{% endcq %}

<!-- more -->
***

## 目標

- 使用HTML5的 `Canvas` 來製作一個畫布
- 透過 <font color="red">滑鼠</font> 可達到作畫的效果

## 實踐步驟

1. 建立 `canvas` 的區塊，設定為 `2D` 環境，並設定相關屬性
    - strokeStyle、lineJoin、lineCap、lineWidth

2. 透過 JS 設定 canvas 會應用到的相關變數
    - canvas的顏色、線條粗細、座標...等
    - function draw(e)

2. 監聽 滑鼠 的事件
    - 開始作畫：mousedown
    - 作畫中..：mousemove
    - 結束作畫：mouseup、mouseout

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/08%20-%20Fun%20with%20HTML5%20Canvas/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/08%20-%20Fun%20with%20HTML5%20Canvas/index.html)


***
## canvas學習紀錄

### 設定線條樣式

下列為這次 `canvas` 作畫用到的屬性

- `strokeStyle` 線條顏色
- `lineJoin` 線條轉彎的樣式
- `lineCap` 線條收尾的樣式
- `lineWidth` 線條寬度

``` html HTML
<canvas id="draw" width="800" height="800"></canvas>
```

``` js JS-canvas設定
const canvas = document.querySelector('#draw');
const ctx = canvas.getContext('2d'); // 宣告為 2D 渲染環境(e.g. 2D、webgl、webg2、bitmaprenderer)

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

ctx.strokeStyle = 'blue'; // 線條顏色
ctx.lineJoin = 'round'; // 線條轉彎的樣式(e.g. round(圓角)、bevel(去斜角)、、miter(尖形))
ctx.lineCap = 'round'; // 線條收尾的樣式(e.g. butt(短方形)、round(圓形)、square(長方形))
ctx.lineWidth = 25;
ctx.globalCompositeOperation = 'source-in'; // 當有重疊的部份，如何呈現(e.g. source-over、screen、multiply、etc.)
```

### 作畫步驟

利用JS完成作畫的步驟，如下

1. `beginPath()` 宣告開始作畫
2. `moveTo()` 作畫的起點
3. `lineTo()` 作畫的終點
4. `stroke()` 完成作畫

``` js 作畫部份程式碼
ctx.beginPath();
ctx.moveTo(lastX, lastY);   // start from
ctx.lineTo(e.offsetX, e.offsetY); // go to
ctx.stroke();
```

<div class="note info">[MDN-CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D)
[CanvasRenderingContext2D.globalCompositeOperation](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation)</div>

***
## JS學習紀錄

### DOM監聽事件之this

此次範例，作者在 <font color="red">DOM監聽事件</font> 使用了 <font color="red">箭頭函式</font>，雖然看似沒什麼情況，不過有一點要注意，若是有要對本身的DOM做修改的話，`this` 這個變數就會有所差異，如下例所示。

``` js 箭頭函式
canvas.addEventListener('mousedown', (e) => {
    console.log(this); // window
});
```

``` js 一般函式
canvas.addEventListener('mousedown', function(e) {
    console.log(this); // DOM元素
});
```

<div class="note info">[JS-一次搞懂 JavaScript 的 this](https://kanboo.github.io/2018/01/24/JS-this/)</div>