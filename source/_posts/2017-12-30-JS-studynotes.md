---
title: JS-基礎用法
date: 2017-12-30 22:18:55
categories: 
- JS
tags:
- JS
---

{% cq %}

{% asset_img js_01.png %}

{% endcq %}

<!-- more -->
***

## Selector - 選擇元素

- element = document.querySelector(selectors);
    - element 是`元素`物件。
    - selectors 是以逗號分隔，包含一個或多個 `CSS 選擇器`的字串。


``` js 選擇單一元素 querySelector
//回傳第一個符合條件的元素
var el = document.querySelector('#titleId');
```

``` js 選擇多個元素 querySelectorAll
//回傳符合條件的元素
var el = document.querySelectorAll('.titleClass');
```

***
## Attribute - 增加標籤屬性

- <font color="red">set</font>Attribute  設定

``` js 設定 標籤屬性
var el = document.querySelector('.titleClass a');
el.setAttribute('href','http://www.yahoo.com.tw');
```

- <font color="red">get</font>Attribute  取得

``` js 取得 標籤屬性
var el3 = document.querySelector('.titleClass a').getAttribute('href');
console.log(el3);
```

***
## innerHTML - 插入HTML

將元素內的html<font color="red">重新覆蓋寫入</font>新的html。

``` js 插入HTML
var el = document.getElementById('main');
var str = '<h1 class="blue">1234</h1>'
el.innerHTML = str;
```

***
## createElement - 插入dom元素

建立一個新的DOM元素，然後再使用 `appendChild` 新增子節點，並不會覆蓋原有的DOM元素。

``` html 新增dom元素
<h1 class="title">
    <em>titile</em>
</h1>

<script >
    // 建立元素
    var sonElement = document.createElement("a");
    sonElement.setAttribute('href','www.facebook.com');
    sonElement.textContent = '前往Facebook';

    // 增加子節點
    var fatherElement = document.querySelector('.title');
    fatherElement.appendChild(sonElement);
</script>
```

``` diff 更新後結果
<h1 class="title">
    <em>titile</em>
+   <a href="www.facebook.com">前往Facebook</a>
</h1>
```

<div class="note primary">使用 appendChild 要注意的小細節：
要留意的是 如果 `appendChild` 使用時，append 上去的是一個<font color="red">已存在</font>的 node 時，它會做的是<font color="red">搬移</font>，而<font color="red">非複製</font>，
所以 appendChild 使用時要複製而非搬移，記得先使用 `Node.cloneNode()` 這個方法複製 Node Element。

參考：[PJ - Node Element 在 appendChild 後消失（disappear）!?](https://pjchender.blogspot.tw/2017/06/js-node-element-appenchild-disappear.html)
</div>

***
## addEventListener - 事件氣泡、事件捕捉

<span id="inline-blue">基本語法</span>

element.addEventListener(event, function, useCapture)

- 第三個參數：可省略，預設為 `false`。

<span id="inline-purple">範例</span>

可試試將第三個參數分別改成 `true` 、 `false`，各執行一次，會有什麼不一樣的結果。

``` html html
<div class="warp">
  <div class="box"></div>
</div>
```
``` js 預設：事件氣泡-從指定元素往外找
var el = document.querySelector('.box');
el.addEventListener('click',function(){
  alert('box');
  console.log('box');
},false);

var elBody = document.querySelector('.body');
elBody.addEventListener('click',function(){
  alert('body');
  console.log('body');
},false);
// false (事件氣泡 - event Bubbling) - 從指定元素往外找
// true (事件捕捉 - event capturing) - 從最外面找到指定元素
```

<p data-height="212" data-theme-id="0" data-slug-hash="MrmOzO" data-default-tab="result" data-user="Kanboo" data-embed-version="2" data-pen-title="addEventListener - 事件氣泡、事件捕捉" class="codepen">See the Pen <a href="https://codepen.io/Kanboo/pen/MrmOzO/">addEventListener - 事件氣泡、事件捕捉</a> by Kanboo (<a href="https://codepen.io/Kanboo">@Kanboo</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


## stopPropagation - 中止冒泡行為

依上例 <font color="red">addEventListener-事件氣泡</font> 因素，有時只是想單純針對單一元素監聽，不想因為事件冒泡的行為，而去觸發到其他元素，這時就可利用 <font color="red">stopPropagation</font> 來達成此需求。

``` diff
var el = document.querySelector('.box');
el.addEventListener('click',function(e){
+ e.stopPropagation(); // 中止冒泡行為
  alert('box');
  console.log('box');
},false);
```

***
## 事件監聽優化 & e.target

有時子元素可能要<font color="red">上千個</font>，每個都要綁上監聽的話，效能不是很優，這時可從父節點下手，使用 <font color="red">e.target.nodeName</font> 判斷是否為 想監聽的子元素，若是為想監聽的子元素，可再用 e.target.value 或是 e.target.textContent 取得 值。

- e.target.nodeName 取得點擊元素的<font color="red">標籤名稱</font>，如：UL、LI、INPUT…
- e.target.value 取得選取元素的值

<span id="inline-purple">範例</span>

當有一個 ul 底下有多個 li 都要監聽的話，這時我們可以利用 <font color="red">addEventListener-事件氣泡</font> 的原理，只要針對 <font color="red">ul</font> 監聽，讓他往上冒泡，當到達 li 時，這時我們就可以針對 li 做事了。

``` js 原始寫法
//取得ul底下的所有li元素
var list = document.querySelectorAll('.list li');
//forloop，將每個li元素綁上監聽事件(N次)
var len = list.length;
for(var i = 0;len>i;i++){
    list[i].addEventListener('click',checkName,false)
}

function checkName(e){
    console.log(e.target.textContent);
}
```

``` js 優化寫法
//取得ul元素
var list = document.querySelector('.list');
//將ul元素綁上監事件(一次)
list.addEventListener('click',checkName,false)

function checkName(e){
    if(e.target.nodeName !== 'LI'){return}; // 判斷是否為li元素
    console.log(e.target.textContent);
}
```

***
## preventDefault - 取消預設觸發行為

比較常用在 <font color="red">a連結的href</font>、<font color="red">Form表單的submit</font> 上，有時可能只是想觸發呼叫Function，而不想使用到原生附與的功能的話，就可利用 <font color="red">preventDefault</font> 達成此需求。

``` js 取消預設觸發行為
var list = document.querySelector('a');
list.addEventListener('click',function(e){
    e.preventDefault(); //取消預設觸發行為

    /* 撰寫你的Code */
})
```

***
## localStorage - 灠瀏器資料儲存

<span id="inline-blue">基本語法</span>

``` js 儲存
localStorage.setItem('countryItem',countryString);
```

``` js 讀取
localStorage.getItem('countryItem');
```

<span id="inline-purple">範例</span>

localstorage 只能保存 `string` 資料，所以當資料非字串型態的話，記得轉為`字串string`。

<div class="note default">JSON.<font color="red">stringify</font>() 將 array 轉為 string
JSON.<font color="red">parse</font>() 將 string 轉為 array</div>

``` js
var country = [
    {farmer:'王農夫'}
];

//儲存
var countryString= JSON.stringify(country); // 轉字串
console.log(countryString);
localStorage.setItem('countryItem',countryString);

//讀取
var getData = localStorage.getItem('countryItem');
var getDataAry = JSON.parse(getData); // 轉array

console.log(getDataAry[0].farmer);
```

<div class="note primary">補充小知識
var data = listData; 
var data2 = listData || []; //(建議寫法)

console.log('沒有 []', data); // 有可能取得資料或 undefined 
console.log('加上 []', data2); // 有可能</div>

***
## data-* - 透過 dataset 讀取自訂資料

- 名字絕對不能以 <font color="red">xml</font> 起頭，無論是否用於 xml、
- 名字絕對不能包含<font color="red">分號（U+003A）、</font>
- 名字絕對不能包含<font color="red">大寫 A 到大小 Z </font>的拉丁字母。

可透過 HTMLElement.<font color="red">dataset</font>.testValue 或 HTMLElement.<font color="red">dataset</font>["testValue"] 訪問

``` html 
<div id="user" data-id="1234567890" data-user="johndoe" data-date-of-birth>John Doe</div>

let el = document.querySelector('#user');

// el.id == 'user'
// el.dataset.id === '1234567890'
// el.dataset.user === 'johndoe'
// el.dataset.dateOfBirth === ''

el.dataset.dateOfBirth = '1960-10-03'; // set the DOB.

// 'someDataAttr' in el.dataset === false
el.dataset.someDataAttr = 'mydata';
// 'someDataAttr' in el.dataset === true
```

***
## AjAX 

<span id="inline-blue">屬性</span>

<font color="red">readyState</font>:
　０：尚未讀取
　１：讀取中
　２：已下載完畢
　３：資訊交換中
　４：處理完畢

<font color="red">Status</font>:即[HTTP協定的狀態碼](https://blog.miniasp.com/post/2009/01/16/Web-developer-should-know-about-HTTP-Status-Code.aspx)

當 readyState == <font color="red">4</font>，代表有執行完成，但不一定是有正確撈到資料，
要配合HTTP status == <font color="red">200</font>，才代表是正確撈到資料。

<span id="inline-purple">範例</span>

利用AJAX傳送(<font color="red">POST</font>)帳號、密碼資料至後端註冊會員帳號。

``` js POST資料
var send = document.querySelector('.send');
send.addEventListener('click',signup,false);

function signup(){
    var emailStr = document.querySelector('.account').value;
    var passwordStr = document.querySelector('.password').value;

    //資料丟到物件
    var account = {};
    account.email = emailStr;
    account.password = passwordStr;
    
    var xhr = new XMLHttpRequest();
    xhr.open('post','https://hexschool-tutorial.herokuapp.com/api/signup',true);
    xhr.setRequestHeader('Content-type','application/json'); //宣告json格式
    var data = JSON.stringify(account); //轉成字串
    xhr.send(data); // 送出

    //ajax完成後，執行此event
    xhr.onload = function(){
        if (xhr.readyState == 4 && xhr.status == 200) {
            // 萬事具備
            var callbackData = JSON.parse(xhr.responseText); //接收回傳後資料
            console.log(callbackData);
            var veriStr =  callbackData.message;
            if(veriStr =="帳號註冊成功"){
                alert('帳號註冊成功！！');
            }else{
                alert("帳號註冊失敗！");
            }
        } else {
            // 似乎有點問題。
            // 或許伺服器傳回了 404（查無此頁）
            // 或者 500（內部錯誤）什麼的。
            alert ("伺服器處理錯誤");
        }

    }
}
```

``` js ajax回傳的格式
{
  "success": true,
  "result": {'結果'},
  "message": "登入成功"
}
```

<div class="note info">[Asynchronous JavaScript And XML](https://developer.mozilla.org/zh-TW/docs/Web/Guide/AJAX/Getting_Started)</div>
