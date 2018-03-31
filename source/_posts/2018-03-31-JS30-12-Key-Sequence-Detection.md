---
title: JS30-12-Key-Sequence-Detection
date: 2018-03-31 10:33:29
categories: 
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img UNICORNS.png %}

<font style="font-size:20px;">輸入<font color="red">隱藏指令</font>後，觸發特效的效果。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 網頁輸入「<font color="red">隱藏指令</font>」後，觸發特效的效果


## 實踐步驟

1. 監聽 `window` 的 `keyup` 事件
    - window.addEventListener('keydown', function)

2. 判斷輸入的<font color="red">隱藏指令</font>，是否正確。
    - 利用 `e.key` 取得鍵盤的值
    - 籍由 `splice()`、`join()`、`includes()` 搭配使用進行判斷。

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/12%20-%20Key%20Sequence%20Detection/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/12%20-%20Key%20Sequence%20Detection/index.html)
><font color="red">隱藏指令：kanboo</font>


***
## JS學習紀錄

### 思考：如何刪除多餘的字元

<span id="inline-toc">Q</span>

主要思考的點是每次鍵盤輸入後，只留下隱藏指令的<font color="red">長度</font>，而將多餘的字元刪除呢？

<span id="inline-toc">A</span>

因為每次 `push` 都加進Array最後面的位置，所以就代表多餘的字元是在前面的位置，

所以我的思維是「從前面扣掉 多餘的字元(鍵盤輸入總長度-隱藏指令長度)」。

``` js 從前面計算
// 只保留與superKey一樣長度的字元，多餘的去除掉，避免太過冗長不好看。
keyArr.splice(0, keyArr.length - superKey.length);
```

下例為影片作者的寫法，附上參考。

``` js 作者寫法：從後往前計算
keyArr.splice(-superKey.length - 1, keyArr.length - superKey.length); 
```


<span id="inline-purple">JS程式碼</span>

程式碼不多，就直接貼上來，方便看。

``` js 整段程式碼
const superKey = 'kanboo'; // 隱藏指令
const keyArr = []; // 暫存鍵盤輸入

function isSuperKey(e){
  keyArr.push(e.key);

  // 只保留與superKey一樣長度的字元，多餘的去除掉，避免太過冗長不好看。
  keyArr.splice(0, keyArr.length - superKey.length);

  // 作者寫法：從後往前計算
  // keyArr.splice(-superKey.length - 1, keyArr.length - superKey.length); 

  if ( keyArr.join('').includes(superKey)){
    cornify_add();
  }
}

window.addEventListener('keydown', isSuperKey);
```

***
### 補充：字串比對

看到**Guahsu大**的文章，有整理關於<font color="red">字串比對</font>的各種方式，覺得非常實用，<font color="red">筆記...</font>

方便以後可快速搜尋。

``` js 各種判斷的方式
var str = 'abcde';
var check1 = 'ab'; //包含ab，期待值是true
var check2 = 'ac'; //包含ac，期待值是false

//用includes()來取得true/false
str.includes(check1); //true
str.includes(check2); //false

//用match()來處理，判斷是否為 null 來取得true/false
str.match(check1); // object
str.match(check2); // null

//用indexOf()來處理，判斷是否為 -1 來取得true/false
str.indexOf(check1); // 0
str.indexOf(check2); // -1

//用search()，判斷是否為 -1 來取得true/false
str.search(check1); // 0
str.search(check2); // -1

//用RegExp正規表示式來取得true/false
var reg1 = /ab/;
var reg2 = /ac/;
reg1.test(str); // true
reg2.test(str); // false
```

<div class="note info">[Guahsu-探索](https://guahsu.io/2017/07/JavaScript30-12-Key-Sequence-Detection/)</div>
