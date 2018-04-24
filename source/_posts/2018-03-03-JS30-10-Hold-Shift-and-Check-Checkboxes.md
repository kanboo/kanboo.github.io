---
title: JS30-10-Hold-Shift-and-Check-Checkboxes
date: 2018-03-03 00:33:52
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img checklist.png %}

<font style="font-size:20px;">搭配 <font color="red">Shift + 滑鼠</font> 勾選多個Checkbox</font>

{% endcq %}

<!-- more -->
***

## 目標

- 利用 <font color="red">Shift + 滑鼠點擊</font> 完成勾選範圍的項目

## 實踐步驟

1. 取得所有的 checkbox元素，並且監聽 click 事件

2. 利用變數紀錄 <font color="red">前次 與 這次</font> 的點擊項目，計算出二者之間的範圍，完成勾選範圍的項目
    - lastChooise 記錄最後選擇項目
    - e.shiftKey 判斷有無按Shift鍵

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/10%20-%20Hold%20Shift%20and%20Check%20Checkboxes/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/10%20-%20Hold%20Shift%20and%20Check%20Checkboxes/index.html)


***
## JS學習紀錄

### 思考：如何篩選出範圍

<font color="blue">想法1</font>(我的想法)：

1. 紀錄 最後一次的點擊項目
2. 判斷 目前點擊項目 是在 最後一次點擊項目 的上面或下面
    1. 先爬上一次
    2. 再爬下一次
3. 知道方向後，再由 目前點擊項目 loop至 最後一次點擊項目，完成勾選範圍的項目

<font color="blue">想法2</font>(作者作法)：


1. 紀錄 最後一次的點擊項目
2. 重新到尾loop 一次
    1. 將第一個遇到的「currentChooise 或 lastChooise」當起頭
    2. 最後結束的時機點，就利用剩下還沒遇到的 「currentChooise 或 lastChooise」當結尾，
    3. 由此劃分出 currentChooise 與 lastChooise 之間的範圍

由上述的 <font color="blue">想法1</font> 和 <font color="blue">想法2</font> 來比較的話，<font color="blue">想法1</font> 在過程中就多跑了好幾次loop來判斷一些事項，
才能達到結果，所以在此次練習中，又學到了新的思維。

<span id="inline-purple">JS程式碼</span>

因為程式碼不多，就直接貼上來，方便看。

``` js 整段程式碼
const checkboxs = document.querySelectorAll('[type="checkbox"]');
let lastChooise = null; // 紀錄最後選擇的Check元素

function handleCheck(e){

  let isInChooiseScope = false;

  // 有按shift鍵 & 有勾選Check & 不是最後選擇的Ckeck元素 & 不是第一次選擇Check
  if (e.shiftKey === true && this.checked === true && lastChooise !== this && lastChooise !== null){

    checkboxs.forEach( checkbox => {
        // 重點：
        // 利用loop從頭到尾迭代的特性，將第一個遇到的「currentChooise 或 lastChooise」當起頭
        // 最後結束的時機點，就利用剩下還沒遇到的 「currentChooise 或 lastChooise」當結尾，
        // 由此劃分出 currentChooise 與 lastChooise 之間的範圍
        if ( checkbox === this || checkbox === lastChooise){
          isInChooiseScope = !isInChooiseScope;
        }

        if (isInChooiseScope){
          // checkbox.setAttribute('checked', true);
          // 上下二者結果皆相同
          checkbox.checked = true;
        }
    })
  }

  lastChooise = this;
}

checkboxs.forEach( checkbox => checkbox.addEventListener('click', handleCheck))
```