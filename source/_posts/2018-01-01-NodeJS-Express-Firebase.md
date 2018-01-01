---
title: NodeJS-Express+Firebase
date: 2018-01-01 22:28:16
categories: 
- NodeJS
tags:
- NodeJS
- Express
- Firebase
- RESTful
---


{% cq %}

{% asset_img ef_01.png %}

<font style="font-size:20px;">紀錄如何使用 express + Firebase 設計 RESTful API Todoist</font>

{% endcq %}

<!-- more -->
***

## 取得Firebase Admin SDK

1. 開啟 專案設定 → 服務帳號

    {% asset_img ef_02.png %}

2. 設定方法

 - 安裝NPM `npm install firebase-admin --save`
 - 複製Admin SDK 設定程式碼片段，貼至 app.js
 - 下載Firebase金鑰(json檔案)，並修改Firebase金鑰路徑

    {% asset_img ef_03.png %}

3. 完成設定

    {% asset_img ef_04.png %}

***
## once、set 讀取、寫入

雖然之前介紹firebase<font color="red">讀取</font>的方式有 `once`、`on` 二種，
不過在後端的話，只能使用 `once` 方式，因為 `on` 在後端並不會有同步更新的效果。

``` js 讀取 once
var fireData = admin.database(); //取得Firebase的database

fireData.ref('todos').once('value', function (snapshot) {
    console.log(snapshot.val());
})
```

<font color="red">寫入</font>方式一樣可以使用 `set`、`push` 二種。

``` js 寫入 set、push
var fireData = admin.database(); //取得Firebase的database

// 寫法1
fireData.ref('todos').set({title: '待辦清單'})

// 寫法2
fireData.ref('todos').push({
    content: '要去看電影'
})
```

***
## then 設計思維

將資料新增至firebase後，可用 `then` 接著查看firebase的資料是否有成功新增。

``` js
var fireData = admin.database(); //取得Firebase的database

//將資料新增至firebase後，可用 then 接著查看firebase的資料是否有成功新增。
fireData.ref('todos').push({content: '要去看電影'})
.then(function () {
    //讀取todos，查看資料是否有寫入？
    fireData.ref('todos').once('value', function (snapshot) {
        console.log(snapshot.val());
    })
})
```

***
## EJS 整合 Firebase

用 todolist 為例，說明如何將firebase上的待辦事項與後端整合，並呈現於網頁上。

<span id="inline-purple">範例</span>

firebase資料

{% asset_img ef_05.png %}

取得Firebase的todos資料，並將資料傳入index。

> <font color="red">res.render 要放進</font> <font color="red">firebase function裡面</font>，因為是用AJAX取得資料，所以要確保取得資料後，才進行傳遞參數的動作。

``` js app.js
var fireData = admin.database(); //取得Firebase的database

//路由
app.get('/', function (req, res) {
    fireData.ref('todos').once('value', function (snapshot) {
        // console.log(snapshot.val());
        var data = snapshot.val();
        
        //render 要放進firebase function裡面，因為是用AJAX取得資料，所以要確保取得資料後，才進行傳遞參數的動作。
        res.render('index', {
            "todolist": data
        });
    })
})
```

取得傳入的資料 `todolist`，使用ejs的forloop寫法，將資料渲染成HTML。

``` html index.ejs
<h1>待辦清單</h1>
<input type="text" id="content" value="">
<input type="submit" id="send" value="送出">
<ul id="list">
    <!-- forloop寫法 -->
    <% for(item in todolist){  %>
        <li> 
            <%- todolist[item].content %>
            <input type="button" data-id="<%- item %>" name="" value="刪除">
        </li>
    <% } %>

</ul>
```

***
## 新增資料API

前端畫面輸入 `待辦事項` 後，點擊 `送出` 按鈕，此時會將資料 <font color="red">post</font> 給 <font color="red">addTodo</font> 路由，後端處理完後，再將結果傳送給前端更新畫面。

<span id="inline-purple">範例</span>

``` js 後端app.js
var fireData = admin.database(); //取得Firebase的database

// 新增邏輯
app.post('/addTodo', function (req, res) {
    var content = req.body.content; // 取得前端的content欄位資料
    var contentRef = fireData.ref('todos').push(); //先設定寫入的方法為push

    // 當資料寫入成功後，並使用 then 回傳資料，送至前端渲染畫面
    contentRef.set({"content": content}).then(function () {
        fireData.ref('todos').once('value', function (snapshot) {
            // console.log(snapshot.val());

            res.send({
                "succes": true,
                "result": snapshot.val(), //todos的資料
                "message": "資料新增成功"
            });
        })
    })
})
```
> [官方：先設定push的寫法](https://firebase.google.com/docs/database/admin/save-data#section-push)

``` html HTML架構
<body>
    <h1>待辦清單</h1>
    <input type="text" id="content" value="">
    <input type="submit" id="send" value="送出">

    <ul id="list">
        <!-- forloop寫法 -->
        <% for(item in todolist){  %>
            <li> 
                <%- todolist[item].content %>
                <input type="button" data-id="<%- item %>" name="" value="刪除">
            </li>
        <% } %>
    </ul>

    <script src="/js/all.js"></script>
</body>
```

``` js 前端all.js
var send = document.querySelector('#send');
var content = document.querySelector('#content');
var list = document.querySelector('#list');

//新增API
send.addEventListener('click', function (e) {
    e.preventDefault();

    var xhr = new XMLHttpRequest();
    xhr.open('post', '/addTodo'); // 新增資料API
    xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8');

    //取得輸入 待辦事項
    var str = content.value; 
    content.value = "";
    var todo = JSON.stringify({"content": str}); // 轉換成字串
    xhr.send(todo);


    //AJAX回傳後的結果
    xhr.onload = function () {
        var originData = JSON.parse(xhr.responseText);
        // console.log(originData);

        //重載todoList
        reloadList(originData);
    }
})

//Reload todoList
function reloadList(originData) {

    if (originData.success === false) {
        alert(originData.message);
        return;
    }

    var data = originData.result;
    var str = '';
    for (item in data) {
        str += '<li>' + data[item].content + '<input type="button" data-id="' + item + '" name="" value="刪除">' + '</li>'
    }

    list.innerHTML = str;
}
```

***
## 刪除資料API

前端點擊 `刪除` 按鈕，此時會將刪除按鈕的 <font color="red">data-id</font> 資料 <font color="red">post</font> 給 <font color="red">removeTodo</font> 路由，後端處理完後，再將結果傳送給前端更新畫面。

<span id="inline-purple">範例</span>

``` js 後端app.js
// 刪除邏輯
app.post("/removeTodo", function (req, res) {
    var _id = req.body.id; // 取得刪除按鈕的 data-id

    //刪除todos的資料成功後，並使用 then 回傳資料，送至前端渲染畫面
    fireData.ref('todos').child(_id).remove().then(function () {
        fireData.ref('todos').once('value', function (snapshot) {
            // console.log(snapshot.val());
            res.send({
                "succes": true,
                "result": snapshot.val(),
                "message": "資料刪除成功"
            });
        })
    })
})
```

在每一個的<font color="red">INPUT(刪除按鈕)</font>上，新增<font color="red">data-id</font>屬性，紀錄firebase的Key值。

``` html HTML架構
<body>
    <h1>待辦清單</h1>
    <input type="text" id="content" value="">
    <input type="submit" id="send" value="送出">

    <ul id="list">
        <!-- forloop寫法 -->
        <% for(item in todolist){  %>
            <li> 
                <%- todolist[item].content %>
                <input type="button" data-id="<%- item %>" name="" value="刪除">
            </li>
        <% } %>
    </ul>

    <script src="/js/all.js"></script>
</body>
```

下例 前端all.js ，是針對 <font color="red">ul</font> 進行監聽，進而判斷點擊的元素是否為<font color="red">INPUT(刪除按鈕)</font>，這樣就不用針對 <font color="red">n個的刪除按鈕</font> 建立監聽事件，以達到優化的效果。

``` js 前端all.js
var send = document.querySelector('#send');
var content = document.querySelector('#content');
var list = document.querySelector('#list');

//刪除API
list.addEventListener('click', function (e) {

    //判斷元素是否為 INPUT(刪除按鈕)
    if (e.target.nodeName !== "INPUT") {
        return;
    }

    var xhr = new XMLHttpRequest();
    xhr.open('post', '/removeTodo'); // 刪除API
    xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8');

    //取得輸入 data-id 資料
    var id = e.target.dataset.id;
    var todo = JSON.stringify({"id": id}); // 轉成字串
    xhr.send(todo);

    //AJAX回傳後的結果
    xhr.onload = function () {
        var originData = JSON.parse(xhr.responseText);
        // console.log(originData);

        //重載todoList
        reloadList(originData);

    }

})

//Reload todoList
function reloadList(originData) {

    if (originData.success === false) {
        alert(originData.message);
        return;
    }

    var data = originData.result;
    var str = '';
    for (item in data) {
        str += '<li>' + data[item].content + '<input type="button" data-id="' + item + '" name="" value="刪除">' + '</li>'
    }

    list.innerHTML = str;
}
```