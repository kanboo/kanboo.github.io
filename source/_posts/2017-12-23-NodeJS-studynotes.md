---
title: NodeJS-基礎觀念
date: 2017-12-24 14:47:40
categories: 
- NodeJS
tags:
- NodeJS
---

{% cq %}

{% asset_img node_01.png %}

{% endcq %}

<!-- more -->
***

## node全域物件：global

- node的全域物件為 <span id="inline-green">global</span> ，就像網頁的全域物件為 <span id="inline-purple">window</span>。

- 如下圖，在node環境下，不同js檔之間是區隔的，使用到相同的變數名稱是不會互相影響。

{% asset_img node_02.png %}

***
## require、module exports

由於node環境下，不同js檔之間是區隔的，所以若想要在js檔之間達到資料的傳遞的話，方法如下


``` js app.js 引用語法
//引用 模組
var content = require('./data'); //設定預引用的js檔路徑，不用加副檔名

console.log(content.data);
console.log(content.bark());
```

輸出語法有二種，可任意選擇喜歡的寫法，但不能二種寫法同時在同一個js檔裡，因為會互相覆蓋掉。

``` js data.js 輸出語法

//第一種：輸出資料
var data = 2;

//輸出的資料
module.exports = {
    content: data,
    title: 'big'
};


//第二種：輸出資料
exports.data = 2;

exports.bark = function(){
    return 'bark!!';
}
```

***
## Node核心模組-createServer

createServer的參數：

request：接收User的行為，如：request.url 可取得目前網頁網址
response：回應User的結果


``` js
var http = require('http');


http.createServer(function (request, response) {
    
    // 輸出 純文字
    // response.writeHead(200, {"Content-Type": "text/plain"}); //文字格式
    // response.write('hello word!!');

    //輸出 HTML
    response.writeHead(200, {"Content-Type": "text/html"}); //HTML格式
    response.write('<h1>hello word!!</h1>');

    response.end();
}).listen(8080); //監聽 8080 port
```

<div class="note primary">常見的port：
21 FTP
80 http
3389 遠端桌面
</div>

***
## Node模組-Path

__dirname：取得目前js檔的<font color="red">**目錄**</font>路徑，如：/Users/kanboo/Desktop/project

__filename：取得目前js檔的<font color="red">**檔案**</font>路徑，如：/Users/kanboo/Desktop/project/<font color="blue">app.js</font>

``` js
var path = require('path');

//抓目錄路徑
console.log(path.dirname('/xx/yy/zz.js'));
//=> /xx/yy

//路徑合併
console.log(path.join(__dirname,'/xx'));
//=> /Users/kanboo/Desktop/project/xx

//抓檔名
console.log(path.basename('xx/yy/zz.js'));
//=> zz.js

//抓副檔名
console.log(path.extname('xx/yy/zz.js'));
//=> .js

//分析路徑
console.log(path.parse('xx/yy/zz.js'));
//=> { root: '', dir: 'xx/yy', base: 'zz.js', ext: '.js', name: 'zz' }
```

<div class="note info">[Node.js PATH API文件](https://nodejs.org/api/path.html)</div>

***
## NPM-nodemon

nodemon是一個專為Node.js設計的模組，它的作用是**持續監視著你的程式碼**，一旦你修改後保存了，nodemon就會重新啟動你的Node.js程式，這樣你只要刷新你的瀏覽器就能看到改動。

``` zsh 安裝語法
npm install -g nodemon
```

``` js 啟動語法
//原先啟動server的方法
node index.js

//改用此方法
nodemon index.js
```

<div class="note info">[nodemon](https://nodemon.io/)</div>