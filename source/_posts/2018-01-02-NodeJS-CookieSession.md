---
title: NodeJS-Cookie、Session
date: 2018-01-02 20:42:36
categories: 
- NodeJS
tags:
- NodeJS
- Cookie
- Session
---


{% cq %}

<font style="font-size:20px;">紀錄Cookie、Session的差異，以及如何搭配使用。</font>

{% endcq %}

<!-- more -->
***

## COOKIE 與 SESSION 的比較與區別

- <font color="red">Cookie</font> 
  -數據存放在<font color="blue">客戶的瀏覽器</font>上
  -用戶端的限制約<font color="blue">4K</font>
  -不存放機密資料，避免被他人解析，故僅存放 session id

- <font color="red">Session</font> 
  -數據放在<font color="blue">伺服器</font>上
  -session資料是 <font color="blue">暫存</font>

***
## 前端 Cookie

### 寫入 Cookie

`document.cookie = "myName=tom";`

### 寫入 Cookie，並加入過期時間，

`document.cookie="username=bob; expires=Mon, 04 Dec 2017 08:18:32 GMT; path=/";`

### GMT 時間

`new Date().toGMTString()`

### 寫入 Cookie，設定 10 秒後失效

`document.cookie="username=bob; max-age=10; path=/";`


***
## 後端 解析Cookie

<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install cookie-parser --save
```

> [cookie-parser](https://github.com/expressjs/cookie-parser)

<span id="inline-purple">範例(讀取、寫入)</span>


``` js app.js引用cookie-parser模組
var express = require('express')
var cookieParser = require('cookie-parser') // 解析cookie模組

var app = express()
app.use(cookieParser()) //解析前端cookie
```

``` js routes/index.js
var express = require('express');
var router = express.Router();


router.get('/', function (req, res, next) {
  
  //讀取前端的cookies(全部)
  console.log(req.cookies);

  //讀取前端的cookies(單個)
  console.log(req.cookies.userName);


  //後端寫入前端Cookies
  res.cookie('name', 'lulu', {
    maxAge: 10000, // 只存在n秒，n秒後自動消失
    httpOnly: true // 僅限後端存取，無法使用前端document.cookie取得
  })


  res.render('index', {title: 'Express'});
});

module.exports = router;
```

***
## 後端 session

- 儲存在伺服器的<font color="red">暫存資料</font>，此暫存可放在記憶體或資料庫上
- session可在cookie上儲存一筆辨識你是誰的 session id

<span id="inline-yellow">概念</span>

{% asset_img cookie_01.png %}

<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install express-session --save
```

> [express-session](https://github.com/expressjs/session)


<span id="inline-purple">範例</span>

當User登入後，伺服器先提供一組<font color="red">session id</font>給你，並紀錄在 <font color="red">客戶的瀏覽器的cookie</font>

``` js app.js
var express = require('express')
var parseurl = require('parseurl')
var session = require('express-session')

var app = express()

//提供一組session id
app.use(session({
  secret: 'keyboard cat',
  resave: true,
  saveUninitialized: true,
  cookie:{
    maxAge: 100*1000 //100秒後過期
  }
}))
```

表單點擊送出後，會將User資料 <font color="red">post</font> 至後端

``` html html架構
<body>

  <!-- 顯示user資料 -->
  <h1><%= userName %> <%= email %></h1>

  <!-- 表單填寫區 -->
  <form method="post" action="/">
    <input type="text" name="username" value="">    
    <input type="text" name="email" value="">    
    <input type="submit"  value="送出">    
  </form>

</body>
```

前端post資料後，會先將user資料寫入 <font color="red">session暫存</font>，再將網址導回index，並且傳入資料供渲染畫面。

``` js index.js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {

  //將user資料傳入index.ejs供渲染畫面
  res.render('index', { 
    userName: req.session.username,
    email: req.session.email
   });
});

//接收前端傳入的資料
router.post('/',function(req,res){

  //取得前端資料，並寫入至後端session暫存
  req.session.username = req.body.username;
  req.session.email = req.body.email;

  //轉址至首頁
  res.redirect('/');
})

module.exports = router;
```

***
## 參考來源
<div class="note info">[NodeJS 前後端開發實戰](https://www.udemy.com/teachnodejs/)
[六角學院](http://www.hexschool.com/)</div>