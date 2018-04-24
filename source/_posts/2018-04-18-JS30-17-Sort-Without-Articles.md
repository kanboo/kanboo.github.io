---
title: JS30-17-Sort-Without-Articles
date: 2018-04-18 16:54:56
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js17.jpg %}

<font style="font-size:20px;">將陣列中的資料，去除冠詞(Articles)後，再進行排序。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 將陣列中的資料，去除冠詞(Articles)後，再進行排序。


## 實踐步驟

1. 取得陣列
  - 去除 冠詞 的字眼
  - sort的寫法

## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/17%20-%20Sort%20Without%20Articles/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/17%20-%20Sort%20Without%20Articles/index.html)


***
## JS學習紀錄

此次重點在於

1. 運用「正規表達式」
2. 如何精進Sort的寫法

``` js 整段程式碼
const bands = ['The Plot in You', 'The Devil Wears Prada', 'Pierce the Veil',
'Norma Jean', 'The Bled', 'Say Anything', 'The Midway State',
'We Came as Romans', 'Counterparts', 'Oh, Sleeper', 'A Skylit Drive',
'Anywhere But Here', 'An Old Dog'];

// 去除 冠詞 的字眼
// 使用replace搭配正規表示式來將包含了a, the, an開頭的文字替換為空白。
function strip(bandName){
  // ^：比對輸入列的啟始位置
  // i：Case-insensitive search
  return bandName.replace(/^(a |the |an )/i, '').trim();
}

// sort寫法3：利用 箭頭函數 與 三元運算式的簡寫
const sortBands = bands.sort((a, b) => strip(a) > strip(b) ? 1 : -1);

// 寫入HTML
document.querySelector('#bands').innerHTML =
  sortBands
    .map( band => `<li>${band}</li>`)
    .join('');
```

<span id="inline-yellow">sort寫法簡化</span>

從上面程式碼，將「sort程式碼」簡化過程，呈現在下方，做個紀錄。

``` js 簡化過程
// sort寫法1：
const sortBands = bands.sort((a, b) =>{
  if (strip(a) > strip(b)){
    return 1;
  }else{
    return -1;
  }
})

// sort寫法2：利用 三元運算式的簡寫
const sortBands = bands.sort((a, b) =>{
  return strip(a) > strip(b) ? 1 : -1
})

// sort寫法3：利用 箭頭函數 與 三元運算式的簡寫
const sortBands = bands.sort((a, b) => strip(a) > strip(b) ? 1 : -1);
```