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
## 後端 讀取Cookie、寫入Cookie

<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install cookie-parser --save
```

> [cookie-parser](https://github.com/expressjs/cookie-parser)

<span id="inline-purple">範例</span>


``` js app.js引用cookie-parser模組
var express = require('express')
var cookieParser = require('cookie-parser') // 解析cookie模組

var app = express()
app.use(cookieParser()) //解析前端cookie
```

- 「讀取」是用 req(Request)
- 「寫入」是用 res(Response)

``` js routes/index.js
var express = require('express');
var router = express.Router();


router.get('/', function (req, res, next) {
  
  // 「讀取」是用 req(Request)
  // 讀取前端的cookies(全部)
  console.log(req.cookies);

  // 讀取前端的cookies(單個)
  console.log(req.cookies.userName);


  // 「寫入」是用 res(Response)
  // 後端寫入前端Cookies
  res.cookie('name', 'lulu', {
    maxAge: 10000, // 只存在n秒，n秒後自動消失
    httpOnly: true // 僅限後端存取，無法使用前端document.cookie取得
  })


  res.render('index', {title: 'Express'});
});

module.exports = router;
```

<span id="inline-green">補充：Response.cookie(寫入)</span>

`res.cookie(name, value [, options])`

參數：
- name: 設定cookie的名字
- value: 設定其值，可能是字串或是轉成JSON格式的物件。
- options: 選項參數是一個物件，所以，其<font color="red">屬性</font>放在{}裡，以逗號分隔。

屬性：
- domain (字串) 適用此cookie的domain name
- encode (函式) 用於cookie編碼的同步函式，預設encodeURIComponent.
- expires (日期) cookie的到期日，超過此日期，即失效。
- httpOnly (布林) 標記此cookie只能從web server　訪問，以避免不正確的進入來取得竄改。
- maxAge (數字) 設定此cookie的生存時間(毫秒為單位)，比方60000(10分鐘後到期，必須重新訪問)
- path (字串) 適用此cookie的路徑，預設： “/”.
- secure (布林) 設定此cookie是否只在https使用。
- signed (布林) 此cookie是否要設簽章。(如果是true，必須使用cookie-parser設定簽章 )

<div class="note info">[官方-req.cookies(讀取)](http://expressjs.com/zh-tw/4x/api.html#req.cookies)
[官方-res.cookie(寫入)](http://expressjs.com/zh-tw/4x/api.html#res.cookie)
[Cookie在express上的應用](https://ithelp.ithome.com.tw/articles/10187343)</div>


***
## 後端 session

- 儲存在伺服器的<font color="red">暫存資料</font>，此暫存可放在記憶體或資料庫上
- session可在cookie上儲存一筆辨識你是誰的 session id


<span id="inline-blue">安裝語法</span>

``` zsh
$ npm install express-session --save
```

> [express-session](https://github.com/expressjs/session)


<span id="inline-yellow">session 常用參數</span>

- name：設置cookie中，保存session的字段名稱。(預設：<font color="red">connect.sid</font>)
- store： session 的存儲方式。(預設：存放在<font color="red">內存(記憶體)</font>中，也可以使用redis，mongodb 等)
- <font color="blue">secret(必要選項)</font>： 通過設置的 secret 字符串，來計算<font color="red">hash</font>值並放在cookie 中，使產生的 signedCookie 防篡改。
- genid：產生一個新的 session_id 時，所使用的函數。(預設：使用uid2這個npm包)
- rolling： 每個請求都重新設置一個cookie。(預設：<font color="red">false</font>)
- resave： 即使 session 沒有被修改，也保存session 值。(預設：<font color="red">true</font>)
- saveUninitialized：強制將未初始化的session存回 session store，未初始化的意思是它是新的而且未被修改。(預設：<font color="red">true</font>)
- cookie： 設置存放 session id 的 cookie 的相關選項。
  - httpOnly：為設置<font color="red">true</font>時，將<font color="red">不允許</font>的 `JavaScript` 通過 `document.cookie` 訪問取得資料。
  - maxAge： number（毫秒），用於指定Set-Cookie的Expires屬性。設置後，Cookie將在指定時間後失效。
  - path： 用於指定Set-Cookie的Path屬性。默認會被設置為'/'，即當前域名的根路徑。
  - secure： 設置為<font color="red">true</font>時，使用非HTTPS連接時，當客戶端將不會回傳的Cookie。

    ``` js cookie預設值
    default: { 
      path: '/',
      httpOnly: true,
      secure: false,
      maxAge: null 
    }
    ```

<span id="inline-purple">範例</span>

{% asset_img cookie_01.png %}

當User登入後，伺服器先提供一組<font color="red">session id</font>給你，並紀錄在 <font color="red">客戶的瀏覽器的cookie</font>

``` js app.js
var express = require('express')
var session = require('express-session')

var app = express()

//提供一組session id
app.use(session({
  secret: 'iamkanboo', //
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

<div class="note info">[session 在 express 上的應用](https://ithelp.ithome.com.tw/articles/10187464)</div>

***
## 參考來源
<div class="note info">[NodeJS 前後端開發實戰](https://www.udemy.com/teachnodejs/)
[六角學院](http://www.hexschool.com/)</div>