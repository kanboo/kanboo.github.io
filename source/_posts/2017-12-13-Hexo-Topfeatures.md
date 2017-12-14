---
title: Hexo-文章置頂
date: 2017-12-13 09:29:37
categories: 
- Hexo
tags:
- Hexo
- Top
- 置頂
---

## 前言

解決Hexo置頂問題，需要完成的事情有：

1. 將目前 node_modules/hexo-generator-index/lib/generator.js 程式碼替換
2. 在需要置頂的文章的前事中添加頂值，值越大越置頂。

<!-- more -->

***

## 操作步骤

1. 替換 generator.js 程式碼

    ``` js
    'use strict';
    var pagination = require('hexo-pagination');
    module.exports = function(locals) {
        var config = this.config;
        var posts = locals.posts;
        posts.data = posts.data.sort(function(a, b) {
            if (a.top && b.top) { // 兩篇文章top都有定義
                if (a.top == b.top) return b.date - a.date; // 若top值一样則按照文章日期降序排
                else return b.top - a.top; // 否則按照top值降序排
            } else if (a.top && !b.top) { // 以下是只有一篇文章top有定義，那麼将有top的排在前面
                return -1;
            } else if (!a.top && b.top) {
                return 1;
            } else return b.date - a.date; // 都沒定義按照文章日期降序排
        });
        var paginationDir = config.pagination_dir || 'page';
        return pagination('', posts, {
            perPage: config.index_generator.per_page,
            layout: ['index', 'archive'],
            format: paginationDir + '/%d/',
            data: {
                __index: true
            }
        });
    };

    ```

    >若找不到 **node_modules/hexo-generator-index/lib/generator.js** 的話，
    >請先安裝 `hexo-generator-index`。
    >
    >指令： `npm i --save hexo-generator-index`

2. 在需要置顶的文章的中添加 top 值，值越大越置顶。

    ``` zsh
    title: Hexo-文章置頂
    date: 2017-12-13 09:29:37
    top: 100
    ```

***

## 參考資料

[hexo置顶功能](http://spxiaomin.github.io/2017/02/12/hexo%E7%BD%AE%E9%A1%B6%E5%8A%9F%E8%83%BD/)
[Hexo文章置顶](http://hongyitong.github.io/2017/01/06/Hexo%E6%96%87%E7%AB%A0%E7%BD%AE%E9%A1%B6/)