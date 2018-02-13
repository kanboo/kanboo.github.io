---
title: JS30-01-JavaScript-Drum-Kit
tags:
---


## 目標

- 按鍵觸發時，播放聲音、顯示CSS效果
- 播放完畢後，移除CSS效果

## 實踐步驟

1. 先監聽 鍵盤 的 keydown Event
    - 利用 e.keyCode 取得符合的 audio標籤 和 div標籤
    - audio標籤 => 播放 聲音
    - div標籤 => 顯示 CSS效果

2. 監聽 CSS 的 transitionend Event (transitionend 事件會在 transition 结束后觸發)
    - `e.propertyName !== "transform"` 僅針對 transform 繼續做事，不是則停止
    - 若為 transform，則移除 CSS效果

## JS學習紀錄

### HTML5標籤 HTMLMediaElement

``` html
<audio data-key="65" src="sounds/clap.wav"></audio>
```
``` js
const audio = document.querySelector(`audio[data-key="${keyCode}"]`);

audio.currentTime = 0 ; //設定音效從 0 秒開始
audio.play();  // 播放音效
```

### DOM元素 Element.classList

``` html
<div data-key="65" class="key">
    <kbd>A</kbd>
    <span class="sound">clap</span>
</div>
```
``` js
const key = document.querySelector(`.key[data-key="${keyCode}"]`);

key.classList.add('playing');    //新增CSS屬性
key.classList.remove('playing'); //移除CSS屬性
key.classList.toggle('playing'); //切換CSS屬性
```

### DOM元素 NodeList

``` html
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
``` js
const keys = document.querySelectorAll('.key'); //NodeList.prototype.forEach() => IE不支援
```

### CSS監聽事件 transitionend

``` js
function removeTransition(e){
    // console.log(e);  //CSS 的 TransitionEvent
    // console.log(e.propertyName);  //
    if(e.propertyName !== "transform") return; // 僅針對 transform，不是則停止

    // 一般函式的this，作為 DOM 事件偵聽函式
    this.classList.remove('playing'); //移除CSS屬性
}


const keys = Array.from(document.querySelectorAll('.key'));
// transitionend 事件會在 CSS transition 结束后觸發
keys.forEach(key => key.addEventListener('transitionend',removeTransition))  

```
### 一般函式的this(作為 DOM 事件偵聽函式)