---
title: CSS-練習-自刻時鐘
date: 2017-12-20 22:35:37
categories: 
- CSS
tags:
- CSS
- 練習
- 時鐘
- clock
---

{% cq %}

{% asset_img clock_01.png %}
練習-自刻時鐘

{% endcq %}

<!-- more -->
***

## 步驟

1. 先建立骨架(HTML)

    ``` html
    <div class="warp">
        <div class="clock">
            <div class="centerpoint"></div>
            <div class="hour"></div>
            <div class="minute"></div>
            <div class="second"></div>
        </div>
    </div>
    ```

2. 將時鐘元件垂直置中

    將「圓點、時針、分針、秒針」移至時鐘的中心點。

    ``` css
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    ```

3. 修改transform-origin與position位置

    下列說明以 **時針** 為例(分針、秒針比照辦理)：

    1. 垂直置中後的 時針 css

    ``` scss
    $handwidth: 7px;

    .hour {
        width: $handwidth;
        height: 80px;
        background-color: blue;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
    ```

    2. 先修改 transform-origin 的軸心

    ``` diff
    .hour {
        width: $handwidth;
        height: 80px;
        background-color: blue;
    +   //將軸心變更到 中下方 的位置
    +   transform-origin: 50% 80%;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }

    ```

    3. 加上旋轉的動畫效果

    ``` diff
    .hour {
        width: $handwidth;
        height: 80px;
        background-color: blue;
        //將軸心變更到 中下方 的位置
        transform-origin: 50% 80%;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    +   animation: time 25s infinite linear;
    }

    +   //旋轉的動畫效果
    +   @keyframes time {
    +     to {
    +      transform: rotate(360deg);
    +     }
    +   }
    ```

    4. 修改 position 位置

    此時可以開啟Chrome的開發者工具，先利用工具微調好 時針 的position位置，讓他旋轉效果有繞著時鐘的中心點旋轉後，再將程式碼複製貼回去。

    ``` diff
    .hour {
        width: $handwidth;
        height: 80px;
        background-color: blue;
        //將軸心變更到 中下方 的位置
        transform-origin: 50% 80%;
        position: absolute;
    -   top: 0;
    +   top: -49px;
        bottom: 0;
    -   left: 0;
    +   left: -1px;
        right: 0;
        margin: auto;
        animation: time 25s infinite linear;
    }

    //旋轉的動畫效果
    @keyframes time {
        to {
            transform: rotate(360deg);
        }
    }
    ```

## 範例

<p data-height="364" data-theme-id="0" data-slug-hash="wpMVJK" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="練習-自刻時鐘" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/wpMVJK/">練習-自刻時鐘</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>