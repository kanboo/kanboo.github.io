---
title: NodeJS-Express
date: 2017-12-27 23:23:49
categories: 
- NodeJS
tags:
- Express
---


{% cq %}

{% asset_img express_01.png %}

{% endcq %}

<!-- more -->
***

## 基本語法

下列語法為 `express` 的起手式，後面會慢慢新增功能。

``` js 網址：http://127.0.0.1:3000
var express = require('express'); //引用express模組
var app = express();


/* app.get參數說明 
req 是 Request 物件，存放這此請求的所有資訊
res 是 Response 物件，用來回應該請求
*/

// 首頁
app.get('/',function(req,res){
    // res.send('1234');
    res.send('<html><head></head><body><h1>hi!</h1></body></html>')
})

// 監聽 port
var port = process.env.PORT || 3000;
app.listen(port); 
```


***
## params - 取得指定路徑

透過 `:name` 這個變數，再配合 `req.params` 搭配使用，可取得路由的值。

PS：設定為<font color="red">變數</font>的重點為前面加個<font color="red">冒號</font>。


``` js 網址：http://127.0.0.1:3000/user
//http://127.0.0.1:3000/user/kanboo

app.get('/user/:name', function (req, res) {
    console.log(req.params);
    var myName = req.params.name;

    console.log(myName);
    if ( myName !== 'kanboo'){
        res.send('<html><body><h1>'+'查無此人'+'</h1></body></html>');
    }else{
        res.send('<html><body><h1>'+myName+'</h1></body></html>');

    }
})
```

下列範例為可設多個變數(:name、:data)，取得路由的值。

``` js 網址：http://127.0.0.1:3000/user/kanboo/abcd
//http://127.0.0.1:3000/user/kanboo/abcd
app.get('/user/:name/:data', function (req, res) {
    console.log(req.params);
    var myName = req.params.name;
    var myData = req.params.data;
    console.log(myName,myData);

    res.send('<html><body><h1>' + myName + '<br>' + myData + '</h1></body></html>');
})
```

***
## query - 取得網址參數

以 `http://127.0.0.1:3000/user/kanboo?limit=100&q=張惠妹` 這段網址為例，
我要用從<font color="red">?(問號)</font>切開，
前段網址：`http://127.0.0.1:3000/user/kanboo`
後段網址：`limit=100&q=張惠妹`
要取得`前段`網址的資料可用 <span id="inline-blue">params</span> ，
若要取`後段`網址的資料就要用 <span id="inline-green">query</span>。

下列示範利用 `req.query` 取得網址參數

``` js 
//網址：http://127.0.0.1:3000/user/kanboo?limit=100&q=張惠妹

app.get('/user/:name', function (req, res) {
    console.log(req.params);
    console.log(req.query);

    var myName = req.params.name;
    var limit = req.query.limit; // limit = 100
    var q = req.query.q;    //q = 張惠妹

    console.log(myName);
    console.log(limit);
    console.log(q);

    res.send('<html><body><h1>' +
        'myName:' + myName + '<br>' +
        '搜尋:' + q + '<br>' +
        '筆數：' + limit +
        '</h1></body></html>');

})
//http://127.0.0.1:3000/user/kanboo?limit=100&q=張惠妹
```

***
## Middleware - 中介層

簡單來說，Middleware的功能就有點像<font color="blue">卡控機制</font>，
例如：我要進去會員頁面時，Middleware可以幫我先<font color="blue">確認</font>是否已登入，再看要不要放行？

範例：

當我網址要切換至 http://127.0.0.1:3000/<font color="red">user</font> 頁面時，此時可利用 app.<font color="#f90">use</font> 先確認是否已登入，再決定是否使用  `next()` 放行，可讀取到 app.<font color="#f90">get</font>。


``` js 網址：http://127.0.0.1:3000/user
/* app.use參數說明 
req 是 Request 物件，存放這此請求的所有資訊
res 是 Response 物件，用來回應該請求
next 用來控制流程，是否可繼續執行
*/

app.use(function (req, res, next) {
    //這裡可以撰寫 驗證的Code
    var isVerification = true;

    if (isVerification === true) {
        console.log('驗證成功!!');
        next();
    }

})

/* 
注意：使用 app.use 的話，要放在 app.get 的前面，否則就沒有作用！
*/

app.get('/user', function (req, res) {

    res.send('<html><body><h1>' +
        'user' +
        '</h1></body></html>');

})
//http://127.0.0.1:3000/user
```
<div class="note warning">注意：使用 app.<font color="red">use</font> 的話，要放在 app.<font color="red">get</font> 的前面，否則就沒有作用！</div>

## Middleware - 404頁面

假設user在網站亂打網址`http://127.0.0.1:3000/alskdjlasdjf`，而根本無此頁面的話，這時會客制一個404頁面回應給user知道無此頁面。

範例：

利用 `res.status(404)`,判斷當無此頁面時，回應 `抱歉，你的頁面找不到` 的訊息給User知道。

``` js 網址：http://127.0.0.1:3000/alskdjlasdjf
// 404頁面  
app.use(function (req, res, next) {
    res.status(404).send('<html><body><h1>' +
        '抱歉，你的頁面找不到' +
        '</h1></body></html>');
})
//http://127.0.0.1:3000/alskdjlasdjf
```

***
## Middleware - 錯誤頁面

另一種情形是當user要進入某個頁面時，有可能是<font color="red">程式碼執行有誤</font>，這時我們可以先將畫面切換至錯誤頁面，告知User待修復，稍後嘗試。

範例：

程式碼執行 <font color="red">cc()</font> 這個Function出錯了，這時利用 `app.use` 的新參數 <font color="red">err</font> ,接收程式碼的錯誤訊息，並回應User系統有誤待修復。

``` js 
// 錯誤程式碼的狀況
app.use(function (req, res, next) {

    cc(); // 執行不存在的Function

    next(); // 放人通行..
})


// 錯誤頁面  
app.use(function (err, req, res, next) {
    console.log(err.stack);
    res.status(500).send('<html><body><h1>' +
    '程式有些問題，請稍後嘗試。' +
    '</h1></body></html>');
})
```

***
## Middleware - 中介function呼叫

假設我們有`很多頁面`要進入時，都需要先確認user是否已登入，才能進入頁面的話，
這時我們可以將 `確認登入程式碼(isLogin)` 包成一個 Function，然後在 `app.get` 插入 isLogin 進行確認。

下列重點為 isLogin 插入的位置
app.get('/', <font color="red">isLogin</font>, function (req, res){ 程式碼 })

``` js 
var isLogin = function (req, res, next) {
    //這裡可以撰寫 驗證的Code

    var _url = req.url;
    console.log(_url);

    console.log(_url !== '/');

    if (_url === '/') {
        console.log('登入成功!!');
        next();
    } else {
        console.log('登入失敗!!');
        res.send('<html><body><h1>' +
            '登入失敗' +
            '</h1></body></html>');
    }
}

// 開啟頁面前，先驗證 isLogin 是否成功
app.get('/', isLogin, function (req, res) {

    res.send('<html><body><h1>' +
        'Hello' +
        '</h1></body></html>');

})

// 開啟頁面前，先驗證 isLogin 是否成功
app.get('/user', isLogin, function (req, res) {

    res.send('<html><body><h1>' +
        'Hello' +
        '</h1></body></html>');

})


//http://127.0.0.1:3000/
//http://127.0.0.1:3000/user
```

***
## static - 載入靜態檔案

若我們在頁面需要提供「圖片、txt檔…」等檔案，如下列範例需使用到 img圖檔，我們要先利用 `express.static` 指定我們的`目錄起點`在哪，這樣img圖檔的路徑才能使用`相對路徑`取得資源。

``` js 
var express = require('express');
var app = express();

//增加靜態檔案的路徑
app.use(express.static('public'));

app.get('/', function (req, res) {

    res.send('<html><body><h1>' +
    '<img src="/img/logo.png" >'+
        'Hello' +
        '</h1></body></html>');

})
//http://127.0.0.1:3000/

```

***
## EJS - 樣板語言

安裝語法
``` zsh
$ npm install ejs-locals --save
```

使用方式

1. 宣告 樣板語言
2. 使用 res.render 讀取哪個ejs檔案，如：res.render('index')

``` js
var express = require('express');
var app = express();

//增加靜態檔案的路徑
app.use(express.static('public'));

//宣告 樣板語言
var engine = require('ejs-locals');
app.engine('ejs',engine);
app.set('views','./views'); // 宣告樣板程式碼放在哪裡
app.set('view engine','ejs'); // 宣告樣板是用哪種語言？ejs、pug、handlebars


app.get('/', function (req, res) {
    res.render('index');
})
//http://127.0.0.1:3000/
```

## EJS - 參數導入

下列範例示範res.render如何 `傳送參數` ，以及 `ejs接收參數的寫法`

``` js 
app.get('/', function (req, res) {
    res.render('index', {
        'title': '我是傳入的值',
        'dog': '狗狗',
        'html': '<h1>我是HTML</h1>',
        'show': true,
        'course': ['html','js','css'],
    });
})
//http://127.0.0.1:3000/
```

``` ejs ejs寫法
<!-- 
= 是 渲染成字串
- 是 渲染成HTML
-->

<%= html %>
<%- html %>


<!-- if寫法 -->
<% if(show){  %>
<h1>條件成立，顯示222!!</h1>
<% } %>


<!-- forloop寫法 -->
<ul>
<% for(var i=0;course.length>i;i++){  %>
<li> <%- course[i] %> </li>
<% } %>
</ul>

```

***
## body-parser - 取得表單資料

安裝語法
``` zsh
$ npm install body-parser --save
```

使用方式

user在search搜尋頁面的searchText輸入搜尋文字後

``` html search搜尋頁面
<form action="/searchList" method="post" >
    <input type="text" name="searchText" value="">
    <input type="submit" value="送出">

    <!-- 只有id的話，body-parser無法解析 -->
    <!-- 
        <input type="text" id="idflied" value=""> 
    -->
</form>
```


``` js
var bodyParser = require('body-parser');

// 增加 body 解析
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
    extended: false
}))

//搜尋頁面
app.get('/search', function (req, res) {
    res.render('search');
})
//http://127.0.0.1:3000/search

//傳統表單
app.post('/searchList', function (req, res) {
    //所有body欄位資料
    console.log(req.body);
    //取得某欄位資料
    console.log(req.body.searchText);

    //轉址
    res.redirect('search');
})
```