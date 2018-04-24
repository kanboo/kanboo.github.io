---
title: JS30-18-Adding-Up-Times-with-Reduce
date: 2018-04-18 17:12:03
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js18.jpg %}

<font style="font-size:20px;">利用 map() 與 reduce() 來計算播放清單的總秒數。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 利用 map() 與 reduce() 來計算播放清單的總秒數。


## 實踐步驟

1. 取得所有 data-time 的DOM元素，並轉換成 Array，以便可使用map..等method
2. 計算出總秒數
  2-1. 取得 時間(分、秒)
  2-2. 將時間(分、秒)轉化成數字型態，且算成 秒數
  2-3. 最後將每一首的秒數加總
3. 將 總秒數 計算成 時、分、秒 顯示
  3-1. 先計算 小時，只取 商數
  3-2. 剩下的秒數，再計算 分鐘，只取 商數
  3-3. 最後就是剩餘的 秒數

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/18%20-%20Adding%20Up%20Times%20with%20Reduce/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/18%20-%20Adding%20Up%20Times%20with%20Reduce/index.html)


***
## JS學習紀錄

此次重點在於

1. 運用「map」
2. 如何精進Sort的寫法

### 一、取得所有要計算時間的元素

``` js 取得所有要計算時間的元素
/* 1. 取得所有 data-time 的DOM元素，並轉換成 Array，以便可使用map..等method */

const timeNodes = Array.from(document.querySelectorAll('[data-time]'));
/* 可簡化如下 */
const timeNodes = [...document.querySelectorAll('[data-time]')];
```

### 二、計算出總秒數

``` js 計算出總秒數
/* 2. 計算出總秒數 */
const seconds = timeNodes
  // 取出每個元素中的data-time資料
  .map( node => node.dataset.time )
  // 將 分、秒 拆解並轉成 秒數
  .map( time => {
    // const [mins, secs] = time.split(':').map( str => parseFloat(str));
    /* 可簡化如下 */
    const [mins, secs] = time.split(':').map(parseFloat);

    return (mins * 60) + secs
  })
  // 用reduce來加總每次執行結果
  .reduce( (total , second) =>  total + second)
```

### 三、.將 總秒數 計算成 時、分、秒 顯示

``` js 將 總秒數 計算成 時、分、秒 顯示
/* 3. 將 總秒數 計算成 時、分、秒 顯示 */
// 使用Math.floor取整數，再利用%來操作餘數

let secondsLeft = seconds;
// 計算 時數
const hours = Math.floor(secondsLeft / (60 * 60));

// 扣掉 時數
// secondsLeft = secondsLeft - (hours * 3600);
secondsLeft = secondsLeft % 3600;

// 計算 分鐘
const minutes = Math.floor(secondsLeft / 60);

// 扣掉 分鐘，剩下的就是 秒數
// secondsLeft = secondsLeft - (minutes * 60);
secondsLeft = secondsLeft % 60;
```

### Array.map() 呼叫 function

作者在範例中，在`Array.map()`又Call Function，還可以再簡化寫法，

不過目前對我還不是很直覺可以寫出這種，先在此紀錄一下。

``` js
const [mins, secs] = time.split(':').map( str => parseFloat(str));
// 可簡化如下
const [mins, secs] = time.split(':').map(parseFloat);
```