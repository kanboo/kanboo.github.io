---
title: JS30-21-Geolocation
date: 2018-04-21 09:54:55
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js21.jpg %}

<font style="font-size:20px;"> 取得裝置的地理位置和速度。</font>

{% endcq %}

<!-- more -->
***

## 目標

-  取得裝置的地理位置和速度。


## 實踐步驟

1. 利用 `navigator.geolocation` 取得裝置的地理位置和速度

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/21%20-%20Geolocation/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/21%20-%20Geolocation/index.html)


***
## JS學習紀錄

紀錄JS如何取得裝置的地理位置和速度。

``` js
// 取得DOM元素
const arrow = document.querySelector('.arrow');
const speed = document.querySelector('.speed-value');

// 取得裝置的地理位置和速度
navigator.geolocation.watchPosition((data) => {
  // 若有成功取得，則會回傳一組 Position
  console.log(data);

  // data.coords.speed 取得 速度(公尺/秒)
  speed.textContent = data.coords.speed;

  // data.coords.heading 取得 角度(0為正北、90為正東)
  arrow.style.transform = `rotate(${data.coords.heading}deg)`;
}, (err) => {
  console.error(err);
});
```

<div class="note info">[Geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)</div>