---
title: NodeJS-Express
date: 2017-12-27 23:23:49
categories: 
- NodeJS
tags:
- NodeJS
- Express
---


{% cq %}

{% asset_img express_01.png %}

{% endcq %}

<!-- more -->
***

## 基本語法

下列語法為 `express` 的起手式，後面會慢慢新增功能。

``` js 後端app.js
var express = require('express'); //引用express模組
var app = express();


/* app.get參數說明 
req 是 Request 物件，存放這此請求的所有資訊
res 是 Response 物件，用來回應該請求
*/

// 路由 route
app.get('/',function(req,res){
    // res.send('1234');
    res.send('<html><head></head><body><h1>hi!</h1></body></html>')
})

// 監聽 port
var port = process.env.PORT || 3000;
app.listen(port); 

//執行 node app.js
//網頁連結 http://127.0.0.1:3000/
```


***
## params - 取得指定路徑

透過 `:name` 這個變數，再配合 `req.params` 搭配使用，可取得路由的值。

PS：設定為<font color="red">變數</font>的重點為前面加個<font color="red">冒號</font>。


``` js 後端app.js
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

``` js 後端app.js
//http://127.0.0.1:3000/user/kanboo/abcd

app.get('/user/:name/:data', function (req, res) {
    console.log(req.params);
    var myName = req.params.name; // kanboo
    var myData = req.params.data; // abcd
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

下列示範利用 `req.query` 取得 `後段` 網址的參數

``` js 後端app.js
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

<span id="inline-purple">範例</span>

當我網址要切換至 http://127.0.0.1:3000/<font color="red">user</font> 頁面時，此時可利用 app.<font color="red">use</font> 先確認是否已登入，再決定是否使用  `next()` 放行，可讀取到 app.<font color="red">get</font>。


``` js 後端app.js
//網址：http://127.0.0.1:3000/user

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

***
## Middleware - 404頁面

假設user開到一個錯誤的網址 `http://127.0.0.1:3000/alskdjlasdjf`，而根本無此網頁的話，這時會客制一個404頁面回應給user知道無此網頁。

<span id="inline-purple">範例</span>

利用 `res.status(404)` 判斷當無此頁面時，回應 `抱歉，你的頁面找不到` 的訊息給User知道。

``` js 後端app.js
// 網址：http://127.0.0.1:3000/alskdjlasdjf

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

另一種情形是當user要進入某個頁面時，有可能是<font color="red">程式碼執行有誤</font>，這時我們可以先將畫面切換至一個客制的錯誤頁面，告知User待修復，稍後嘗試。

<span id="inline-purple">範例</span>

程式碼執行 <font color="red">cc()</font> 這個Function出錯了，這時利用 `app.use` 的新參數 <font color="red">err</font> ,接收程式碼的錯誤訊息，並回應User系統有誤待修復。

``` js 後端app.js
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

<span id="inline-purple">範例</span>

下列重點為 isLogin 插入的位置，app.get('/', <font color="red">isLogin</font>, function (req, res){ 程式碼 })

``` js 後端app.js
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

<span id="inline-purple">範例</span>

``` js 後端app.js
var express = require('express');
var app = express();

//增加靜態檔案的路徑
app.use(express.static('public')); // 設定 目錄起點位置

app.get('/', function (req, res) {

    res.send('<html><body><h1>' +
    '<img src="/img/logo.png" >'+
        'Hello' +
        '</h1></body></html>');

})

//監聽port
var port = process.env.PORT || 3000;
console.log(port);
app.listen(port);

//執行 node app.js
//網頁連結 http://127.0.0.1:3000/
```

示意圖
{% asset_img express_02.png %}

***
## EJS - 樣板語言

<span id="inline-blue">安裝語法</span>
``` zsh
$ npm install ejs-locals --save
```

<span id="inline-purple">使用方法</span>

1. 宣告 樣板語言
2. 使用 res.render 讀取哪個ejs檔案，如：res.render('<font color="red">index</font>')

``` js 後端app.js
var express = require('express');
var app = express();

//增加靜態檔案的路徑
app.use(express.static('public')); // 設定 目錄起點位置

//宣告 樣板語言
var engine = require('ejs-locals');
app.engine('ejs',engine);
app.set('views','./views'); // 設定「樣板程式碼」放在哪裡
app.set('view engine','ejs'); // 設定「樣板語言」是用哪種？ejs、pug、handlebars


app.get('/', function (req, res) {
    res.render('index'); //渲染 index.ejs 的檔案
})
//http://127.0.0.1:3000/
```
示意圖
{% asset_img express_03.png %}

***
## EJS - 參數導入

除了一般將.ejs檔案渲染成HTML外，有時也會因為<font color="red">條件的不同</font>，要渲染出不一樣的HTML格式，這時就可以透過<font color="red">參數的傳遞</font>，來達成此需求。

<span id="inline-purple">範例</span>

下列範例示範 <font color="red">res.render</font>，如何`傳送參數`以及`ejs接收參數的寫法`

``` js 傳送 參數
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

``` html ejs接收參數的寫法
<!-- 
= 是 渲染成字串
- 是 渲染成HTML
-->

<%= html %>
=> <h1>我是HTML</h1>

<%- html %>
=> 我是HTML


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
## EJS - 設定layout

當有很多頁面都有用到<font color="red">共同的元素</font>(如：表頭、表尾...等)，這時我們可以將會一直重覆的區塊取出，撰寫在 `layout.ejs`，若有其他頁面需要`表頭、表尾`的部份，我只要將`layout.ejs` include在頁面即可。

<span id="inline-purple">範例</span>

layout.ejs
1. 將共用的部份撰寫在 `layout.ejs`
2. 新增 `<%- body %>` 這段語法，代表要放置不同頁面各自的內容。

``` html layout.ejs
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <header>
    表頭共用的部份(logo、navbar...等)
    </header>

    <%- body %>

    <footer>
    表尾共用的部份(作者、版權信息或者相關鏈接...等)
    </footer>
</body>
</html>

```

search.ejs
1. 在頁面寫入 `<% layout('layout') %>` 這段語法，代表載入共用HTML部份。
2. 接著開始撰寫HTML內容，而撰寫好的HTML，就會放置在layout.ejs檔案裡的`<%- body %>`位置。

``` html search.ejs搜尋頁面
<!-- 共用元素 -->
<% layout('layout') %>

<!-- 表單內容 -->
<form action="/searchList" method="post" >
    <input type="text" name="content" id="content" value="">
    <input type="submit" id="send" value="送出">
</form>

<script src="/js/all.js"></script>
```


***
## body-parser - 取得表單資料(傳統表單)

<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install body-parser --save
```

<span id="inline-purple">範例</span>

1. 在search搜尋頁面的 `searchText` 輸入搜尋文字後，點擊`送出`按鈕
2. 此時會將表單資料傳送(<font color="red">post</font>)給`/searchList`
3. 再透過後端 `req.body` 取得到表單資料，進行後續解析。

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


``` js 後端app.js
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
    res.redirect('search'); // 將畫面導回搜尋頁面
})
```
<div class="note primary">搜尋頁面(search) 是使用 app.<font color="red">get</font>
接收表單頁面(searchList) 是使用 app.<font color="red">post</font>

在<font color="red">searchList</font>處理完資料後，最後記得將畫面導回搜尋畫面 <font color="red">res.redirect('search');</font>
</div>

***
## body-parser - 取得表單資料(AJAX)

若達到頁面<font color="red">不跳轉</font>，就可回應相關訊息，此時就需要透過<font color="red">AJAX</font>的方式，達到此需求。

<span id="inline-purple">範例</span>

新增一路由 <font color="red">searchAJAX</font>，用來處理搜尋的相關資料，最後<font color="red">回傳給前端</font>。

``` js 後端app.js
var express = require('express');
var app = express();
var bodyParser = require('body-parser');

//增加靜態檔案的路徑
app.use(express.static('public'));

//宣告 樣板語言
var engine = require('ejs-locals');
app.engine('ejs', engine);
app.set('views', './views'); // 樣板放在哪裡？
app.set('view engine', 'ejs'); // 宣告樣板是用哪種語言？ejs、pug、handlebars


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


//AJAX
app.post('/searchAJAX', function (req, res) {
    //所有body欄位資料
    console.log(req.body);
    console.log(req.body.list[1]);

    /* 
        這裡撰寫程式碼
    */

    //回傳結果
    res.send('Hello!');
})

//監聽port
var port = process.env.PORT || 3000;
console.log(port);
app.listen(port);

//執行 node app.js
//網頁連結 http://127.0.0.1:3000/
```

重點在於我們將原本在搜尋頁面的 <font color="red">send按鈕</font> 要Submit的動作取消 `e.preventDefault();`，改用 <font color="red">AJAX</font> 方式，將表單的內容資料，利用 `post` 給 `searchAJAX`。

``` js 前端all.js
var send = document.querySelector('#send');
var content = document.querySelector('#content');

send.addEventListener('click', function (e) {
    e.preventDefault();

    var str = content.value;
    console.log(str);

    var xhr = new XMLHttpRequest();
    xhr.open('post', '/searchAJAX'); //發送給哪個路由

    //組合表單資料(文字型態)
    // xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    // var data = 'content=' + str; // content=123&title=abc

    //組合表單資料(json型態)
    xhr.setRequestHeader("Content-type", "application/json");
    var searchlist = {
        "content": str,
        "list": [1, 2, 3]
    };
    var data = JSON.stringify(searchlist); //轉成字串

    //發送表單資料給後端app.js處理
    xhr.send(data);

    //取得後端app.js回傳的資料
    xhr.onload = function () {
        console.log(xhr.responseText); 
    }

})
```

***
## Router - 進階設定

當我們網站有<font color="blue">越來越多頁面</font>的話，不可能把所有網頁的路由判斷在寫app.js裡，這樣會造成程式碼雜亂，導致後續維護不便，所以我們可以將<font color="blue">同類型頁面</font>分別包裝在不同<font color="red">js檔</font>裡，並且使用 <font color="red">module(模組)</font> 輸出，可供 <font color="red">app.js</font> 分別載入，以達到程式碼的簡潔易懂。


<span id="inline-purple">範例</span>

app.js 分別載入 {% label success@user模組(個人資料) %}、{% label success@cart模組(購物車) %} ，讓整個app.js變得簡潔易懂。

``` js 後端app.js
var express = require('express');
var user = require('./routes/user.js') // 載入 user模組(個人資料)
var user = require('./routes/cart.js') // 載入 cart模組(購物車)

var app = express();


//網址若為 http://127.0.0.1:3000/user/... 之類的話，就會進去user模組執行。
app.use('/user',user);

//網址若為 http://127.0.0.1:3000/cart/... 之類的話，就會進去cart模組執行。
app.use('/cart',cart);


//監聽port
var port = process.env.PORT || 3000;
app.listen(port);

//執行 node app.js
//網頁連結 http://127.0.0.1:3000/
```

以{% label success@user模組(個人資料) %}為例，因外部引用此模組，語法設定為 app.use('<font color="red">/user</font>',user);
所以網址起頭從 http://127.0.0.1:3000/<font color="red">/user</font>/ 開始

``` js user.js
var express = require('express');
var router = express.Router(); // 路由器

//因外部引用此模組，語法為 app.use('/user',user);
//所以網址起頭從 http://127.0.0.1:3000/user/ 開始


router.get('/edit-porfile', function (req, res) {
    //若網址為 http://127.0.0.1:3000/user/edit-porfile 進行此function
    console.log('porfile頁面');
    res.send('porfile');
})


router.get('/photo', function (req, res) {
    //若網址為 http://127.0.0.1:3000/user/photo 進行此function
    console.log('photo頁面');
    res.send('photo');
})

router.get('/card', function (req, res) {
    //若網址為 http://127.0.0.1:3000/user/card 進行此function
    console.log('card頁面');
    res.send('card');
})

/* 
原先都是使用 app.get ，要改用成 router.get
*/

module.exports = router;
```
<div class="note primary">注意 user.js，原先都是使用 <font color="red">app</font>.get ，要改用成 <font color="red">router</font>.get
</div>

***
## express-generator - 應用程式產生器

經過上述種種的介紹，了解express各種的使用方式後，之後我們可以利用 {% label danger@Express應用程式產生器 %} ，自動幫我們生成一個專案的基本架構，就無須從頭到尾慢慢建構。

<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install express-generator -g
```

<span id="inline-purple">建立語法</span>

1. 建立 express 專案資料
    ``` zsh
    $ express -e project
    ```

2. 切換至專案目錄，並安裝 NPM 套件
    ``` zsh
    $ cd project
    $ npm install
    ```

3. 運行專案
    ``` zsh
    $ npm start
    ```
4. 專案網址：http://127.0.0.1:3000/

<span id="inline-yellow">專案初始化</span>

下列為 <font color="red">專案初始化</font> 之程式碼，後面可在依據需求新增其他功能模組，如：`express-session`、`nodemailer(寄信)`、`csurf(阻擋跨站攻擊)`、`connect-flash(message暫存器)`、`dotenv(環境變數設定)`、`firebase-admin(資料庫)`…等

``` js 後端app.js
var express = require('express');
var path = require('path');                   // 抓取目錄路徑
var favicon = require('serve-favicon');       // 設定icon
var logger = require('morgan');               // 日誌
var cookieParser = require('cookie-parser');  // 解析前端cookie
var bodyParser = require('body-parser');      // 取得前端表單資料


// 自行撰寫頁面部份
var index = require('./routes/index'); // 載入 index模組(首頁)
var users = require('./routes/users'); // 載入 user模組(個人資料)


var app = express();

// view engine setup - 樣版語言設定
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');


// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico'))); // 設定icon的路徑
app.use(logger('dev'));
app.use(bodyParser.json());                               // 解析表單資料-json
app.use(bodyParser.urlencoded({ extended: false }));      // 解析表單資料-URL-encoded格式
app.use(cookieParser());                                  // 解析前端cookie
app.use(express.static(path.join(__dirname, 'public')));  // 設定 靜態檔案的目錄起點位置


// 路由區塊----Start
app.use('/', index);      //網址若為 http://127.0.0.1:3000/...     ，就會進去 index模組 執行。
app.use('/users', users); //網址若為 http://127.0.0.1:3000/user/...，就會進去 user模組  執行。
// 路由區塊----End


// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;

```
<div class="note info">[官方：Express應用程式產生器](http://expressjs.com/zh-tw/starter/generator.html)</div>

***
## 參考來源
<div class="note info">[NodeJS 前後端開發實戰](https://www.udemy.com/teachnodejs/)
[六角學院](http://www.hexschool.com/)</div>