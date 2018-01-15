---
title: JS-事件機制的原理
date: 2018-01-15 23:39:08
categories: 
- JS
- 重新認識 JavaScript
tags:
- JS
- 重新認識 JavaScript
- 事件冒泡 (Event Bubbling)
- 事件捕獲 (Event Capturing)
---

{% cq %}

{% asset_img js_01.png %}

<font style="font-size:20px;">事件冒泡 (Event Bubbling) & 事件捕獲 (Event Capturing)</font>

{% endcq %}

<!-- more -->
***

## 前言

在 2018 iT 邦幫忙鐵人賽，看到Kuro大的重新認識 JavaScript 系列文，仔細的閱讀後，紀錄自己觀念不足的部份，也非常推薦給大家觀看此系列文。

<div class="note info">[Kuro-重新認識 JavaScript 系列](https://ithelp.ithome.com.tw/users/20065504/ironman/1259?page=1)</div>

***
## 事件冒泡 (Event Bubbling)

{% asset_img js_02.png %}
>[圖片來源： Event Flow: capture, target, and bubbling](http://www.java2s.com/Book/JavaScript/DOM/Event_Flow_capture_target_and_bubbling.htm)

事件冒泡指的是「從啟動事件的元素節點開始，逐層往上傳遞」，直到整個網頁的根節點，也就是 document。

***
## 事件捕獲 (Event Capturing)

{% asset_img js_03.png %}
>[圖片來源： Event Flow: capture, target, and bubbling](http://www.java2s.com/Book/JavaScript/DOM/Event_Flow_capture_target_and_bubbling.htm)

剛剛說過「事件冒泡」機制是由下往上來傳遞，那麼「事件捕獲」(Event Capturing) 機制則正好相反。

***
## 事件傳遞順序

既然事件傳遞順序有兩種機制，那**我怎麼知道事件是依賴哪種機制**執行的？

答案是：兩種都會執行。

{% asset_img js_04.png %}
>[圖片來源: W3C, DOM event flow](https://www.w3.org/TR/2003/NOTE-DOM-Level-3-Events-20031107/events.html#Events-phases)

<div class="note primary">開啟console，分別點擊父元素、子元素
[事件機制的原理](https://codepen.io/Kanboo/pen/goKXby)</div>

***
## 事件監聽 EventTarget.addEventListener()

addEventListener() 基本上有三個參數，分別依序是
- 事件名稱
- 事件的處理器(事件觸發時執行的 function)
- Boolean，由這個 Boolean 決定事件是以「捕獲」或「冒泡」機制執行，若不指定則預設為「<font color="red">冒泡</font>」。

``` html
<button id="btn">Click</button>
```

``` js 綁定監聽動作
var btn = document.getElementById('btn');

btn.addEventListener('click', function(){
  console.log('HI');
}, false);
```

使用這種方式來註冊事件的好處是可以重複指定多個「處理器」(handler) 給同一個元素的同一個事件：

``` js
var btn = document.getElementById('btn');

btn.addEventListener('click', function(){
  console.log('HI');
}, false);

btn.addEventListener('click', function(){
  console.log('HELLO');
}, false);
```

點擊後 console 出現：

``` js
"HI"
"HELLO"
```

若是要解除事件的註冊，則是透過 <font color="red">removeEventListener()</font> 來取消。

<font color="red">removeEventListener()</font> 的三個參數與 <font color="red">addEventListener()</font> 一樣，分別是「事件名稱」、「事件的處理器」以及「捕獲」或「冒泡」的機制。

但是需要注意的是，由於 addEventListener() 可以同時針對某個事件綁定多個 handler，所以透過 removeEventListener() 解除事件的時候，<font color="red">第二個參數的 handler</font> 必須要與先前在 addEventListener() 綁定的 handler 是同一個「<font color="red">實體(記憶體位址)</font> 」。

``` js 二個function各自存在不同的記憶體位址
var btn = document.getElementById('btn');

btn.addEventListener('click', function(){
  console.log('HI');
}, false);

// 移除事件，但是沒用
btn.removeEventListener('click', function(){
  console.log('HI');
}, false);
```

像上面這樣，即使執行了 removeEventListener 來移除事件，但 click 時仍會出現 'HI'。 因為 addEventListener 與 removeEventListener 所移除的 handler 實際上是兩個不同<font color="red">實體(記憶體位址)</font> 的 function 物件。

``` js 同一個function，同個記憶體位址
var btn = document.getElementById('btn');

// 把 event handler 拉出來
var clickHandler = function(){
  console.log('HI');
};

btn.addEventListener('click', clickHandler, false);

// 移除 clickHandler， ok!
btn.removeEventListener('click', clickHandler, false);
```

