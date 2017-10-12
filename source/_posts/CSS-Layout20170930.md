---
title: CSS - 排版重點整理
date: 2017-09-30 12:09:28
categories: 
- CSS
tags:
- CSS
- 排版
- Layout
---

## [display屬性]
每一個html標籤都有一個預設的display屬性，通常是block(區塊元素)或者是inline(行內元素)。
* block：會從新的一行開始且在占據網頁的一整行，具自己個寬高。
* inline：無法設定寬高，依照包含的內容先是寬度。
* inline-block：可設定寬高，但又不占據一整行。
* none：不顯示，可用來跟使用者互動。

<!-- more -->

## [定位]
* position，設定區塊的座標方式。如下[position]
* left/top/right/bottom，設定區塊的左/上/右/下的座標。
* z-index，設定區塊的重疊時的顯示優先權。
* overflow，當內容超出區塊範圍時候的顯示方式。

## [position]
* static:預設，各元素不用設定即是static，遵守網頁規則由左至右，由上至下。
* relative:依照原來存在位置進行調整。absolute:會找上一層(找不到再往上找)有誰設定過
* position，依照這個父元素進行位置調整，如果都沒有那就依body進行調整。
* fixed:依目前看得網頁進行定位。若是要對齊容器內（div）的對位置，採用absolute比較方便。不用特別計算跟父容器的距離。

## [float]
設定區塊間的流動方式，像是doc當中的文繞圖。
* left:往網頁左邊流動。
* right:往網頁右邊流動。
* clear:both ：清除流動關係。
 
## [區塊]
* width，設定區塊寬度。width:1080px;
* height，設定區塊高度。height:500px;
* float，設定區塊流動方向。float:left;
* clear，解除區塊流動。
* margin，設定區塊外間距離。如下[margin]。
* padding，設定區塊內距。同margin。

## [margin設定]
* margin:10px 20px 30px 40px;  依序為上 右 下 左
* margin:10xp 20px 30px;    上 右 下 ?-->找對面的來補,所以是找右的設定來補
* margin:10px 20px; 依序為上下 左右
* margin:10px;  全都一樣

## [背景]
* background-color，背景顏色
* background-image，背景圖案
* background-attachment，背景是否固定不動
* background-repeat，背景是否重複
* background-position，背景位置


## [邊框]
* border-color，四邊的顏色
* border-style，四邊的樣式
* border-width，四邊的寬度
* border-top-color，上邊框的顏色，top可以置換成為right、bottom、left。
* border-top-style，上邊框的樣式，top可以置換成為right、bottom、left。
* border-top-width，上邊框的寬度，top可以置換成為right、bottom、left。
* border-top，上邊框的顏色、樣式與寬度。top可以置換成為right、bottom、left。
* border，寬度、樣式、顏色。

## [文字]
* font-family，設定字型
* font-size，字體大小
* color，字體顏色
* line-height，文字行高
* font-weight，文字粗體
* text-decoration，文字底線
* word-spacing，間距
* letter-spacing，間距
* text-aling，水平對齊方向
* text-indent，字首縮排

