---
title: JS30-28-Video-Speed-Controller
date: 2018-04-26 00:58:20
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js28.jpg %}

<font style="font-size:20px;">用滑鼠控制影片播放速率</font>

{% endcq %}

<!-- more -->
***

## 目標

- 用滑鼠控制影片播放速率。

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/28%20-%20Video%20Speed%20Controller/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/28%20-%20Video%20Speed%20Controller/index.html)

***
## 學習紀錄

此次重點在於

1. 滑鼠在 <font color="red">速率條移動</font> 時，如何取得正確的座標
2. <font color="red">滑鼠移動的座標</font> 與 <font color="red">速率條</font> 二者之間，如何計算出比例轉換成 <font color="blue">速率值</font>

``` js 整段程式碼
const speed = document.querySelector('.speed');
const bar = speed.querySelector('.speed-bar');
const video = document.querySelector('.flex');

speed.addEventListener('mousemove', function(e){
  // console.log(e);

  // 取得滑鼠在元素移動的座標
  const y = e.pageY - this.offsetTop;
  // 計算滑鼠移到的座標點，佔元素比例是多少
  const percent = y / this.offsetHeight;
  // 比例 轉成 百分比
  const height = Math.round(percent * 100) + '%';
  // 修改 速度條 的高度
  bar.style.height = height;

  // 設定 最慢速率、最快速率
  const min = 0.4;
  const max = 4;

  // 取得播放速率(最慢0.4倍，最多4倍速)
  const playbackRate = percent * (max - min) + min;
  // 最多取得小數點後兩位
  bar.textContent = playbackRate.toFixed(2) + '×';
  // 控制影片的速率
  video.playbackRate = playbackRate;
})
```