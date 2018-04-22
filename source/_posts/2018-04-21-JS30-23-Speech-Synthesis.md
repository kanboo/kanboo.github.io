---
title: JS30-23-Speech-Synthesis
date: 2018-04-21 21:52:22
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js23.jpg %}

<font style="font-size:20px;"> 將文字轉語音(可變更速率及音調)。</font>

{% endcq %}

<!-- more -->
***

## 目標

-  將文字轉語音(可變更速率及音調)。

## 實踐步驟

1. 取得 相關DOM元素 並設定 SpeechSynthesisUtterance(語音設定)
2. 監聽 SpeechSynthesis(語音播放) 的 voiceschanged 事件，取得 語系 相關資料
3. 設定 發音語系
4. 監聽 語系是否變更，更新語音設定，並播放語音
5. 監聽 速率、音調、文字區塊 是否變更，更新語音設定，並播放語音
6. 監聽 buttom 的 click事件，播放或取消語音

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/23%20-%20Speech%20Synthesis/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/23%20-%20Speech%20Synthesis/index.html)


***
## JS學習紀錄


### 一、取得 相關DOM元素 並設定 SpeechSynthesisUtterance(語音設定)

``` js
// 設定語音服務應讀取的文字內容及播放的細節(語言、音調、聲音、速率...等屬性)
const msg = new SpeechSynthesisUtterance();
let voices = [];
const voicesDropdown = document.querySelector('[name="voice"]');
const options = document.querySelectorAll('[type="range"], [name="text"]');
const speakButton = document.querySelector('#speak');
const stopButton = document.querySelector('#stop');

// 設定發音的文字內容
msg.text = document.querySelector('[name="text"]').value;

// 將一段文字加入發音庫，播放此發音。
speechSynthesis.speak(msg); // 測試用
```

### 二、監聽 SpeechSynthesis(語音播放) 的 voiceschanged 事件，取得 語系 相關資料

``` js
// 取得 語系 相關資料
function populateVoices() {
  /* 取得 speechSynthesis 中所有的 SpeechSynthesisVoice 物件，
  而物件的屬性為 所有發音的資訊 */
  voices = this.getVoices();

  // 取出所有語系，組成HTML後，更新下拉清單的值
  voicesDropdown.innerHTML = voices
    .map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`)
    .join('');
}

// 監聽 voiceschanged 事件(當 SpeechSynthesisVoice 清單改變時，就會觸發此事件)
speechSynthesis.addEventListener('voiceschanged', populateVoices);
```

### 三、設定 發音語系

``` js
// 設定 發音語系
function setVoice() {
  // 根據select挑選的值，從所有的 SpeechSynthesisVoice 物件中，取出符合條件的物件
  msg.voice = voices.find(voice => voice.name === this.value);
}
```

### 四、監聽 語系是否變更，更新語音設定，並播放語音

``` diff
// 設定 發音語系
function setVoice() {
  // 根據select挑選的值，從所有的 SpeechSynthesisVoice 物件中，取出符合條件的物件
  msg.voice = voices.find(voice => voice.name === this.value);
+  // 播放語音
+  toggle();
}

+// 觸發 播放語音
+function toggle(startOver = true) {
+  // 移除所有的發音資訊
+  speechSynthesis.cancel();

+ // 將一段文字加入發音庫，播放語音
+  if (startOver) {
+    speechSynthesis.speak(msg);
+  }
+}

+// 監聽 語系 是否變更
+voicesDropdown.addEventListener('change', setVoice);
```

### 五、監聽 速率、音調、文字區塊 是否變更，更新語音設定，並播放語音

透過 DOM元素的命名方式，將 name命名 與 SpeechSynthesisUtterance物件屬性 取一樣名稱，
在二邊的配合下，這樣就可以更精簡程式碼。

``` js
// 設定速率跟音準
function setOption() {
  /*
  透過 DOM元素的命名方式，
  將 name命名 與 SpeechSynthesisUtterance物件屬性 取一樣名稱，
  在二邊的配合下，這樣就可以更精簡程式碼。
  */

  // 更新 語音設定
  msg[this.name] = this.value;

  // 播放語音
  toggle();
}

// 監聽 速率、音調、文字區塊 是否變更
options.forEach(option => option.addEventListener('change', setOption));
```

### 六、監聽 buttom 的 click事件，播放或取消語音

``` diff
// 觸發 播放語音
function toggle(startOver = true) {
  // 移除所有的發音資訊
  speechSynthesis.cancel();

  // 將一段文字加入發音庫，播放語音
  if (startOver) {
    speechSynthesis.speak(msg);
  }
}

+// 播放按鈕
+speakButton.addEventListener('click', toggle);
+// 停止按鈕
+stopButton.addEventListener('click', () => toggle(false));
```
<div class="note info">[SpeechSynthesisUtterance(語音設定)](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance)
[SpeechSynthesis(語音播放)](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis)</div>

***
<span id="inline-purple">觀念補充</span>

在使用 `addEventListener` 時，若遇到需<font color="red">傳入參數</font>的話，不可直接用下列此方式，會導致功能不正常。

``` js 功能不正常
// 不能直接寫 function並加上參數
stopButton.addEventListener('click', toggle(false));
```

可透過下列二種方式，傳入參數值。

``` js 解決方式
// 寫法1
stopButton.addEventListener("click", function(){
  toggle(false);
})

// 寫法2:arrow function
stopButton.addEventListener("click", () => toggle(false))
```
