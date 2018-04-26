---
title: JS30-29-Countdown-Timer
date: 2018-04-26 00:58:52
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js29.jpg %}

<font style="font-size:20px;">完成可自定義時間的倒數計時器</font>

{% endcq %}

<!-- more -->
***

## 目標

- 完成可自定義時間的倒數計時器。

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/28%20-%20Video%20Speed%20Controller/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/28%20-%20Video%20Speed%20Controller/index.html)

***
## 學習紀錄


將此專案拆分為多個部份，然後再依序慢慢完成一部份的小功能，
最終就會完成一個成型的專案，下列為拆分出來的小功能

- 完成計時函式
- UI顯示倒數時間
- UI顯示結束時間
- 監聽倒數時間的按鈕
- 客制自訂時間倒數

至於上述的功能，沒有一定的先後順序，都是可以獨立完成的，
只是寫Code前，可以多想一下，哪些功能先完成的話，
會較利於下一個小功能開發時，可以更方便測試或節省開發時間...等因素

### 一、完成計時函式

``` js 計時函式
let countdown;

function timer(seconds) {
  // 取得 現在時間
  const now = Date.now();
  // 取得 現在時間 + 計時秒數
  const then = now + seconds * 1000;

  // 顯示倒數時間
  console.log(seconds);

  // 每秒執行一次，刷新資料
  // 將 setInterval 設定在 countdown變數，
  // 以便於後面可用程式指定結束 setInterval。
  countdown = setInterval(() => {
    // 計算 倒數的總秒數
    const secondsLeft = Math.round((then - Date.now()) / 1000);

    // 倒數完畢後，結束 setInterval
    if(secondsLeft < 0) {
      clearInterval(countdown);
      return;
    }

    // 顯示倒數時間
    console.log(secondsLeft);
  }, 1000);
}
```

### 二、UI顯示倒數時間

``` js UI顯示倒數時間
const timerDisplay = document.querySelector('.display__time-left');

// 顯示倒數時間
function displayTimeLeft(seconds) {
  // 取得 分
  const minutes = Math.floor(seconds / 60);
  // 取得 秒
  const remainderSeconds = seconds % 60;

  // 在顯示秒數部份，若秒數小於10，在個位數前面補零
  const display = `${minutes}:${remainderSeconds < 10 ? '0' : '' }${remainderSeconds}`;

  // 顯示至畫面上
  document.title = display;
  timerDisplay.textContent = display;
}
```

因為完成了 **UI顯示倒數時間** 功能，所以就可以更新剛剛完成的 **計時函式**，
更新如下

``` diff 更新 計時函式
function timer(seconds) {
  // ...略

  // 顯示倒數時間
- console.log(seconds);
+ displayTimeLeft(seconds);

  countdown = setInterval(() => {
    // ...略

    // 顯示倒數時間
-   console.log(secondsLeft);
+   displayTimeLeft(secondsLeft);
  }, 1000);
}
```

### 三、UI顯示結束時間

``` js UI顯示結束時間
const endTime = document.querySelector('.display__end-time');

// 顯示結束時間
function displayEndTime(timestamp) {
  // 將 總秒數 轉換為 時間格式
  const end = new Date(timestamp);

  // 取得 小時
  const hour = end.getHours();
  // 將 小時 轉換 12小時制
  const adjustedHour = hour > 12 ? hour - 12 : hour;
  // 取得 分鐘
  const minutes = end.getMinutes();

  // 在顯示 分鐘 部份，若小於10，在個位數前面補零
  endTime.textContent = `Be Back At ${adjustedHour}:${minutes < 10 ? '0' : ''}${minutes}`;
}
```

跟上面步驟一樣，完成此函式後，就可以再次更新 **計時函式**，
更新如下

``` diff 更新 計時函式
function timer(seconds) {
  // ...略

  // 顯示倒數時間
  displayTimeLeft(seconds);
+ // 顯示結束時間
+ displayEndTime(then);

  // ...略
}
```

### 四、監聽倒數時間的按鈕

``` js 監聽倒數時間的按鈕
const buttons = document.querySelectorAll('[data-time]');

// 啟動 倒數時間
function startTimer() {
  // 取得 data-time 的數值
  const seconds = parseInt(this.dataset.time);
  // 傳入 計時函式
  timer(seconds);
}

// 遍歷按鈕 並加上 監聽事件
buttons.forEach(button => button.addEventListener('click', startTimer));
```

到這裡看似沒什麼問題，不過當<font color="red">連續點擊</font>倒數按鈕的話，會連續呼叫計時函式，
本來連續呼叫函式，應該是沒什麼問題的，但就是因為有使用到 `setInterval`，
所以如果我連續點了 <font color="red">5下</font> 按鈕，這時就會產生**5組的setInterval**<font color="red">同時</font>在循環執行，
導致畫面就會出現怪怪的問題，而修正的方法就是

每當 <font color="red">啟動倒數時間</font> 時，要先清除上一次的`setInterval`。


``` diff 修正bug 計時函式
function timer(seconds) {
+ // 因要重新倒數，清除之前設定
+ clearInterval(countdown);

  // 取得 現在時間
  const now = Date.now();

  // ...略
}
```

### 五、客制自訂時間倒數

``` js
// 客制自訂時間倒數
document.customForm.addEventListener('submit', function(e) {
  // 取消 submit 後，頁面跳轉
  e.preventDefault();
  // 取得 時間
  const mins = this.minutes.value;
  // 傳入 計時函式
  timer(mins * 60);
  // 清空欄位
  this.reset();
});
```