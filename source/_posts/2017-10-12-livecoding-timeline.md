---
title: LiveCoding-切個垂直時間軸
date: 2017-10-12 23:06:34
categories: 
- 切版
tags:
- LiveCoding
- timeline
- 切版
- 排版
- scss
---

## 教學來源


Youtube：[直播 - 切個垂直時間軸](https://youtu.be/0Y7D-ZayhmA)

* GitHub 分支：https://goo.gl/VEbWkm
* 參考版型：https://goo.gl/rqjErS
* 螢幕吸顏色 Sip：https://goo.gl/Vh6YIo
* LiveCoding 官方網站：https://goo.gl/weYx5q
* LiveCoding Youtube 頻道：https://goo.gl/Hyih3u
* LiveCoding 臉書粉絲專頁：https://goo.gl/yhDg0l


## 排版小技巧紀錄

* 切版過程中，可常利用新增「外層的紅框線」，先確認位置是否正確，事後再拿掉。

    `border: 1px solid rgba(255, 0, 0, .3)`

* CSS之calc可做運算式運算

    `calc(50% — 10px / 2)`

<!-- more -->

* z-index

    z-index 記得搭配 **position : relative 、 absolute** 使用
    z-index 數字越大的在越上面，反之則在越下面


* 行高

    ``` css
    H3{
        height: 45px;
        line-height: 45px;
    }
    ```
    > [深入 CSS 之 line-height 應用](http://muki.tw/tech/css-line-height/)

***

## 時間紀錄點

### 頭像
(10分開始)

### 頭像+白色圓外框+黑色的陰影
(12分30秒)


### 時間軸的 垂直中線 位置的偏移
(14分30秒開始)

### 按鈕(載入更多)
(28分19秒開始)

### 時間軸的icon
(38分30秒開始)


### 資訊卡之小箭頭

(1時3分30秒)

使用 [CSS Arrow Please](http://www.cssarrowplease.com/)，再修改部份css

### 將 資訊卡區塊 開始變 左右 排版
(1時7分 開始)

可用的方法有：
1. 針對左右給不同的 class
2. 用 js 控制
3. 用 nth-child

``` css
/* content底下的
第一子元素是:<i>
第二個是<button>
第三個是<div 資訊卡> */

/* 從content底下的子元素，從第3個元素開始並每次加2的倍數(奇數) */
:nth-child(2n + 3)

/* 從content底下的子元素，從第4個元素開始並每次加2的倍數(偶數) */
:nth-child(2n + 4)
```

### 用 js 動態新增資訊卡
(1h 15分 開始)

``` js
// 在div底下
// 新增img HTML
$('<div />').append ($('<img />')

// 新增 info 的Class
$('<div />').addClass ('info')

// 新增 data-time 屬性
$('<div />').attr ('data-time', "2017-10-10")

// 新增 純文字
$('<div />').text('我是單純顯示的文字')

```

### 資訊卡之圖片
(1時28分 開始)

``` scss
.content {
    content屬性設定
    
    .img {
        img屬性設定
        
        +h3 {
            h3屬性設定-特例
        }
    }
    
    .h3 {
        h3屬性設定
    }
}
```
注意：
1. img 底下有加一個 **+h3** 的設定
2. 另外 img 平行也有一個 h3 的設定

這裡的觀念為 **CSS權重**，下列有二個 **h3** 的設定

``` css
img+h3{
    屬性設定-特例
}

h3{
    屬性設定
}
```

雖然CSS通常都是 `權重一樣大` 的話，就是 **後面 蓋掉 前面 的屬性**，但是 img+h3 權重 > h3 權重，
所以 **img+h3** 不會被後面 **單一個h3** 屬性蓋掉。

> [CSS權重](http://kailian.github.io/2017/02/21/css-selector-level)


### 改寫 RWD的格式
(1時34分30秒)

### js 塞入html方式 改寫用 object(json格式)
(1時50分 開始)