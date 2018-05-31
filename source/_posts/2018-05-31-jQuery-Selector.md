---
title: jQuery-選取器
date: 2018-05-31 14:27:41
categories:
- JS
- jQuery
tags:
- JS
- jQuery
---

{% cq %}

{% asset_img jQuery_image.jpg %}

<font style="font-size:20px;">深入了解 jQuery <font color="red">選取器</font> 實作的原理</font>

{% endcq %}

<!-- more -->

---

## 前言

在 Youtube 直播看到 <font style="color:#f00;font-size:20px;">Alex</font> 大大，深入說明 jQuery 底層的程式碼，在此紀錄 <font color="red">選取器</font> 的脈絡。

<div class="note info">Youtube：[從 jQuery 入門到認識 JavaScript #2 DOM 選取器與遍歷的使用解析](https://youtu.be/bTw0CKumCsI)
Facebook：[Alex 宅幹嘛-粉絲團](https://www.facebook.com/AlexOtakuWhat/)</div>

---

## Node.nodeType

在 jQuery 的原始裡，很常使用 `Node.nodeType` 來判斷取得物件的種類，

所以我們要先了解 `Node.nodeType` 是什麼。

{% asset_img nodeType.png %}

常出現的號碼，請記

<font color="blue">1</font>：表示元素的 <font color="red">Element</font> 節點，如 `<body>`、`<a>`、`<p>`、`<script>`、`<style>`、`<html>`、`<h1>` 或 `<div>`。

<font color="blue">3</font>：表示 HTML 元素屬性的 <font color="red">Attr</font> 節點 或 實際文字字元的 <font color="red">Text</font> 節點，它包括了「<font color="red">換行與空格</font>」。

<font color="blue">9</font>：表示文件的 <font color="red">Document</font> 節點。

<div class="note info">[MDN - Node.nodeType](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)</div>

---

## Node.nextSibling

有時候我們會需要取得 目前元素`<div>` 的下一個兄弟元素`<div>`，這時我們會使用 `element.nextSibling` 來完成這個任務，偏偏有時回傳給我們的東西，竟然不是我們的好兄弟元素`<div>`，而是一個奇怪的東西，這奇怪的東西正好是上述所說的 `Node.nodeType 為 3`。

<span id="inline-blue">範例測試</span>

```html HTML
<div id="div-01">Here is div-01</div>
<div id="div-02">Here is div-02</div>
```

```js Javascript
var el = document.getElementById('div-01').nextSibling,
  i = 1;

console.log('Siblings of div-01:');

while (el) {
  console.log(i + '. ' + el.nodeName);
  el = el.nextSibling;
  i++;
}

// 輸出結果：

// Siblings of div-01:
// 1. #text
// 2. DIV
// 3. #text
// 4. SCRIPT
// 5. #text
// 6. SCRIPT
```

由上例可看到 `div-01` 取下一個元素時，回傳的是 `#text`，而不是 `DIV` 元素。

> [CodePen](https://codepen.io/Kanboo/pen/eKmaGj)

---

## 解決#text 問題

那我們如何解決這個問題呢？下列有幾種解法供參考。

<span id="inline-blue">解法 1</span>

使用 `nodeType` 判斷，當我們取得下一個的話，判斷 `el.nextSibling !== 1` 時，就代表不是元素的 `Element` 節點，所以需要再往下抓下一個，直到 `el.nextSibling === 1` 時，才是我們要的 `div` 元素。

<span id="inline-blue">解法 2</span>

使用 `element..nextElementSibling` 取得下一個元素，它幫會我們避開 `el.nextSibling !== 1` 的東西，若你看他的原始碼的話，他也是使用 `解法 1` 的寫法去執行，只是此函式事先幫我們判斷好而已。

<div class="note info">[MDN - NonDocumentTypeChildNode.nextElementSibling](https://developer.mozilla.org/en-US/docs/Web/API/NonDocumentTypeChildNode/nextElementSibling)</div>

---

## for loop 潮的寫法

以往我們看到使用 `for loop` 的寫法如下

```js forloop
for (let i = 0; i < 5; i++) {
  // your code
}
```

再開始介紹 `for loop` <font color="red">潮</font>的寫法之前，這裡先重新複習 `for loop` 的語法結構

<span id="inline-purple">英文版</span>：for ([initialization]; [condition]; [final-expression])
<span id="inline-purple">中文版</span>：for ([宣告]; [條件]; [改變])

我就用中文來解說

* 宣告

  * 就等於我們一般在使用的 `let a = 10` 、 `let b = 87`

* 條件

  * 就像我們使用 `if` 的判斷式一樣， `if(a === 9)`

看完說明後，接下來我們來看 jQuery 裡，使用比較<font color="red">潮</font>的寫法

```js siblings函式
var siblings = function(n, elem) {
  var matched = [];

  for (; n; n = n.nextSibling) {
    if (n.nodeType === 1 && n !== elem) {
      matched.push(n);
    }
  }

  return matched;
};
```

注意上例 `for loop` 的寫法，他第一個 `initialization` 竟然沒有寫，為什麼呢？

下面我們用簡單的範例來說明

```js 潮的寫法
var sibling = elem.parentNode.firstChild;
for (; sibling; sibling = sibling.nextSibling) {
  if (sibling.nodeType !== 1 || sibling === elem) continue;
  siblings.push(sibling);
}
```

其實你注意 `for loop` 的上面一行，

可以看到它只是<font color="red">提前</font>把 `for loop`裡第一個 `initialization` 拉出來外層撰寫，所以當開始執行 `for loop` 時，就不必再宣告一次，程式就可以直接執行了。

<div class="note info">[Youtube 說明時間點-2h32m50s](https://youtu.be/bTw0CKumCsI?t=2h32m50s)</div>

---

## jQuery 的 siblings

再來我們來看 jQuery 的 siblings 函式時，紀錄二個重點

<span id="inline-blue">重點 1</span>

<span id="inline-toc">Q.</span>`siblings` 如何實現取得目前元素之相鄰的兄弟元素？

<span id="inline-toc">A.</span>

最初的想法，我們可以使用 `Node.nextSibling` 和 `Node.previousSibling` ，分別 `for loop` 往前往後各跑一次抓取。

不過我們來看一下 `jQuery`，是如何實現呢？

```js siblings函式
var siblings = function(n, elem) {
  var matched = [];

  for (; n; n = n.nextSibling) {
    if (n.nodeType === 1 && n !== elem) {
      matched.push(n);
    }
  }

  return matched;
};
```

```js 呼叫siblings
siblings: function( elem ) {
  return siblings( ( elem.parentNode || {} ).firstChild, elem );
}
```

主要的重點在於 `呼叫siblings` 裡的 `( elem.parentNode || {} ).firstChild`，它的作法為

1.  由目前元素 先往上一層父元素後，再往下層抓排序第一個的子元素，這樣就回到兄弟元素的第一個了。
2.  接著開始從第一個元素，`for loop` 使用 `nextSibling` 取得下一個元素，並且過程中排除自己。

這樣的話，就可以取得所有相鄰的兄弟元素。

<span id="inline-blue">重點 2</span>

延續上個案例

<span id="inline-toc">Q.</span>`(elem.parentNode || {}).firstChild` 之裡面的 `{}` 是做什麼用的？

<span id="inline-toc">A.</span>

其實後面的 `{}` 是用來擋 `程式錯誤 噴error` 的，

如果當`elem`是沒東西時，這時就會給 `{}` 值，而當 JS 執行此段語法 `{}.firstchild` 時，

會回傳 `undefined` 值，至少 js 還看得懂 `undefined`，所以就不會出現 `error訊息`，造成網站掛了，但若是直接用 <font color="red">undefined</font>`.firstchild`，就會死給你看。

<span id="inline-blue">範例說明</span>

可以將下列的程式碼，貼到 Chrome 開發者工具試試。

```js 例1：沒給值
let a; // 沒給值，預設為 undefined
a.firstchild;

// console 輸出：
// Uncaught TypeError: Cannot read property 'firstchild' of undefined
```

```js 例2：有給值
let b = {};
b.firstchild;

// console 輸出：
// undefined
```

<span id="inline-yellow">情境說明</span>

以 `AJAX` 為例

`res.data.member[0].name`

有時跟後端串 API 後，回傳的資料要取得 User 的名字，

但有時避免後端<font color="red">開錯規格或遺漏掉</font>的話，避免 JS 執行時，直接噴錯誤的話，

我們就要自行新增一些 <font color="red">擋煞</font> 的機制，參考寫法如下

```js 擋煞機制
if (res.data) {
  let list = data.membe || [];
  let name = list[0].name || '';

  // i get name
}
```

<div class="note info">[說明 {} 的用途](https://youtu.be/bTw0CKumCsI?t=2h50m40s)</div>
