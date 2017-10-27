---
title: Hexo-Search
date: 2017-10-18 13:53:37
categories: 
- Hexo
tags:
- Hexo
- Search
---


# 前言

在新增 Hexo 的 Local Search 時，參考下列的連結文章，跟著步驟安裝及設定即可，
不過剛裝好後，發現搜尋功能無法正常使用，後來有查到原因，在此紀錄一下Debug過程。


[Local Search](http://theme-next.iissnan.com/third-party-services.html#local-search)
[Hexo博客添加站內搜索](https://www.ezlippi.com/blog/2017/02/hexo-search.html)

<!-- more -->

# 安裝順序

## 安裝hexo-generator-searchdb

在站點的根目錄下執行以下命令

``` zsh
$ npm install hexo-generator-searchdb --save
```

## 編輯站點配置文件(Hexo)

新增以下內容到任意位置：

``` zsh
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

## 編輯主題配置文件(Next)

啟用本地搜索功能：

``` zsh
# Local search
local_search:
  enable: true
```

***
# 問題紀錄

1. 點了搜尋後，畫面一直轉圈圈沒停止，查看Console也沒出現Error
{% asset_img log01.jpg %}

2. 查看XHR，目前我有10篇文章，卻只產生三筆entry，且其中一筆entry的content內容沒產生
{% asset_img log02.jpg %}

3. 並且有此錯誤訊息
{% asset_img log03.jpg %}

4. 後來針對有問題那篇文章複製內容，貼到 notepad++ 看看，發現有一亂碼，將亂碼刪除後，Local Search就可以正常運作了。
{% asset_img log04.jpg %}