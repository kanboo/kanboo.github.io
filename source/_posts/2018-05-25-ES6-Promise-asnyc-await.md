---
title: ES6-Promise-asnyc-await
date: 2018-05-25 21:04:08
categories:
- JS
- ES6
tags:
- JS
- ES6
- Promise
- asnyc-await
---

{% cq %}

{% asset_img Async.png %}
<font style="font-size:20px;">Promise 與 asnyc-await 的運用</font>

{% endcq %}

<!-- more -->
***

## Promise Chain

下列為連續執行任務時，全部成功執行完畢的情況

``` js Promise Chain
let task01 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => { resolve('OK'); }, 0)
  })
}
let task02 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => { resolve('OK'); }, 0)
  })
}
let task03 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => { resolve('OK'); }, 0)
  })
}

// 執行任務
task01()
  .then(result => {
    console.log(`task01=>${result}`);
    return task02();
  })
  .then(result => {
    console.log(`task02=>${result}`);
    return task03();
  })
  .then(result => {
    console.log(`task03=>${result}`);
    console.log('done！！');
  })
  .catch(err => console.log(err));

// 輸出訊息
// task01=>OK
// task02=>OK
// task03=>OK
// done！！
```
> [JSBin](http://jsbin.com/netepopema/1/edit?js,console)

***
## Promise Chain 如何截取錯誤

若執行任務過程中有 <font color="red">失敗</font> 的話，就會統一使用 `catch` 截取錯誤的訊息。

``` js 截取錯誤
let task01 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      resolve('01 OK');
    }, 0)
  })
}
let task02 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      reject('02 failed');  // 這裡出錯了...
    }, 0)
  })
}
let task03 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      resolve('01 OK');
    }, 0)
  })
}

// 執行任務
task01()
  .then(result => {
    console.log(`task01=>${result}`)
  })
  .then(task02)
  .then(result => {
    console.log(`task02=>${result}`)
  })
  .then(task03)
  .then(result => {
    console.log(`task03=>${result}`)
  })
  .catch(e => {
    // 統一截取錯誤
    console.log(`error=>${e}`);
  })

// 輸出訊息
// task01=>01 OK
// error=>02 failed
```
> [JSBin](http://jsbin.com/xazupow/edit?js,console)

***
## asnyc-await

下列為連續執行任務時，全部成功執行完畢的情況

``` js asnyc-await chain
async function fn1() {
  return 'ok'
}
async function fn2() {
  return 'ok'
}
async function fn3() {
  return 'ok'
}

// 執行任務
(async () => {
  let a = await fn1();
  console.log(`fn1：${a}`);
  console.log(`fn1做完...才往下`);

  let b = await fn2();
  console.log(`fn2：${b}`);
  console.log(`fn2做完...才往下`);

  let c = await fn3();
  console.log(`fn3：${c}`);
})();

// 輸出訊息
// fn1：ok
// fn1做完...才往下
// fn2：ok
// fn2做完...才往下
// fn3：ok
```
> [JSBin](http://jsbin.com/nilivaxali/1/edit?js,console)

***
## asnyc-await 如何截取錯誤

在Promise中，我們知道是通過 `catch` 的方式來捕獲異常，
而當我們使用 `async` 時，則通過 `try/catch` 來截取錯誤。

``` js 截取錯誤
async function fn1() {
  return 'fn1'
}
async function fn2() {
  throw 'error：fn2' //送出錯誤
}
async function fn3() {
  return 'fn3'
}

// 執行任務
(async () => {
  let a = await fn1();

  // 截取錯誤
  try {
    let b = await fn2();
  } catch(e) {
    console.log(e);
  }

  let c = await fn3();
})();

// 輸出訊息
// error：fn2
```
> [JSBin](http://jsbin.com/yiyazunusi/1/edit?js,console)

但是依上列情況來說，當有多個 `await` 時，包太多 `try/catch` 的話，就會顯示程式碼不好看也不好閱讀

``` js
async function fn1() {
  throw 'error：fn1'
}
async function fn2() {
  throw 'error：fn2'
}
async function fn3() {
  throw 'error：fn3'
}

// 執行任務
(async () => {
  try {
    let a = await fn1();
  } catch(e) {
    console.log(e);
  }

  try {
    let b = await fn2();
  } catch(e) {
    console.log(e);
  }

  try {
    let c = await fn3();
  } catch(e) {
    console.log(e);
  }
})();

// 輸出訊息
// error：fn1
// error：fn2
// error：fn3
```
> [JSBin](http://jsbin.com/zoroyuyuje/1/edit?js,console)

這時我們可以換個寫法，將 `try/catch` 移位至 `function` 裡，而不是包在 `await` 外層

``` js
async function fn1() {
  // 方式1：分別在 try 和 catch 裡，return不同的結果
  try {
    return 'ok';
  } catch(e) {
    return 'fail';
  }
}

async function fn2() {
  // 方式2：用一個變數紀錄，最後再return結果
  let result = false;

  try {
    result = true;
  } catch(e) {
    result = false;
  }

  return result; //回傳結果
}

async function fn3() {
  let result = false;

  try {
    throw 'error';
  } catch(e) {
    result = false;
  }

  return result;
}

// 執行任務
(async () => {

  let a = await fn1();
  let b = await fn2();
  let c = await fn3();

  console.log(`fn1：${a}`);
  console.log(`fn2：${b}`);
  console.log(`fn3：${c}`);
})();

// 輸出訊息
// fn1：ok
// fn2：true
// fn3：false
```
> [JSBin](http://jsbin.com/zarupuduzo/1/edit?js,console)

***
## 何時要用 Promise 還是 asnyc-await 呢？

先說結論(個人看法)：

- 任務<font color="red">有</font>前後關係的話，使用 `Promise Chain` 寫法
- 任務<font color="red">無</font>前後關係的話，使用 `asnyc-await` 寫法

下面用不同情況來說明

<span id="inline-blue">情境 1</span>

因 task01、task02、task03 <font color="red">有前後關係</font>的話，
執行的順序需要先完成 task01 → task02 → task03，
當執行任務過程中，有錯的話，就不必再繼續往下執行。

``` js Promise Chain 寫法
let task01 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      resolve('01 OK');
    }, 0)
  })
}
let task02 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      reject('02 failed'); // 這裡出錯了
    }, 0)
  })
}
let task03 = () => {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      resolve('01 OK');
    }, 0)
  })
}

// 執行任務
task01()
  .then(result => {
    console.log(`task01=>${result}`)
  })
  .then(task02)
  .then(result => {
    console.log(`task02=>${result}`)
  })
  .then(task03)
  .then(result => {
    console.log(`task03=>${result}`)
  })
  .catch(e => {
    console.log(`error=>${e}`);
  })

// 輸出訊息
// task01=>01 OK
// error=>02 failed
```
> [JSBin](http://jsbin.com/xazupow/edit?js,console)

若是我們改用 `asnyc-await` 寫的話，如下

``` js asnyc-await 寫法
async function fn1() {
  try {
    return true;
  } catch(e) {
    return true;
  }
}
async function fn2() {
  let result = false;
  try {
    result = false;
  } catch(e) {
    result = false;
  }
  return result;
}
async function fn3() {
  let result = false;
  try {
    throw 'error';
  } catch(e) {
    result = false;
  }
  return result;
}

// 執行任務
(async () => {

  let a = await fn1();

  // 判斷 fn1 是否成功
  if(a){
    console.log(`fn1：${a}`);
  }else{
    console.log(`fn1：out.....`);
    throw 'out...'
  }

  let b = await fn2();

  // 判斷 fn2 是否成功
  if(b){
    console.log(`fn2：${b}`);
  }else{
    console.log(`fn2：out.....`);
    throw 'out...'
  }

  let c = await fn3();

  // 判斷 fn3 是否成功
  if(c){
    console.log(`fn3：${c}`);
  }else{
    console.log(`fn3：out.....`);
    throw 'out...'
  }
})();

// 輸出訊息
// fn1：true
// fn2：out.....
```
> [JSBin](http://jsbin.com/cuseroc/edit?js,console)

雖然`asnyc-await`一樣可以完成同樣的事情，
不過程式碼與`Promise`的寫法來看，就稍微雜亂了一點，
主要是因為執行每個任務時，需要在任務之間，穿插 `if` 的判斷來確認上個任務是否完成，
才能繼續往下執行，不像`Promise`統一使用一個 `catch` 截取錯誤的訊息。


<span id="inline-blue">情境 2</span>

若任務<font color="red">沒有</font>相依關係的話，使用 `asnyc-await` 的寫法，這樣程式碼看起來就簡潔一點。

``` js asnyc-await
async function fn1() {
  try {
    return 'ok';
  } catch(e) {
    return 'fail';
  }
}
async function fn2() {
  let result = false;
  try {
    result = true;
  } catch(e) {
    result = false;
  }
  return result;
}
async function fn3() {
  let result = false;
  try {
    throw 'error';
  } catch(e) {
    result = false;
  }
  return result;
}

// 執行任務
(async () => {

  let a = await fn1();
  let b = await fn2();
  let c = await fn3();

  console.log(`fn1：${a}`);
  console.log(`fn2：${b}`);
  console.log(`fn3：${c}`);
})();

// 執行任務
// fn1：ok
// fn2：true
// fn3：false
```
> [JSBin](http://jsbin.com/badaqenomu/edit?js,console)

***
## 參考文章

<div class="note info">[Alex 宅幹嘛-這些年經歷過的同步非同步 with Tommy](https://youtu.be/TNTIKEWoD_Q)
[帮助你开始理解async/await](https://juejin.im/post/5ae57d8d6fb9a07aa6318a20)</div>