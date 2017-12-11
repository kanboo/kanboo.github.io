---
title: 2017-12-11-RWDlayout
date: 2017-12-11 13:55:46
categories: 
- CSS
tags:
- transform
- translateX
- rotate
- translateY
- SCSS
- linear-gradient
- clip-path
- position
---

<font style="font-size:18px;">紀錄練習切版時，遇到卡卡的問題</font>

<br>
{% asset_img Exercise.png %}

<!-- more -->
***

## icon 利用 position 位移


### 示意圖
{% asset_img icon_position.png %}


### 重點

1. row 的 position 設置 **_relative_**;
2. icon 的 position 設置 **_absolute_**;
3. 移動 icon 的位置， top 、 left
4. 將 icon 移進去在 input 時，input 需增加 _**左邊padding**_，保留位置給 icon。


### HTML 程式碼
``` HTML

<div class="row">
    <label for="email">Email Address*</label>
    <label for="email" class="icon"><i class="fa fa-envelope-o" aria-hidden="true"></i></label>
    <input type="email" name="email" id="email">
</div>

```

### SCSS 程式碼
``` SCSS
//記得input的左邊要留padding,保留一些位置給icon
input {
    padding: 6px 0 6px 28px;
}


//位移 icon 至 input 裡
.row {
    position: relative;

    .icon {
        position: absolute;
        top: 26px;
        left: 9px;
        font-size: 16px;
        z-index: 10;
        color: rgba(61, 17, 1, 0.5);
    }
}
```

>codepen範例: [LoginForm](https://codepen.io/Kanboo/pen/ooKBYb)


## 緞帶效果

{% asset_img ribbon.png %}


### 重點


* <font style="color:blue;">.tag-wrap 為 藍色框線</font>
* <font style="color:red;">.tag 為 紅色框線(熱賣)</font>


1. <font style="color:blue;">**_.tag-wrap_**</font> 的 position 設置 **_absolute_** ，並且位移要_**超出原有的框架**_，才能做出緞帶效果;
2. 新增 <font style="color:red;">.tag</font> 的 width、background-color...等屬性
3. 利用 **_transform_** ，位移翻轉 <font style="color:red;">.tag</font> 
4. 移動好 <font style="color:red;">.tag</font> 後，在 <font style="color:blue;">.tag-wrap</font>  新增 _**overflow: hidden;**_， 將多餘的部份隱藏
5. 利用 <font style="color:red;">.tag</font>  的偽元素 **_:before、:after_**，新增 **_小三角 ▲_** 

### html 程式碼
``` html
<div class="card">
  <!--  不影響結構  -->
  <div class="tag-wrap">
    <div class="tag">
      熱賣!!
    </div>
  </div>
  
  <!--  主要結構  -->
  <div class="card-block">
  </div>
</div>
```

### SCSS 程式碼
``` scss
.card {
  width: 230px;
  height: 200px;
  position: relative;
}

//緞帶效果
.tag-wrap {
  $p: 5px;
  $size: 80px;
  position: absolute;
  //位移要超出原有的框架，才能做出緞帶效果
  top: -$p;
  right: -$p;
  //超出的部份隱藏
  overflow: hidden;
  width: $size * 2;
  height: $size * 2;
  //查看位置
  border: 1px solid blue;

  .tag {
    position: relative;
    width: $size*2;
    background-color: orange;
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
    text-align: center;
    margin-top: -$p * 2;
    color: white;
    //rotate為旋轉、translate為移動、skew為傾斜、scale為縮放
    transform: translateX(50%) rotate(45deg) translateY(150%);
     //查看位置
    border: 1px solid red;

    &:before {
      content: "";
      position: absolute;
      //利用 加粗邊框 + 邊框上方顏色，達到小三角的效果
      border: $p solid transparent;
      border-top-color: darken(orange, 15%);
      bottom: -10px;
      left: 6px;
    }

    &:after {
      content: "";
      position: absolute;
      //利用 加粗邊框 + 邊框上方顏色，達到小三角的效果
      border: $p solid transparent;
      border-top-color: darken(orange, 15%);
      bottom: -10px;
      right: 14px;
    }
  }
}
```


>codepen範例: [CSS-緞帶效果(乾淨版本)](https://codepen.io/Kanboo/pen/eyOgBP)
>codepen範例: [CSS-緞帶效果](https://codepen.io/Kanboo/pen/ZagBev)

***

## 觀念補充

### 六角範例

[從 Sketch 設計到 CSS 切版](https://www.youtube.com/watch?v=ev9bGi_XCqU)

[本日範例：https://codepen.io/Wcc723/pen/zEYXdN](https://codepen.io/Wcc723/pen/zEYXdN)

[本日設計參考：https://codepen.io/Wcc723/pen/pWzxXO](https://codepen.io/Wcc723/pen/pWzxXO)

### transform

{% asset_img transform.png %}


### CSS 程式碼
``` css
/* 
rotate為旋轉
skew為傾斜
scale為縮放
translate為移動 
*/

/* 範例1：移動X軸 → 旋轉 → 移動Y軸 */
transform: translateX(50%) rotate(45deg) translateY(150%);

/* 範例2：旋轉 → 移動X軸 → 移動Y軸 */
transform: rotate(45deg) translateX(50%)  translateY(150%);

```

上列 CSS程式碼 「_**範例1 、 範例2**_」，
雖然同樣都做了「移動X軸、移動Y軸、旋轉」的動作，
但是因為 **_執行順序_** 不同，
所以二個範例呈現出來的結果也會**不一樣**。

>[transform基本介紹](https://www.great-good.tw/learn/css-transform/)
>[1.CSS transform 概觀](https://wcc723.github.io/css/2013/10/08/css-transform/)
>[2.CSS transform 軸線的謊言](https://wcc723.github.io/css/2013/10/09/css-transform-mistake/)
>[3.CSS transform-origin](https://wcc723.github.io/css/2013/10/10/css-transform-origin/)
>[4.CSS transform-3D的透視](https://wcc723.github.io/css/2013/10/11/css-perspective/)


### SCSS顏色函數範例

``` scss
.box{
    background: rgba(#000,.5); //變半透明
    background: invert(#f00); //變反向色彩
    background: lighten(#06C, 30%); //變亮
    background: darken(#06C,15%); //變暗
    background: saturate(#06C,50%); //提高飽和度
    background: desaturate(#06C,50%); //降低飽和度
    background: grayscale(#06C); //灰階
}
```

>[SCSS相關](https://blog.user.today/scss/)

### linear-gradient讓顏色有漸層效果

{% asset_img deg.png %}

### CSS 程式碼
``` css
background-image: linear-gradient(165deg, white, white 50%, $bg-color 50%);
```

background: linear-gradient( 方向, 第一個顏色, 第二個顏色, ... );

>[背景色 — 线性渐变](http://www.jianshu.com/p/7a2eb94c91da)
>[背景色 — 漸層效果](https://medium.com/@savemuse/%E6%87%89%E7%94%A8css3-gradients%E8%A3%BD%E4%BD%9C%E6%BC%B8%E5%B1%A4%E6%95%88%E6%9E%9C-490cf0efd634)


### 繪製幾何圖形(支援度不高)

{% asset_img clip.png %}

``` css
clip-path: polygon(0 0, 280px 0, 370px 100%, 0% 100%);
```
>[CSS clip-path 生成器](http://www.css88.com/tool/css-clip-path)
>[利用CSS繪製更多形狀](https://www.webdesigns.com.tw/css_clip-path.asp)
>[不可思议的CSS之CLIP-PATH](https://juejin.im/entry/59a6dab4f265da24976003d3)
