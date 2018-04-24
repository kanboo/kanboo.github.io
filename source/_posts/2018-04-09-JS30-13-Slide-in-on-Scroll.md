---
title: JS30-13-Slide-in-on-Scroll
date: 2018-04-09 13:43:38
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img scroll.png %}

<font style="font-size:20px;">滾動捲軸，使圖片滑動顯示。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 當滾動捲軸到「<font color="red">特定定點</font>」時，使圖片滑動顯示。


## 實踐步驟

1. CSS的動畫效果
    - transition
    - transform、translateX、scale

2. 監聽scroll滾動事件，並取得相關高度的資訊，進行判斷
    - `window.scrollY`
    - `window.innerHeight`
    - `HTMLElement.height`
    - `HTMLElement.offsetTop`

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/13%20-%20Slide%20in%20on%20Scroll/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/13%20-%20Slide%20in%20on%20Scroll/index.html)


***
## JS學習紀錄

### 減緩呼叫function的時間

因為監聽scroll的事件，當畫面滾動時，會在短時間觸發好幾次Event，
所以為了降低對效能上的影響，作者寫了一個`debounce`的函式，
用來降低觸發的間隔時間。

``` js
// 減緩呼叫function的時間
function debounce(func, wait = 20, immediate = true) {
  var timeout;
  return function() {
  var context = this, args = arguments;
  var later = function() {
      timeout = null;
      if (!immediate) func.apply(context, args);
  };
  var callNow = immediate && !timeout;
  clearTimeout(timeout);
  timeout = setTimeout(later, wait);
  if (callNow) func.apply(context, args);
  };
}

// 呼叫 funtion
window.addEventListener('scroll', debounce(handleShowImage));
```

### 各種高度的取得及運用

程式的邏輯：
  1. 當 `window.scrollY` 移動到 圖片一半以上 的位置時，將圖片<font color="blue">顯示</font>
  2. 當 圖片底部 已超過 `window.scrollY` 的位置時，將圖片<font color="blue">隱藏</font>

``` js 高度的運用
function handleShowImage(e){
  slideImages.forEach( sliderImage => {
    // 取得 圖片1/2的座標點（卷軸垂直位移量＋視窗高度）- 1/2圖片高度
    const slideInAt = (window.scrollY + window.innerHeight) - sliderImage.height / 2;
    // 取得 圖片底部座標點（圖片頂部座標點 + 圖片高度）
    const imageBottom = sliderImage.offsetTop + sliderImage.height;

    // 判斷 視窗 是否已經超過 圖片高度一半
    const ishalf = slideInAt > sliderImage.offsetTop;
    // 判斷 滾動範圍 是否已經超過 圖片底部
    const isNotOver = imageBottom > window.scrollY;

    // 超過圖片高度一半 且 未超過圖片底部，則顯示
    if ( ishalf && isNotOver){
      sliderImage.classList.add('active');
    }else{
      sliderImage.classList.remove('active');
    }
  })
}
```

- [window.scrollY](https://developer.mozilla.org/en-US/docs/Web/API/Window/scrollY)
目前瀏覽器視窗<font color="red">已滾動</font>的Y軸（垂直位置）

- [window.innerHeight](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerHeight)
目前瀏覽器視窗的高度(<font color="red">不含</font> 上方功能列及開發者工具 區塊)

- [HTMLElement.offsetTop](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/offsetTop)
取得 DOM元素 相對於在<font color="red">父元素</font>頂部距離的位置

- [HTMLImageElement.height](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-height)
圖片本身的高度(此屬性不是每個DOM元素都有)

<span id="inline-yellow">可額外參考的方法：</span>

- [Element.getBoundingClientRect()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)
取得 目標元素的 大小 與 相對於瀏覽器視窗的位置資訊


