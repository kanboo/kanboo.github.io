---
title: Tool-gulp
date: 2017-10-09 12:56:20
categories: 
- tool
tags:
- tool
- gulp
---

# gulp環境安裝

## 安裝 Global Gulp 環境
`npm install gulp -g` 

> 如果無法安裝  Global Gulp 環境，請用以下並輸入系統密碼
> `sudo npm install gulp -g`

<!-- more -->

 ## 有沒有加入 -g 差異在哪裡呢？

- 有加入 "-g"：
這是安裝全域的套件
也就是安裝在目前的電腦上
目的是啟用 "gulp" 這個指令

- 沒有加入：
這是 local 套件安裝的指令
是裝在**目前的專案資料夾內**
目的是執行 gulp 時可以取用的套件

## 總結：
只有 npm install gulp -g  才會使用到全域的指令，在教學中， -g 也只會出現一次喔～
其餘 gulp 套件只會用到 "npm install --save"，像是

``` 
npm install gulp --save # gulp 這個套件在專案資料夾也需要載入一次喔
npm install gulp-jade --save # 其它套件都不會用到 -g
```

***

# gulp設定

1. 先建立 package.json

    `npm init`

2. 安裝 專案用的gulp環境

    `npm install gulp --save`

    > 註1：最後面加 --save 代表將安裝紀錄，記在 package.json
    > 註2：安裝完後，會產生node_modules 資料夾(裡面會有許多npm的相依套件)

3. 新增一個檔案：gulpfile.js
4. 新增一個資料夾：source，裡面再新增一個 index.html 檔案

    > 可將已設定好的 package.json 檔案，複製一份放置新專案資料夾，
    > 輸入指令：npm install ,即可安裝好相同的開發環境


```
npm install gulp --save		->給「正式環境」使用
npm install gulp --save-dev     ->給「開發環境」使用

--save-dev是你開發時候依賴的東西，--save是你發布之後還依賴的東西。

比如，你寫ES6代碼，如果你想編譯成ES5發布那麼babel就是devDependencies。
如果你用了jQuery，由於發布之後還是依賴jQuery，所以是dependencies。
```

npm install --save-dev 

const sourcemaps = require('gulp-sourcemaps');
const babel = require('gulp-babel');
const concat = require('gulp-concat');

npm install --save-dev gulp-sourcemaps gulp-babel gulp-concat