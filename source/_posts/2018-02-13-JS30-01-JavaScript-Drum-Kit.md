---
title: JS30-01-JavaScript-Drum-Kit
date: 2018-02-13 10:59:37
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img drum-kit.jpg %}

<font style="font-size:20px;">按下鍵盤時，觸發聲音及特效</font>

{% endcq %}

<!-- more -->
***


## 目標

- 按下鍵盤時，播放聲音、顯示CSS效果
- 播放完畢後，移除CSS效果

## 實踐步驟

1. 先監聽 鍵盤 的 `keydown` Event
    - 利用 `e.keyCode` 取得符合的 audio標籤 和 div標籤
    - audio 標籤 => 播放 聲音
    - div 標籤 => 顯示 CSS效果

2. 監聽 CSS 的 `transitionend` Event (transitionend 事件會在 transition 结束后觸發)
    - `e.propertyName !== "transform"` 僅針對 transform 繼續做事，不是則停止
    - 若為 transform，則移除 CSS效果

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/01%20-%20JavaScript%20Drum%20Kit/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/01%20-%20JavaScript%20Drum%20Kit/index.html) 

***
## JS學習紀錄

### HTML5標籤 HTMLMediaElement

透過 js 取得 HTMLMediaElement 元素，來進行影音的播放。

``` html HTML
<audio data-key="65" src="sounds/clap.wav"></audio>
```

``` js JS
//取得 HTMLMediaElement 元素
const audio = document.querySelector(`audio[data-key="${keyCode}"]`);

audio.currentTime = 0 ; //設定音效從 0 秒開始
audio.play();  // 播放音效
```

<div class="note info">[MDN-HTMLMediaElement](https://developer.mozilla.org/zh-TW/docs/Web/API/HTMLMediaElement)
[HTMLMediaElement.play](https://developer.mozilla.org/en-US/docs/Web/Events/play)
[HTMLMediaElement.currentTime](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/currentTime)</div>

***
### DOM元素 Element.classList

透過 `classList` 新增、移除、切換 CSS屬性，就像jQuery的 `addClass`、`removeClass` 一樣。

``` html HTML
<div data-key="65" class="key">
    <kbd>A</kbd>
    <span class="sound">clap</span>
</div>
```

``` js JS
//取得 DOM 元素
const key = document.querySelector(`.key[data-key="${keyCode}"]`);

key.classList.add('playing');    //新增CSS屬性
key.classList.remove('playing'); //移除CSS屬性
key.classList.toggle('playing'); //切換CSS屬性
```

<div class="note info">[Element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)</div>

***
### DOM元素 NodeList

使用 `querySelectorAll` 取得的DOM，返回的結果是 `NodeList` 型態，非 Array 型態，所以不能使用 Array的method，若要使用Array的method的話，就需轉換成Array型態。

``` html HTML
<div data-key="65" class="key">
    <kbd>A</kbd>
    <span class="sound">clap</span>
</div>
<div data-key="83" class="key">
    <kbd>S</kbd>
    <span class="sound">hihat</span>
</div>
<div data-key="68" class="key">
    <kbd>D</kbd>
    <span class="sound">kick</span>
</div>
```
querySelectorAll 取得 DOM元素後，使用 NodeList.prototype.forEach() 的 method，有可能會在部份瀏覽器不支援。

``` js JS
// 取得 DOM 元素 => NodeList 型態
const keys = document.querySelectorAll('.key'); 

// NodeList.prototype.forEach() => IE不支援
keys.forEach(key => key.addEventListener('transitionend',removeTransition))
```

{% asset_img NodeList_foreach.png %}

可藉由下列方法，將 NodeList 轉換 Array型態，支援度較高。

``` js NodeList 轉換 Array型態
// 第一種方法：Array.from()
const keys = Array.from(document.querySelectorAll('.key')); 

// 第二種方法：... 展開運算子( Spread Operator )
const keys = [...document.querySelectorAll('.key'))]; 


// 轉換成 Array型態 後，就可以使用 forEach()、map(), concat() …等method
keys.forEach(key => key.addEventListener('transitionend',removeTransition))

```

<div class="note info">[NodeList.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/API/NodeList/forEach)
[Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
[... 展開運算子(Spread Operator)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)</div>

***
### CSS監聽事件 transitionend

透過 CSS `transitionend` Event ，當 transition 效果結束後，就會觸發此事件，

``` js JS
function removeTransition(e){
    console.log(e);  //有時會多個 CSS效果 一同執行，參考下圖1
    console.log(e.propertyName); //可透過 event.propertyName 取得想要監聽的CSS屬性，參考下圖2

    if(e.propertyName !== "transform") return; // 僅針對 transform，不是則停止

    /* your code */
}

const keys = Array.from(document.querySelectorAll('.key')); 
keys.forEach(key => key.addEventListener('transitionend',removeTransition)) // transitionend 事件會在 CSS transition 结束后觸發
```

圖1：同時多個 CSS效果
{% asset_img TransitionEvent_01.png %}

圖2：event.propertyName 取得 CSS屬性名稱
{% asset_img TransitionEvent_02.png %}



<div class="note info">[transitionend](https://developer.mozilla.org/en-US/docs/Web/Events/transitionend)
[MDN-Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)</div>

***
### 一般函式的this(作為 DOM 事件偵聽函式 → 該 DOM)

在 `removeTransition` function裡面有使用 `this` 去移除CSS屬性，該如何判斷這個 `this` 是指向誰呢？

答案是：一般函式的this(作為 DOM 事件偵聽函式 → 該 DOM)


``` js this?
function removeTransition(e){
    if(e.propertyName !== "transform") return; // 僅針對 transform，不是則停止

    // 一般函式的this(作為 DOM 事件偵聽函式 → 該 DOM)
    this.classList.remove('playing'); //移除CSS屬性
}

const keys = Array.from(document.querySelectorAll('.key')); 
keys.forEach(key => key.addEventListener('transitionend',removeTransition)) 
```

<div class="note info">[JS-一次搞懂 JavaScript 的 this](https://kanboo.github.io/2018/01/24/JS-this/)</div>