---
title: JS30-05-Flex-Panel-Gallery
date: 2018-02-26 11:19:54
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img Gallery.png %}

<font style="font-size:20px;"> <font color="red">JS-Event 與 CSS-Flex</font> 的搭配使用</font>

{% endcq %}

<!-- more -->
***

## 目標

- 使用 CSS的 <font color="red">flex</font>、transform、transition.. 等屬性。
- 透過 JS 監聽事件(click、transitionend)，修改CSS類別(<font color="red">classList</font>)，達到不同效果。

## 實踐步驟

1. 將HTML的 `panels` 底下的 `5個panel`，利用 <font color="red">flex</font> 將版型排好
    - flex：flex-grow flex-shrink flex-basis

2. 分別監聽(click、transitionend) 三個 panel
    - 利用 `DOM.classList.toggle` 新增、移除 CSS類別


## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/05%20-%20Flex%20Panel%20Gallery/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/05%20-%20Flex%20Panel%20Gallery/index.html)

***
## CSS學習紀錄

### flex: flex-grow flex-shrink flex-basis

在範例中，有運用到 Flex 的一個css屬性(flex)，其參數如下

<span id="inline-blue">語法：</span>

``` css flex的簡寫參數
flex: flex-grow flex-shrink flex-basis
```

<span id="inline-purple">屬性：</span>
 - flex-grow：元件的伸展性
 - flex-shrink：元件的收縮性
 - flex-basis：元件的基準值

而在範例中，主要用到第一個參數 `flex-grow`，在此稍微說明其定義

當 <font color="red">子</font>元素全部加總的長度(<font color="blue">600px</font>) <font color="red"><</font> 父元素的總長度(<font color="blue">1000px</font>) 時，
此時就有「<font color="red">剩餘的空間(1000 - 600 = 400)</font>」，可以讓`flex-grow`依照比例去分配剩下的空間。

承上述，
若是 子元素全部的長度 <font color="red">>=</font> 父元素的長度 時，
此時就「<font color="red">無剩餘的空間</font>」可分配，這種情況下`flex-grow` 就無用了，


再承上述，
若 子元素總長度 <font color="red">超過</font> 父元素長度時，有可能會造成跑版，
這時就可以用第二個參數 `flex-shrink`，來<font color="red">壓縮</font>子元素的長度。

<div class="note info">[CSS Flex 屬性一點也不難](https://wcc723.github.io/css/2017/07/21/css-flex/)
[flex-grow 不易理解，难道不是吗?](https://jinlong.github.io/2016/02/04/flex-grow-is-weird/)
[深入解析 CSS Flexbox](http://www.oxxostudio.tw/articles/201501/css-flexbox.html)</div>

***
## JS學習紀錄

### event.propertyName

在監聽 `transitionend` 事件時，主要是判斷增加 `Flex: 5` 的效果是否已結束，
進而觸發另一事件，新增另一個CSS效果(文字滑入)，
在[JS30-01-JavaScript-Drum-Kit](https://kanboo.github.io/2018/02/13/JS30-01-JavaScript-Drum-Kit/)已有練習使用過，在此重新複習一次。

圖：event.propertyName 取得 CSS屬性名稱
{% asset_img TransitionEvent_02.png %}

<div class="note info">[transitionend](https://developer.mozilla.org/en-US/docs/Web/Events/transitionend)
[MDN-Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)</div>

### includes

在影片中，作者有提到 `transition: flex 0.7s..` 這一段，
在sarafi是 `flex`，而其他瀏覽器為 `flex-grow`，不過二個都有 `flex`這個字眼，
所以利用 `.includes('flex')` 來判斷 flex的css效果 是否已結束。


``` css CSS-flex名稱
.panel {
    /* Safari transitionend event.propertyName === flex */
    /* Chrome + FF transitionend event.propertyName === flex-grow */
    transition:
        font-size 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
        flex 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
        background 0.2s;
}
```

``` js JS-includes
function toggleActie(e){
    // 因 Safari 和 Chrome + FF 顯示 propertyName 有差異
    /* Safari transitionend event.propertyName === flex */
    /* Chrome + FF transitionend event.propertyName === flex-grow */
    // console.log(e);

    if ( e.propertyName.includes('flex')){
        // console.log(this);
        this.classList.toggle('open-active');
    }
}
```

***
## 延伸：點擊其他panel時，關閉已展開panel

若目前畫面已有展開某一panel時，在點擊其他panel時，需要將前一個panel的CSS效果移除。

<span id="inline-purple">解法：</span>

預達到此需求，我先想到 jQuery 的 `.siblings()` 可使用，但若想用純正的 `Vanilla JS` 達成，Javascript有 [Node.nextSibling](https://developer.mozilla.org/en-US/docs/Web/API/Node/nextSibling) 和 [Node.previousSibling](https://developer.mozilla.org/en-US/docs/Web/API/Node/previousSibling) 可運用，
不過已經有別人造好輪子的話，那就...改用大神寫好的來使用，
感謝 [get-elements-siblings-with-vanilla-javascript](https://gomakethings.com/how-to-get-an-elements-siblings-with-vanilla-javascript/)。


``` js 移除鄰邊的CSS
function toggleOpen(e){
    // console.log(this);
    this.classList.toggle('open');

    // 移除鄰邊的CSS
    // 參考網址：https://gomakethings.com/how-to-get-an-elements-siblings-with-vanilla-javascript/
    // DOM.nodeType：元素中的空行或者空格會作為文本節點，返回"#text"
    var sibling = this.parentNode.firstChild;
    for (; sibling; sibling = sibling.nextSibling) {
        if (sibling.nodeType !== 1 || sibling === this) continue;
        // console.log(sibling);
        sibling.classList.remove('open');
    }
}
```

<span id="inline-yellow">其他參考解法</span>

GuaHsu大大的思維：
紀錄最後一個點擊的Panel，當下一次點擊的Panel不一樣時，則除移CSS效果。

``` js
//宣告一個上次點擊的Panel，預設先給他panels
let lastClickPanel = document.querySelector('.panels');
function toggleOpen() {
    //每次檢查進入的element與上次進入的element是不是相同
    //若不相同，則把上次點擊的element移除opev效果
    //再把lastClickPanel指向為這次的elment
    if (this !== lastClickPanel) {
        lastClickPanel.classList.remove('open');
        lastClickPanel = this;
    }

    this.classList.toggle('open');
}
```

<div class="note info">[GuaHsu](https://guahsu.io/2017/05/JavaScript30-05-Flex-Panel-Gallery/)</div>

