---
title: NodeJS-Nodemailer
date: 2018-01-03 19:32:01
categories: 
- NodeJS
tags:
- NodeJS
- Nodemailer
- mail
---

{% cq %}

{% asset_img nodemailer_01.png %}

<font style="font-size:20px;">NodeJS如何實現寄信</font>

{% endcq %}

<!-- more -->
***

## 前言

Mail寄送功能使用 <font color="red">Gmail</font> 當作郵件伺服器，所以需先開通權限，開通後，就可以慢慢新增寄信的功能，以及額外優化的部份。

> [Gmail-低安全性應用程式](https://myaccount.google.com/lesssecureapps)

***
## nodemailer - 發信功能的介接

<span id="inline-blue">安裝</span>

``` zsh
$ npm install nodemailer --save
```

<span id="inline-purple">範例</span>

實現基本寄信的功能，步驟如下

1. 載入 Mail模組
2. 設定 SMTP 相關資料
3. 撰寫Mail相關內容
4. 發送郵件

不過在實際寄信的過程，有出現錯誤訊息為 <font color="red">self signed certificate in certificate chain</font>，此錯誤訊息，將在後面小節額外說明。

``` js 起手式
var express = require('express');
var router = express.Router();
var nodemailer = require('nodemailer'); // Mail模組

//設定 SMTP 相關資料
var transporter = nodemailer.createTransport({
    service: 'Gmail',
    secureConnection: true, // 使用SSL方式（安全方式，防止被竊取信息）
    auth: {
        user: 'Gmail帳號', // generated ethereal user
        pass: 'Gmail密碼' // generated ethereal password
    },
    tls: {
        // 不得檢查服務器所發送的憑證
        rejectUnauthorized: false
    }
});


//撰寫Mail相關內容
var mailOptions = {
    from: '"王康寶" <wang.kanboo@gmail.com>', // sender address
    to: 'wang.kanboo@gmail.com', // list of receivers
    subject: 'Node測試寄信', // Subject line
    text: '測試寄信內容', // plain text body
    html: '<p>HTML version of the message</p>'
};


//發送郵件
transporter.sendMail(mailOptions, function (error, info) {
    if (error) {
        return console.log(error);
    }

    //轉址
    res.redirect('/contact/review');
})
```

<div class="note info">[Nodemailer](https://nodemailer.com/about/)</div>

***
## TLS - 傳輸層安全性協定

在實際寄信的過程，有出現錯誤訊息為 <font color="red">self signed certificate in certificate chain</font>，
原因為Gmail為HTTPS協定，會發送的 <font color="red">CA 簽名的證書</font>，
而Node.js 客戶機驗證CA 簽名的證書，方法是根據CA 的<font color="blue">公用</font>證書檢查CA <font color="blue">簽名</font>的證書，
不過因為我們沒有 CA 的<font color="blue">公用</font>證書，所以我們需增加一個參數 <font color="red">rejectUnauthorized</font> ，去忽略此驗證的動作。


``` diff 新增 tls 的 rejectUnauthorized
//設定 SMTP 相關資料
var transporter = nodemailer.createTransport({
    service: 'Gmail',
    secureConnection: true, // 使用SSL方式（安全方式，防止被竊取信息）
    auth: {
        user: 'Gmail帳號', // generated ethereal user
        pass: 'Gmail密碼' // generated ethereal password
    },
+   tls: {
+       // 不得檢查服務器所發送的憑證
+       rejectUnauthorized: false
+   }
});
```

<div class="note info">[self signed certificate in certificate chain node js?
](https://stackoverflow.com/questions/47266393/self-signed-certificate-in-certificate-chain-node-js)
[Allow self-signed certificates](https://nodemailer.com/smtp/#3-allow-self-signed-certificates)</div>

***
## nodemailer 新增附件

擷取 <font color="red">撰寫Mail相關內容</font> 這一部份，新增 <font color="red">attachments</font> 參數，並且該如何正確取得伺服器電腦上的附件。

注意的點：

1. 附件路徑需設定 <font color="red">完整路徑</font>
2. 可透過 `process.cwd()` 取得目前專案的目錄，在延伸至放附件的路徑，如：`path: process.cwd() + '/public/attach/image.png'`
3. 另外可新增 <font color="red">cid</font> 的值，可供 <font color="red">郵件html</font> 渲染出附件檔案。

``` diff 新增附件
//撰寫Mail相關內容
var mailOptions = {
    from: '"王康寶" <wang.kanboo@gmail.com>', // sender address
    to: 'wang.kanboo@gmail.com', // list of receivers
    subject: 'Node測試寄信', // Subject line
    html: req.body.description +
            '<br/><br/>' +
+            '<img src="cid:image_001"/>' + // 引用attachments的cid
            '<br/>' +
            '來自電子郵件：' + req.body.email, // plain text body
+   attachments: [{
+       filename: 'text.txt',
+       content: '軍整風綠業新！'
+    }, {
+       filename: 'image.png',
+       path: process.cwd() + '/public/attach/image.png', // 附件路徑
+       cid: 'image_001' //cid可被郵件使用
+   }]
};
```

結果
{% asset_img nodemailer_02.png %}

<div class="note info">[What is the right way to set attachments path in nodeMailer?
](https://stackoverflow.com/questions/29912055/what-is-the-right-way-to-set-attachments-path-in-nodemailer)
[Nodemailer-Attachments](https://nodemailer.com/message/attachments/)</div>

***
## CSURF - 阻擋跨站攻擊

為了預防 [跨站請求偽造](https://zh.wikipedia.org/zh-tw/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0) 的攻擊，所以我們使用 <font color="red">CSURF模組</font> 來協助我們驗證是否為本伺服器所發出的請求，簡單來說，就是利用 `後端session` 和 `前端cookie` 的配合，達成此驗證功能。

<span id="inline-blue">安裝</span>

``` zsh
$ npm install csurf --save
```

<span id="inline-purple">範例</span>

1. 載入模組後，建立CSURF驗證
2. 在 <font color="red">get、post</font> 之前，都需新增 <font color="red">middlewares</font> 的卡控判斷。
3. 在前端表單，新增一段 在 <font color="red">csrfToken的HTML</font>，供驗證使用。

``` js 後端.js
var express = require('express');
var router = express.Router();
var nodemailer = require('nodemailer'); // Mail模組
var csrf = require('csurf'); // 阻擋跨站攻擊(CSURF)

// 建立一個cookie供CSURF驗證 - setup route middlewares
var csrfProtection = csrf({
    cookie: true
});


//傳入CSURF的驗證
router.get('/', csrfProtection, function (req, res) {
    res.render('contact', {
        csrfToken: req.csrfToken() //傳入csrfToken驗證資料
    });
});


//接收資料時，驗證csrfToken是否正確
router.post('/post', csrfProtection, function (req, res) {

    /* 
    your code
    */

    res.redirect('/contact/review');

});
```

``` diff 前端表單：紀錄由後端提供的csrfToken
<form action="/contact/post" method="post">
+       <input type="hidden" name="_csrf" value="<%= csrfToken %>">

        <h1>聯絡我們</h1>
        <div>
            <label for="email">電子郵件：</label>
            <input type="text" name="email" id="email"/>
        </div>
        <div>
            <input type="submit" value="送出" />
        </div>
    </form>
```

<div class="note info">[CSURF](https://github.com/expressjs/csurf)</div>

***
## dotenv - 環境變數設定

可以將一些比較機密性的資料(帳號、密碼…等)，設置在環境變數裡，這樣的話，就算我們將專案上傳至github等公共環境，也不會將重要的資訊曝露在外。


<span id="inline-blue">安裝</span>

``` zsh
$ npm install dotenv --save
```

<span id="inline-purple">範例</span>

1. 在專案根目錄新增檔案 <font color="red">.env</font>，並在將機密資料設定於此。
2. 修改後端app.js，機密資料 改取自 dotenv環境變數。

``` js .env檔案
gmailUser = Gmail帳號
gmailPass = Gmail密碼
```

``` diff 後端.js
var express = require('express');
var router = express.Router();
var nodemailer = require('nodemailer'); // Mail模組
var csrf = require('csurf'); // 阻擋跨站攻擊(CSURF)
+require('dotenv').config(); // 環境變數設定

//設定 SMTP 相關資料
var transporter = nodemailer.createTransport({
    service: 'Gmail',
    secureConnection: true, // 使用SSL方式（安全方式，防止被竊取信息）
    auth: {
+       // 帳密取自 dotenv環境變數
+       user: process.env.gmailUser, // generated ethereal user
+       pass: process.env.gmailPass // generated ethereal password
    },
    tls: {
        // do not fail on invalid certs
        rejectUnauthorized: false
    }
});

```

<div class="note info">[dotenv](https://www.npmjs.com/package/dotenv)</div>

***
## connect-flash - message的暫存器

簡單來說，flash是一個暫存器，而且暫存器裡面的值<font color="red">使用過一次即被清空</font>，這樣的特性很方面用來做網站的提示信息。

<span id="inline-blue">安裝</span>

``` zsh
$ npm install connect-flash --save
```

<span id="inline-purple">範例</span>

注意：flash要配合session使用

``` js 後端app.js
var express = require('express');
var session = require("express-session");
var flash = require('connect-flash'); // message的暫存器

var app = express();

//使用connect-flash前，需先設定session相關設定，才能使用
app.use(session({
  secret: 'iamkanboo',
  resave: true,
  saveUninitialized: true
}));

app.use(flash()); // 啟用 message的暫存器(session設定完，才use)
```

驗證欄位是否有填寫，若有卡控訊息，則紀錄在 <font color="red">flash</font> 內，並傳入 <font color="red">render</font> ，供前端渲染畫面。

``` js 後端contact.js
router.get('/', csrfProtection, function (req, res) {
    res.render('contact', {
        csrfToken: req.csrfToken(),
        errors: req.flash('errors') //傳入 卡控訊息
    });
});


router.post('/post', csrfProtection, function (req, res) {
    //欄位卡控
    if (req.body.username == '') {
        console.log('username是空的');
        req.flash('errors', 'username是空的'); // 紀錄 卡控訊息
        res.redirect('/contact');
        return;
    }

    /*  your code  */

    //轉址
    res.redirect('/contact/review');

});   

```


``` diff 前端：顯示卡控訊息
<form action="/contact/post" method="post">
    <input type="hidden" name="_csrf" value="<%= csrfToken %>">

+   <%= errors %>

    <h1>聯絡我們</h1>
    <div>
        <label for="username">姓　　名：</label>
        <input type="text" name="username" id="username"/>
    </div>
    <div>
        <label for="email">電子郵件：</label>
        <input type="text" name="email" id="email"/>
    </div>
    <div>
        <input type="submit" value="送出訊息" />
    </div>
</form>

```

<div class="note info">[express4.0之後使用connect-flash應該注意的問題](https://my.oschina.net/u/1771420/blog/333009)</div>

***
## 參考來源
<div class="note info">[NodeJS 前後端開發實戰](https://www.udemy.com/teachnodejs/)
[六角學院](http://www.hexschool.com/)</div>