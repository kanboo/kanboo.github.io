---
title: CSS-IcoMoon
date: 2017-12-23 10:25:42
categories: 
- CSS
tags:
- CSS
- icon
- sass
---

{% cq %}

{% asset_img icomoon_01.png %}

<font style="color:#f90;font-size:28px;">[IcoMoon](https://icomoon.io/)</font>

{% endcq %}

<!-- more -->
***

## 下載icons

1. 挑選好想要的icons

    {% asset_img icomoon_02.png %}

2. 修改部份設定

 - 產生sass的css檔

 - 修改引用的html語法
 預設語法為`<span class="icon-book"></span>`，修改為`<i class="icon-book"></i>`

 {% asset_img icomoon_03.png %}

3. 下載字體

 {% asset_img icomoon_04.png %}

## 引入專案

1. 將下載好檔案解壓縮，複製 `fonts資料夾` 和 `style.scss、variables.scss` 

 {% asset_img icomoon_05.png %}

2. 複製到專案的source資料夾底下

 {% asset_img icomoon_06.png %}

3. 修改sass的檔名和內容

 - 修改variables.scss內容，並更改檔名為 _icomoon_variables.scss

 ``` diff
    //第一行
 -  $icomoon-font-path: "fonts" !default;
 +  $icomoon-font-path: "../fonts" !default; //fonts資料夾不是在css資料夾底下，所以需回上一層
 ```

 - 修改style.scss內容，並更改檔名為 _icomoon.scss

 ``` diff 
    //第一行
 -  @import "variables";
 +  @import "_icomoon_variables";
 ```

4. 主頁 all.css 引入 _icomoon.scss

 ``` diff
    @import "bootstrap";
    @import "font-awesome";
 +  @import "icomoon";
 ```

5. 使用icons語法

 ``` scss
 <i class="icon-music"></i>
 <i class="icon-mic"></i>
 <i class="icon-coin-dollar"></i>
 ```
 {% asset_img icomoon_07.png %}


