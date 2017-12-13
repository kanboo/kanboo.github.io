---
title: Hexo-留言版
date: 2017-12-13 10:02:33
categories: 
- Hexo
tags:
- Hexo
- Comment
- Disqus
- 留言版
- 評論
---

## 前言

{% asset_img Disqus.png %}

新增 Hexo 的留言版，需要完成的事情有：

1. 註冊 Disqus 的帳號
2. 新建 Disqus 的 website
3. 配置 Hexo


<!-- more -->

***

## 操作步骤

### <span id="inline-toc">1.</span>註冊 Disqus 的帳號


Disqus網址：[https://disqus.com/](https://disqus.com/)

打開鏈接後， 可以直接用Facebook，Twitter以及Google 登錄，也可以用郵箱註冊後登錄。

### <span id="inline-toc">2.</span>建立 Disqus website

1. 點擊 **GET STARTED** 開始建立 website

    {% asset_img Disqus_01.png %}

2. 點擊下面 **I want to install Disqus on my site**

    {% asset_img Disqus_02.png %}

3. 填寫 **Website Name**，這是 **短名稱**，用於和 Hexo連結的 **Key值**。

    {% asset_img Disqus_03.png %}

4. 直接點擊 Configure Disqus

    {% asset_img Disqus_04.png %}

5. 填寫 **Website URL** ，這是你 **Blog的網址**

    {% asset_img Disqus_05.png %}

6. 建立完成畫面

    {% asset_img Disqus_06.png %}

### <span id="inline-toc">3.</span>配置 Hexo

**主題配置主題** 下面的 **config.yml** 文件，路徑： ~blog/themes/next/_config.yml

1. 將 Disqus下的 **enable** 設定為 **true** 。
2. 同時填寫您的 **shortname(短名稱)**。
3. count 用於指定是否顯示評論數量。

``` zsh
# Disqus
disqus:
  enable: true
  shortname: kanbooBlog
  count: true
```

上述步驟設置完成後，更新Blog

``` zsh
$ hexo clean
$ hexo g
$ hexo d
```

***

## 參考資料

[Hexo 集成Disqus 評論](http://www.cylong.com/blog/2017/03/26/hexo-next-disqus/)
[Hexo搭建博客系列：（六）Hexo添加Disqus評論](http://www.jianshu.com/p/d68de067ea74)
[Next-评论系统](http://theme-next.iissnan.com/third-party-services.html#comment-system)