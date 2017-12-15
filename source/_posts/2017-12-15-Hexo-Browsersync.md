---
title: Hexo-Browsersync
date: 2017-12-15 14:03:57
categories: 
- Hexo
tags:
- Hexo
- Browsersync
- 頁面自動刷新
---

{% cq %}

實現撰寫文章時，可自動刷新頁面預覽，不用再手動F5了。

{% endcq %}

<!-- more -->

## 前言

實現刷新功能，需要完成的事情有：

1. 安裝套件 Hexo-Browsersync
2. 解決文章太長-渲染失敗Bug
3. 執行 hexo server，即可。

## 操作步骤

### <span id="inline-toc">1.</span>安裝套件

``` zsh
$ npm install hexo-browsersync --save
```

### <span id="inline-toc">2.</span>修正Bug

1. 安裝 hexo-renderer-jade 插件

    ``` zsh
    $ npm install hexo-renderer-jade --save
    ```

2. 在你的 `node_modules` 文件夾裡找到 `hexo-renderer-pug` 的文件夾，然後將裡面 `lib/renderer.js` 的其中一行代碼 <span id="inline-yellow">註解</span> 掉：

    ``` zsh
    //pugRenderer.compile = pugCompile;
    ```

3. 新增 `_config.yml` 的設定。 參考：[issue](https://github.com/hexojs/hexo-server/issues/23)

    ``` zsh 新增路徑：~/blog/_config.yml
    server: 
        compress:  true  #開啟壓縮
    ```

### <span id="inline-toc">3.</span>執行

``` zhs
$ hexo s
```

啟動 <span id="inline-blue">hexo server</span>  後，你會看到下列的訊息

``` zhs
INFO  Start processing
[Browsersync] Access URLs:
 --------------------------------------
          UI: http://localhost:3001
 --------------------------------------
 UI External: http://192.168.1.135:3001
 --------------------------------------
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

port:3001 可修改 browsersync 相關設定。
port:4000 Blog的預覽畫面。

***

## 參考文章

<div class="note info">[Hexo 实现实时预览编辑](http://blog.mutoe.com/2016/hexo-post-livereload-edit/)
[Hexo不重新生成也可预览](https://jerry011235.github.io/2015/05/07/Hexo%E4%B8%8D%E9%87%8D%E6%96%B0%E7%94%9F%E6%88%90%E4%B9%9F%E5%8F%AF%E9%A2%84%E8%A7%88/)

[修正渲染不全問題](https://molunerfinn.com/make-a-hexo-theme/#搭建书写主题的舒适环境)
</div>