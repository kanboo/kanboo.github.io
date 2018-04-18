---
title: JS30-15-LocalStorage
date: 2018-04-18 15:06:00
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js15.jpg %}

<font style="font-size:20px;">使用 <font color="red">LocalStorage</font> 做 todolist。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 使用 <font color="red">LocalStorage</font> 做 todolist。


## 實踐步驟

1. input輸入框
  - 取得輸入項目，儲存List清單的項目
  - 刷新html的List清單
  - 儲存至localStorage → 只接受字串

2. UL的List清單
  - 監聽是否有done
  - 更新List清單的項目

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/15%20-%20LocalStorage/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/15%20-%20LocalStorage/index.html)


***
## JS學習紀錄

此次練習學到的東西都在一些小技巧上面，故一步步將撰寫的步驟寫上，並註記哪裡在特別注意。

### 一、取得輸入項目，並新增至LocalStorage

``` js 預設元件
const addItems = document.querySelector('.add-items');  //送出按鈕
const itemsList = document.querySelector('.plates');    //UL清單
const items = [];   //儲存List清單的項目
```

``` js 監聽 送出按鈕
addItems.addEventListener('submit', addItem);
```

``` js 新增input項目
function addItem(e){
  // 取消原本的預設動作
  e.preventDefault();

  /* 因為點擊的元素 = addItems，所以可以直接用 this 進行後續的動作 */
  // const item = addItems.querySelector('[name="item"]');
  const text = (this.querySelector('[name=item]')).value;

  /* 產生新物件，紀錄輸入的文字與是否勾選的狀態(done) */
  const obj = {
    // text: item.value,  //es5寫法
    text,  //使用es6的解構賦值:text:text
    done: false
  }

  // 新增List清單的項目
  items.push(obj);

  // 刷新html的List清單
  populateList(items, itemsList);

  // 額外將資料存在localStorage
  localStorage.setItem('items', JSON.stringify(items)); // localStorage 只接受字串

  // item.value = '';
  this.reset(); // reset也可清空表單
}
```

``` js 刷新html的List清單
/* 在組HTML時，新增一些資訊在dataset上，以供後續的操作動作，可取得相關資訊。 */
function populateList(plates = [], platesList) {
  platesList.innerHTML = plates.map((plate, index) => {
    return `
      <li>
        <input type="checkbox" data-index=${index} id="item${index}" ${plate.done ? 'checked' : ''} />
        <label for="item${index}">${plate.text}</label>
      </li>
    `;
  }).join('');
}
```

### 二、List清單的預設值

因為運用到localStorage儲存資料，所以重新進入此頁面時，可以利用localStorage取得最後一次的紀錄。

``` js 預設元件
const addItems = document.querySelector('.add-items');  //送出按鈕
const itemsList = document.querySelector('.plates');    //UL清單
//const items = [];   //儲存List清單的項目

const items = JSON.parse(localStorage.getItem('items')) || [];
```


### 三、監聽並儲存checkbox狀態

``` js 監聽List的項目
itemsList.addEventListener('click', toggleDone);
```

``` js
function toggleDone(e) {
  // 判斷是 input:checkbox 才執行
  if (!e.target.matches('input')) return;

  // 取得checkbox的data-index值
  const el = e.target;
  const index = el.dataset.index;

  /* 以往會用 if else 去判斷，可用 「!」來反轉上次的結果。 */
  // 切換是否已done的flag
  items[index].done = !items[index].done;

  // 更新localStorage
  localStorage.setItem('items', JSON.stringify(items));

  // 刷新html的List清單
  populateList(items, itemsList);
}
```