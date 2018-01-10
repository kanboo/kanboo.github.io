---
title: Firebase-基礎用法
date: 2017-12-26 16:24:04
categories: 
- Firebase
tags:
- Firebase
---


{% cq %}

{% asset_img firebase_01.png %}

{% endcq %}

<!-- more -->
***

## ref(路徑)、set(新增)


ref()：尋找資料庫路徑(預設：<font color="red">根目錄</font>)
set()：新增資料(寫入方式：<font color="red">覆蓋</font>)

``` js 新增資料：範例1
firebase.database().ref().set('hi');
```

結果
{% asset_img firebase_02.png %}


``` js 新增資料：範例2(物件)
// firebase 全部物件格式，不能陣列內容
// 一個班級有兩個學生
var school = {
    student1: {
        name: 'tom',
        num: '1'
    },
    student2: {
        name: 'john',
        num: '2'
    }
}

firebase.database().ref().set(school);

```
結果
{% asset_img firebase_03.png %}

<span id="inline-yellow">補充</span>

延續範例2，若要再新增 student3 的話，因為 `set` 寫入的方式是 <font color="red">覆蓋</font>，所以要注意 `ref` 是否有指定到正確的新增位置，不然有可能會覆蓋到原來的資料。

``` js 新增student3
var student3= {
    name: 'kanboo',
    num: '3'
}

// ref 要先指定到 新增的路徑
firebase.database().ref('student3').set(student3);
```


``` js 新增資料：範例3(階層式)
//寫入資料
firebase.database().ref('myName').set('kanboo');
firebase.database().ref('yourName').set('alice');
firebase.database().ref('allName/name01').set('aa');
firebase.database().ref('allName/name02').set('bb');
firebase.database().ref('allName/nameList/name01').set('cc');
```
結果
{% asset_img firebase_04.png %}

***
## once、on 讀取資料

- once(讀取一次資料庫的資料)

    僅向firebase取得<font color="red">一次性</font>資料，所以當firebase資料有<font color="red">異動</font>，需要<font color="red">再一次呼叫</font>，才會取得更新後的資料。

    ``` js
    var myNameRef = firebase.database().ref('allName/nameList/name01');
    // 快照
    myNameRef.once('value', function (snapshot) {
        console.log(snapshot.val());
        document.getElementById('title').textContent = snapshot.val();
    })
    ```

- on(隨時監聽)

    當firebase資料有<font color="red">異動</font>時，會<font color="red">即時回傳</font>更新資料。

    ``` js
    var myNameRef = firebase.database().ref('allName/nameList/name01');
    //on 隨時監聽
    myNameRef.on('value', function (snapshot) {
        console.log(snapshot.val());
        document.getElementById('title').textContent = snapshot.val();
    })
    ```

***
## push - 新增資料

以 Todolist 為例，因為資料隨時會新增，不可能一次就能把所有資料打齊上傳至database，所以就需要借用 <font color="red">push</font> 功能，將 新資料 加在 現有資料 裡。

``` js Todolist：新增待辦
var todos = firebase.database().ref('todos');
todos.push({content:'要去看電影'});
todos.push({content:'要去看跑步'});
```
結果
{% asset_img firebase_05.png %}


<div class="note warning"><font color="red">push</font> 會<font color="blue">自動</font>給一個亂數編號，如果不想產生隨機編號的話，就使用set方式。
<font color="red">set</font> 可以給固定的編號，但要注意新增的位置是否指定正確，要小心覆蓋掉已有的資料。</div>

***
## child 子路徑、remove 移除

以 Todolist 為例，當<font color="blue">完成</font>某項事件時，就要將它從待辦清單 <font color="red">移除</font>。

``` js Todolist：移除待辦
// child 子路徑：移至根目錄下的 todos 
var todos = firebase.database().ref().child('todos');


// remove 移除：移除todos下的 -L1H5cAAQnE9y72dCpJ7(要去看電影)
todos.child('-L1H5cAAQnE9y72dCpJ7').remove();
```

結果
{% asset_img firebase_06.png %}

***
## 在網頁即時顯示firebase資料

可以用 <font color="red">on(隨時監聽)</font> 的特性，隨時將firebase最新的資料回傳更新，這樣在開發時，就不用特地再開firebase的網頁觀看資料，節省切換頁面的時間。

``` html HTML
<pre id="content"></pre>
```

``` js js
// 在網頁即時顯示firebase資料
var ref = firebase.database().ref();
ref.on('value', function (snapshot) {
    document.getElementById('content').textContent = JSON.stringify(snapshot.val(), null, 3);
})
```

<div class="note primary">[JSON.stringify補充說明](https://segmentfault.com/a/1190000011239168)</div>

***
## orderbyForeach 排序


從firebase取得有 <font color="red">排序</font> 過後的資料，實作語法需要利用 `orderByChild` 和 `forEach` 二個函數搭配使用。

``` js people物件資料
var people = {
    "bob": {
        "height": 178,
        "old": 18,
        "weight": 70
    },
    "casper": {
        "height": 180,
        "old": 13,
        "weight": 80
    },
    "mike": {
        "height": 162,
        "old": 15,
        "weight": 55
    }
};
```

``` js 依「體重」排序
var peopleRef = firebase.database().ref('people');
// 路徑>>排序('屬性')>>讀取> forEach 依序撈出資料
peopleRef.orderByChild('height').once('value', function (snapshot) { 
    // console.log(snapshot.val());
    snapshot.forEach(function (item) {
        console.log(item.val());
    })
})
```

<span id="inline-yellow">延伸問題</span>

firebase 只提供一種排序方式，並無 <font color="red">反向</font> 排序的設定，所以若要達成反向排序，需借用 <font color="red">Array.reverse()</font> 的幫忙。

``` diff 反向範例
    var peopleRef = firebase.database().ref('people');
    // 路徑>>排序('屬性')>>讀取> forEach 依序撈出資料
    peopleRef.orderByChild('height').once('value', function (snapshot) { 
        // console.log(snapshot.val());
+       var dataArr = [];
        snapshot.forEach(function (item) {
            console.log(item.val());
+           dataArr.push(item.val());
        })
        //反向排序
+       console.log(dataArr.reverse()); 
    })
```

<div class="note primary">[firebase排序規則](https://firebase.google.com/docs/database/admin/retrieve-data?hl=zh-cn#orderbychild)
[Array.reverse()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)</div>


***
## startAt、endAt、equalTo 過濾條件

- 過濾條件：
    - startAt() 多少以上
    - endAt() 多少以下
    - equalTo() 相等 

使用 <font color="red">過濾條件</font> 時，一定要先 <font color="red">排序(orderByChild)</font> 過資料 


``` js 一個 過濾條件
var peopleRef = firebase.database().ref('people');
// 將 重量 排序後，篩選出 4500 以上的資料
peopleRef.orderByChild('weight').startAt(4500).once('value', function (snapshot) {
    snapshot.forEach(function (item) {
    console.log(item.key);
    console.log(item.val());
    })
    // console.log(snapshot.val());
})
```

``` js 多個 過濾條件
var peopleRef = firebase.database().ref('people');
// 將 重量 排序後，篩選出 2500 ~ 3500 之間的資料
peopleRef.orderByChild('weight').startAt(2500).endAt(3500).once('value', function (snapshot) {
    snapshot.forEach(function (item) {
    console.log(item.key);
    console.log(item.val());
    })
    // console.log(snapshot.val());
})
```

***
## limitToFirst、limitToLast 限制筆數

- 限制筆數：
    - limitToFirst(n) 從 <font color="red">頭</font> 取得 n 筆資料
    - limitToLast(n) 從 <font color="red">尾</font> 取得 n 筆資料

``` js 取得第 1 筆資料
var peopleRef = firebase.database().ref('people');
peopleRef.orderByChild('weight').limitToFirst(1).once('value', function (snapshot) {
    snapshot.forEach(function (item) {
        console.log(item.key);
        console.log(item.val());
    })
    // console.log(snapshot.val());
})
```

``` js 取得倒數 5 筆資料
var peopleRef = firebase.database().ref('people');
peopleRef.orderByChild('weight').limitToLast(5).once('value', function (snapshot) {
    snapshot.forEach(function (item) {
        console.log(item.key);
        console.log(item.val());
    })
    // console.log(snapshot.val());
})
```

***
## 參考來源
<div class="note info">[NodeJS 前後端開發實戰](https://www.udemy.com/teachnodejs/)
[六角學院](http://www.hexschool.com/)</div>