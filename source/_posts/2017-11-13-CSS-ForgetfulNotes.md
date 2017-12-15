---
title: CSS - 常用筆記
date: 2017-11-13 11:50:28
categories: 
- CSS
tags:
- CSS
- 筆記
#top: 100000
---

## 背景圖

``` css
/* 圖片滿版 */
.img-cover {
    background-image: url(./bg.png);
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
}

/* 圖片居中 */
.img-contain {
    background-image: url(./bg.png);
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
}

```

> 其他背景圖範例：[固定、滿版、置中](https://codepen.io/Kanboo/pen/boKpwG)
> background-attachment是背景固定模式的屬性
註：作为背景的圖片是撑不起元素的，須額外需要给div新增「寬、高」

<!-- more -->

## 圓角

``` css
div.circle{
    width:80px;
    height:80px;
    border-radius:50%;
    background-color:blue;
}
```
+ 2個重點：
 - 圓的直徑，長寬 一定要等長
 - border-radius:50%

>參考網址： [圓角詳解](http://www.jianshu.com/p/7f46b8e29b1b)、[CSS3技巧之形狀](https://kknews.cc/zh-tw/news/aezgo8v.html)

## 區塊陰影、文字立體感

``` CSS
/* 外層的陰影 */
box-shadow: 1px 1px 5px rgba(0,0,0,0.3);

/* 內層的陰影(多加 inset) */
box-shadow: inset 1px 1px 5px rgba(0,0,0,0.3);

/* 文字的陰影 */
text-shadow: 1px 1px 1px rgba(0,0,0,0.5);
```

>參考網址： [玩转box-shadow](http://www.jianshu.com/p/18bdcd17b4f2)、[文字立體感](http://www.jianshu.com/p/34d8dcb75dd8)


## a 連結
### 取消 a 連結下底線

``` CSS
a {
 text-decoration: none;
}
```

### :hover 移至 a 連結的效果

``` CSS
a:hover {
 border-bottom: 3px solid #00cc99;
}
```

>註：a 連結 預設display為inline，但為了讓 User有更好的操作體驗，
會變更成 block，增加「寬、高」範圍，讓 User較易點擊到目標連結。

## 斷點

``` CSS
@media (max-width: 768px) {
 //iPad
}
@media (max-width: 767px) {
 //iPhone
}
```

## 新單位：高度vh、寬度vw

**vh** 代表的是view height，也就是螢幕可視範圍高度的百分比；
**vw** 表示的是view width，也就是螢幕可是範圍寬度的百分比。

## ul ol list-style

### 取消樣式
`ol,ul {list-style: none;}`

### 設定樣式
`list-style: circle;`

>其他樣式參考：[CSS list-style](http://www.w3school.com.cn/cssref/pr_list-style-type.asp)

## 指定滑鼠游標的型態
`cursor: pointer; /*手型，表示超連結*/`

{% asset_img cursor.png %}

>參考網址： [游標的型態](http://www.eion.com.tw/Blogger/?Pid=1117)

