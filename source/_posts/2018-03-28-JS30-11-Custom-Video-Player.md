---
title: JS30-11-Custom-Video-Player
date: 2018-03-28 16:08:11
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img video.png %}

<font style="font-size:20px;">利用Video屬性及Method來變更播放器的功能，
播放/暫停、快進/快退、音量控制、倍數控制。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 實現播放器功能
    - 播放/暫停
    - 快進/快退
    - 音量控制
    - 倍數控制

## 實踐步驟

1. 取得所有<font color="red">video DOM元素</font>

2. 撰寫Function 並 監聽DOM元素
    - 監聽 video 播放或暫停 →延伸→ 切換播放的icon
    - 進度條更新目前播放時間
    - 進度條監聽是否有切換video播放時間
    - 調整 聲音、播放倍數
    - 進、快退(+25 or -10)

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/11%20-%20Custom%20Video%20Player/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/11%20-%20Custom%20Video%20Player/index.html)


***
## JS學習紀錄

### video 播放或暫停

``` js JS
function togglePlay(e){
    const method = video.paused ? 'play' : 'pause';
    video[method](); // 觸發影片API，當有更新時，連動會觸發本身的Event 
}

// 切換icon
function updateButton() {
    const icon = this.paused ? '►' : '❚ ❚';
    toggleBtn.textContent = icon;
}

video.addEventListener('click',togglePlay);         //點擊 影片的任何位置
toggleBtn.addEventListener('click',togglePlay);     //點擊 icon

// 監聽影片本身的Event，達到切換播放的icon
video.addEventListener('play', updateButton);
video.addEventListener('pause', updateButton);
```

上例比較特別的地方是在 `togglePlay()` 裡的 `video[method]()` 這個寫法，

原本會寫成 `video.play()` 和 `video.pause()` 來呼叫影片method，

所以也可使用 `video[method]()` 呼叫method。

<div class="note info">[HTMLMediaElement.play()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play)
[HTMLMediaElement.paused()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/paused)</div>

### 進度條更新目前播放時間

因為影片會一直持續播放，所以我們也要持續更新 進度條UI 畫面。

``` js JS
function handleProgress(e){
    // video.currentTime 目前播放時間
    // video.duration 屬性返回當前音頻/視頻的長度，以秒計。
    const percent = (video.currentTime / video.duration) * 100;
    // console.log(video.currentTime, video.duration, percent);
    progressBar.style.flexBasis = `${percent}%`;
}

video.addEventListener('progress', handleProgress);
```

作者有提到關於 video 有兩個監聽參數 `timeupdate`、`progress`，
都可以做為<font color="red">影片時間變動</font>時的觸發條件，
二者差異如下：
- progress：會在<font color="red">載入</font>時，就開始觸發
- timeupdate：會在啟動播放後，才開始觸發

所以建議使用 `progress` 提早觸發。

<div class="note info">[progress](https://developer.mozilla.org/en-US/docs/Web/Events/progress)
[timeupdate](https://developer.mozilla.org/en-US/docs/Web/Events/timeupdate)</div>

### 進度條監聽是否有切換video播放時間

判斷User是否有在 <font color="red">進度條</font> 做 <font color="red">變更video時間</font> 的動作，當符合操作條件時，就變更目前影片的播放時間。

``` js JS
const progress = document.querySelector('.progress'); // 進度條DOM元素

function scrub(e) {
    // e.offsetX 取得滑鼠點擊的位置
    // progress.offsetWidth 進度條的總寬度
    // video.duration 屬性返回當前音頻/視頻的長度，以秒計。
    // console.log(e.offsetX, progress.offsetWidth, video.duration);
    
    const scrubTime = (e.offsetX / progress.offsetWidth) * video.duration;

    video.currentTime = scrubTime; // 更改影片的播放時間
}


let mousedown = false; //判斷滑鼠是否有點擊
progress.addEventListener('click', scrub); //監聽 進度條DOM元素

/* 此段程式碼，可簡寫為下面程式碼
progress.addEventListener('mousemove', (e) => {
    if (mousedown) {
        scrub(e);
    }
});
*/

// 監聽 滑鼠滑動 時，若 滑鼠為down 的狀態時，就呼叫 scrub 的method。
progress.addEventListener('mousemove', (e) => mousedown && scrub(e)); 


// 監聽滑鼠是否是 down 或 up 的狀態
progress.addEventListener('mousedown', () => mousedown = true);
progress.addEventListener('mouseup', () => mousedown = false);
```

<div class="note info">[HTMLMediaElement.currentTime](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/currentTime)</div>

### 變更 聲音、播放倍數

利用 `JS` 取得DOM元素的name、value的值，進而更新video的屬性。

``` html HTML
<!-- 聲音 -->
<input type="range" name="volume" class="player__slider" min="0" max="1" step="0.05" value="1">
<!-- 播放倍數 -->
<input type="range" name="playbackRate" class="player__slider" min="0.5" max="2" step="0.1" value="1">
```

``` js JS
function handleRangeUpdate() {
    video[this.name] = this.value;
}

ranges.forEach(range => range.addEventListener('change', handleRangeUpdate));
ranges.forEach(range => range.addEventListener('mousemove', handleRangeUpdate));
```

<div class="note info">[HTMLMediaElement.volume](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/volume)
[HTMLMediaElement.playbackRate](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/playbackRate)</div>

### 快進、快退(+25 or -10)

利用 `JS` 取得DOM元素<font color="red">dataset</font>的值，進而更新video的屬性。

``` html HTML
<!-- 快退 -->
<button data-skip="-10" class="player__button">« 10s</button>
<!-- 快進 -->
<button data-skip="25" class="player__button">25s »</button>
```

``` js JS
function skip() {
    // dataset值是字串，利用parseFloat需轉換成數字型態
    video.currentTime += parseFloat(this.dataset.skip); 
}

skipButtons.forEach(button => button.addEventListener('click', skip));
```
