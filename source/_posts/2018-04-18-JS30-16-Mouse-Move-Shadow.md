---
title: JS30-16-Mouse-Move-Shadow
date: 2018-04-18 16:22:51
categories:
- JS
- JS30
tags:
- JS
- JS30
---

{% cq %}

{% asset_img js16.png %}

<font style="font-size:20px;">滑鼠移動時，讓<font color="red">文字的陰影</font>跟著移動。</font>

{% endcq %}

<!-- more -->
***

## 目標

- 滑鼠移動時，讓<font color="red">文字的陰影</font>跟著移動。


## 實踐步驟

1. 取得取得文字區域的元件

2. 監聽 `mousemove`
  - 取得 hero 的 寬、高
  - 取得滑鼠的座標
  - 計算位置與比例，修改textShadow屬性值


## 成品

>[[DEMO]](https://kanboo.github.io/JavaScript30/16%20-%20Mouse%20Move%20Shadow/) | [[GitHub]](https://github.com/kanboo/JavaScript30/blob/master/16%20-%20Mouse%20Move%20Shadow/index.html)


***
## JS學習紀錄

此次重點在於

1. 座標點的取得以及計算
2. 解構賦值的運用

``` html HTML
<div class="hero">
  <h1 contenteditable>🇹🇼 Taiwan!</h1>
</div>
```

``` js 全部js程式碼
const hero = document.querySelector('.hero');
const text = hero.querySelector('h1');
const walk = 100; //設定 text-shadow 座標最大的偏移範圍

function shadow(e){
  // console.dir(hero);

  // 取得 hero 的 寬、高
  // const width = hero.offsetWidth;
  // const height = hero.offsetHeight;
  /* 可簡寫如下 */
  const {offsetWidth: width, offsetHeight: height} = hero;

  // 取得滑鼠的座標
  // offsetX與offsetY回傳的座標，是以「目前DOM box model區塊範圍」為主，
  // 回傳滑鼠座標位於「目前的DOM區塊範圍」的哪裡，
  // 而不是以整個「window」為主，另外DOM與DOM重疊的話，依舊是分開計算。
  // 註：起點為左上角： x:0 , y:0 ，向右增加 x ，向下增加 y
  let {offsetX: x, offsetY: y} = e;
  // console.log(x, y);

  // 若滑鼠從父元素移到子元素的話，offsetX與offsetY會 歸0 重新計算，
  // 所以需要將父元素與子元素之間座標的落差補足。
  if (this !== e.target){
    // console.dir(e.target);
    x = x + e.target.offsetLeft; // 目前DOM之滑鼠的X座標 + 目前DOM位於父元素的X座標
    y = y + e.target.offsetTop; // 目前DOM之滑鼠的Y座標 + 目前DOM位於父元素的Y座標
  }
  // console.log(x, y);

  // (座標在hero的比例 * 最大偏移量的值) - (一半的最大偏移量的值)
  // 取得 正值或負值 的座標，如： 最大偏移量的值=100，取得範圍落於 -50~50 之間
  const xWalk = Math.round((x / width * walk) - (walk / 2));
  const yWalk = Math.round((y / height * walk) - (walk / 2));


  text.style.textShadow = `
    ${xWalk}px ${yWalk * -0.7}px 0 rgba(255,0,0,0.3),
    ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,0,0.3),
    ${yWalk}px ${xWalk * -0.5}px 0 rgba(0,255,255,0.3),
    ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.3)
  `;

}

// 監聽滑鼠事件
hero.addEventListener('mousemove', shadow);
```

### 解構賦值(Destructuring assignment)

從上面程式碼，將使用「解構賦值」的地方，額外拉出來

``` js 解構賦值
// 取得 hero 的 寬、高
const width = hero.offsetWidth;
const height = hero.offsetHeight;
// 可簡寫如下
const {offsetWidth: width, offsetHeight: height} = hero;
```

``` js 解構賦值
let x = e.offsetX;
let y = e.offsetY;
// 可簡寫如下
let {offsetX: x, offsetY: y} = e;
```