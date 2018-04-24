---
title: JS30-20-Speech-Detection
date: 2018-04-21 09:54:29
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js20.png %}

<font style="font-size:20px;">利用麥克風說話，將語音轉成文字。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 利用麥克風說話，將語音轉成文字。


## 實踐步驟

1. 建立語音辨識物件
2. 新增文字區塊
3. 監聽並寫入語音資料

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/20%20-%20Speech%20Detection/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/20%20-%20Speech%20Detection/index.html)


***
## JS學習紀錄

紀錄JS如何使用語音的物件。

### 一、建立語音辨識物件

``` js
/* setp 1. 建立語音辨識物件 */
// 將全域環境中的 SpeechRecognition 設定好，根據不同瀏覽器
window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
// 建立 語音識別功能
const recognition = new SpeechRecognition();
// 允許語音辨識回傳識別後的資料
recognition.interimResults = true;

// 開始識別
recognition.start();
```

### 二、新增文字區塊

``` js
/* setp 2. 新增文字區塊 */
let p = document.createElement('p');
const words = document.querySelector('.words');
words.appendChild(p);
```

### 三、監聽並寫入語音資料

``` js
/* setp 3. 監聽並寫入語音資料 */
// 監聽識別回傳
recognition.addEventListener('result', e => {
  // 回傳資料為 nodelist，故轉為 array 執行
  const transcript = Array.from(e.results)
    // 透過 map 取得 陣列的第1筆
    .map(result => result[0])
    // 再取出第1筆的 transcript
    .map(result => result.transcript)
    // 用 join 將連結符號消掉
    .join('')

  // 將回傳的文字，先過濾 髒字
  const poopScript = transcript.replace(/poop|poo|shit|dump/gi, '💩');
  // 過濾完後，將回傳內容塞到 p元素
  p.textContent = poopScript;

  // 若回傳內容已經結束，再重新建立一個新的 p元素 來放下一次內容
  if (e.results[0].isFinal) {
    p = document.createElement('p');
    words.appendChild(p);
  }
})

// 如果語音辨識結束，則重新打開語音辨識。
recognition.addEventListener('end', recognition.start);
```

<div class="note info">[SpeechRecognition.interimResults](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition/interimResults)</div>