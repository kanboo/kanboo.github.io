---
title: JS30-07-Array-Cardio-Day-2
date: 2018-02-26 11:20:09
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img arraymethod.jpg %}

<font style="font-size:20px;">練習運用 <font color="red">Array</font> 的各種函式</font>

{% endcq %}

<!-- more -->
***

## 目標

- 共提供二組資料
    - people： [{ name: 'Wes', year: 1988 },...]
    - comments ：[{ text: 'Love this!', id: 523423 },...]
- 根據不同需求條件篩選出正確的資料


## 練習題目

people 的資料：

1. 在people資料中，是否有19歲以上的人
2. 在people資料中，是否每個人都19歲以上

comments 的資料：

3. 在comments資料中，找到id是 823423 的資料
4. 在comments資料中，找到id是 823423 的資料索引值, 並透過索引值刪除這筆資料

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/07%20-%20Array%20Cardio%20Day%202/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/07%20-%20Array%20Cardio%20Day%202/index.html)

***
## people 題目

``` js people資料
const people = [
    { name: 'Wes', year: 1988 },
    { name: 'Kait', year: 1986 },
    { name: 'Irv', year: 1970 },
    { name: 'Lux', year: 2015 }
];
```

<span id="inline-toc">Q：</span>在people資料中，是否有19歲以上的人

<span id="inline-toc">A：</span>透過 `some()` 逐筆判斷，只要其中一筆有<font color="red">符合</font>，就回傳 `true`

``` js some()
// Some and Every Checks
// Array.prototype.some() // is at least one person 19 or older?

/* 解法 */
const isAdult = people.some(function(people){
  const currYear = (new Date()).getFullYear();
  if ( (currYear - people.year) >= 19){
    return true
  }
})

/* 簡化語法 */
const isAdult = people.some( people => (new Date()).getFullYear() - people.year >= 19)

console.log(isAdult);


```

<span id="inline-toc">Q：</span>在在people資料中，是否每個人都19歲以上

<span id="inline-toc">A：</span>透過 `every()` 逐筆判斷，只要其中一筆<font color="red">不符合</font>，就回傳 `false`

``` js every()
// Array.prototype.every() // is everyone 19 or older?
const allAdults = people.every( people => (new Date()).getFullYear() - people.year >= 19)

console.log(allAdults);
```

## comments 題目

``` js comments資料
const comments = [
    { text: 'Love this!', id: 523423 },
    { text: 'Super good', id: 823423 },
    { text: 'You are the best', id: 2039842 },
    { text: 'Ramen is my fav food ever', id: 123523 },
    { text: 'Nice Nice Nice!', id: 542328 }
];
```

<span id="inline-toc">Q：</span>在comments資料中，找到id是 823423 的資料

<span id="inline-toc">A：</span>透過 `find()` 逐筆判斷，回傳 第一個符合條件的<font color="red">值</font>，若都無符合，則回傳 `undefined`

``` js find()
// Array.prototype.find()
// Find is like filter, but instead returns just the one you are looking for
// find the comment with the ID of 823423

/* 解法 */
const comment = comments.find(function(comment){
  if (comment.id === 823423){
    return true
  }
})


/* 簡化語法 */
const comment = comments.find( comment => comment.id === 823423)

console.log(comment);
```


<span id="inline-toc">Q：</span>在comments資料中，找到id是 823423 的資料索引值, 並透過索引值刪除這筆資料

<span id="inline-toc">A：</span>
1. 透過 `findIndex()` 逐筆判斷，回傳 第一個符合條件的<font color="red">索引值</font>，若都無符合，則回傳 `-1`
2. 將取得的索引值，透過 `slice()` 和 `Spread syntax(展開語法)` 的搭配使用，產出一新陣列

``` js findIndex()、slice()、Spread syntax
// Array.prototype.findIndex()
// Find the comment with this ID
// delete the comment with the ID of 823423

const index  = comments.findIndex( comment => comment.id === 823423)
// console.log(index);

// 去除不要的comment，產生一個新的陣列
// slice 為 淺拷貝（shallow copy），不影響原陣列資料。
const newComments = [
    ...comments.slice(0, index),
    ...comments.slice(index+1)
]

console.log(newComments);
```