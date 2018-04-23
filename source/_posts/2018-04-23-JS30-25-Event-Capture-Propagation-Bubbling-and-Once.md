---
title: JS30-25-Event-Capture-Propagation-Bubbling-and-Once
date: 2018-04-23 16:16:50
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js25.jpg %}

<font style="font-size:20px;">了解 <font color="red">addEventListener</font> 的運作</font>

{% endcq %}

<!-- more -->
***

## 目標

- 了解 <font color="red">addEventListener</font> 的運作
 - 事件冒泡 (Event Bubbling)
   - 停止冒泡行為 e.stopPropagation()
 - 事件捕獲 (Event Capturing)
 - 單次觸發

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/25%20-%20Event%20Capture,%20Propagation,%20Bubbling%20and%20Once/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/25%20-%20Event%20Capture,%20Propagation,%20Bubbling%20and%20Once/index.html)

***
## 學習紀錄

### 事件傳遞順序

使用 `addEventListener` 時，不管是設定 `Event Bubbling` 或 `Event Capturing`，
當Event被觸發時，都是會先從 <font color="red">最外層DOM 走到 target的DOM 然後再走回 最外層DOM</font>，
所以當你設定
 - Event Bubbling(<font color="green">綠色路徑</font>)：只回傳 **target的DOM →→ 最外層DOM** 的路徑
 - Event Capturing(<font color="red">紅色路徑</font>)：只回傳 **最外層DOM →→ target的DOM** 的路徑

{% asset_img js_04.png %}

所以根據上面的規則，套用在下面的案例的話

``` html HTML
<div class="one purple">
  紫色
  <div class="two pink">
    粉色
    <div class="three orange">
      橘色
    </div>
  </div>
</div>
```

**事件冒泡 (Event Bubbling)**：只回傳 **target的DOM →→ 最外層DOM** 的路徑

``` js 事件冒泡 (Event Bubbling)
function logText(e) {
  // 印出當前div的class name
  console.log(this.classList.value);
}

// 第三個參數 useCapture：預設為 false → 事件冒泡 (Event Bubbling)
divs.forEach(div => div.addEventListener('click', logText));

/*
console列出順序為(target的DOM →→ 最外層DOM)

three orange
two pink
one purple
*/
```

**事件捕獲 (Event Capturing)**：只回傳 **最外層DOM →→ target的DOM** 的路徑

``` js 事件捕獲 (Event Capturing)
function logText(e) {
  // 印出當前div的class name
  console.log(this.classList.value);
}

// 第三個參數 useCapture 修改為 true → 事件捕獲 (Event Capturing)
divs.forEach(div => div.addEventListener('click', logText, true));

/*
console列出順序為(最外層DOM →→ target的DOM)

one purple
two pink
three orange
*/
```

<div class="note info">[JS-事件機制的原理](https://kanboo.github.io/2018/01/15/JS-Bubbling-Capturing/)</div>

### 停止冒泡行為 e.stopPropagation()

有時只是想單純針對單一元素監聽，不想因為事件冒泡的行為，
而去觸發到其他元素，這時就可利用 `e.stopPropagation()` 來達成此需求。

``` js e.stopPropagation()
function logText(e) {
  console.log(this.classList.value);

  // 停止冒泡行為！
  e.stopPropagation();
}
```

<div class="note warning">注意：
使用 `e.stopPropagation()` 僅針對 事件<font color="red">冒泡</font> (Event Bubbling) 設定，
若是設定為 事件<font color="red">捕獲</font> (Event Capturing) 的話，則不適用 `e.stopPropagation()`。</div>

### 單次觸發

once 屬性：當第一次被觸發後，就會移除本身的監聽事件，後續就沒有監聽事件。

目前想到的可運用在表單submit後，解除監聽事件，避免重覆送單。

``` js 單次觸發
// 在 button 的 addEventListener 第三個參數裡，設定 once 的屬性
// 當第一次被觸發後，就會移除本身的監聽事件，後續就沒有監聽事件。
button.addEventListener('click', () => {
  console.log('Click!!!');
}, {
  once: true
});
```

<div class="note info">[EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)</div>