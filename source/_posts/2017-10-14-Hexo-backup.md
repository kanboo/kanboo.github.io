---
title: Hexo-備份
date: 2017-10-14 14:14:39
categories: 
- Hexo
tags:
- Hexo
- 備份
---

# 前言

簡單來說，實現備份方法就是利用 `二個分支線` 個別放我們要的檔案，
一個分支線 放 發佈的文章，另一個分支線 放 備份的檔案，

* master: 利用 hexo deploy 直接推送，存放由 hexo 產生的靜態頁面
* hexo: 利用 git command 推送，存放部落格原始碼

> 由於 Github Page 本身限制頁面主要分支必須為 master，
> 因此考慮增設分支 hexo 用以存放部落格原始碼。

<!-- more -->

# 重點提醒

當環境都設定好以後，日後不管是在哪一台電腦上撰寫文件、發佈文件、備份檔案，
都是在

`hexo 分支上`
`hexo 分支上`
`hexo 分支上`

因為很重要，所以說三次。



# 初次備份環境建立

此動作僅需在`第一台撰寫Blog電腦`上執行即可，
如果日後換新電腦或要在不同台電腦撰寫Blog的話，
請參考下面的 `更換環境` 的操作。

## 實作步驟

1. 新建 hexo 分支 
```
$ git branch hexo
```

2. 將檔案備份至 hexo 分支
```
$ git push origin hexo
```

3. github上設定 hexo 為 默認分支 

    日後不同電腦clone下來時，不用再特別切換 hexo 分支 
    {% asset_img backup01.png %}

4. 確認 配置hexo deploy 的参数

    確認 branch參數一定是設定 `master` 分支,因為日後操作都在 `hexo 分支上`，
    至於要發佈文件的話，就靠指令 `hexo d` 幫我們建立發佈的文章。

    ```
    deploy:
    type: git
    repository: https://github.com/用戶名/用戶名.github.io.git
    branch: master
    ```

***

# 更換環境

## 執行步驟

如果是第一次在新電腦的話，請先參考下列動作，先建立Blog的環境。

``` npm
#1.克隆到本地
git clone https://github.com/用户名/用户名.github.io.git Kanbooblog

#2.切換到blog目錄;
cd blog目錄

#3.確認目前分支是否為hexo，若不是，則切換備份的分支分稱(hexo)
git checkout Hexo

#4.安装各种npm包
npm install -g hexo-cli
npm install
npm install hexo-deployer-git --save

```

## 可能遇到的問題

### 1.warning: LF will be replaced by CRLF

在 `Windows` 中廣泛使用來標識一行的結束。而在Linux / UNIX系統中只有換行符。
也就是說在Windows中的換行符為CRLF，而在linux的下的換行符為：LF，
當執行 **git指令** 時，系統提示：LF將被轉換成CRLF。

**解決方法：**

``` git
$ git config --global core.autocrlf false
```

> [git提示警告：LF將被CRLF替換](http://blog.bflyer.com/2015/09/17/git%E6%8F%90%E7%A4%BAwarning-LF-will-be-replaced-by-CRLF/)
> [Hexo Git部署警告"warning： LF will be replaced by CRLF"的去除方法](https://gaomf.cn/2017/01/13/Hexo_Git_CRLF/)

***

# 部落格更新與部署新文章

日後在不同電腦上要新增文件的話，就重覆下列的動作即可。


1. 拉取遠端版本庫上的更新內容
```
$ git pull
```
2. 修改部落格配置或撰寫新文章

3. 添加變更並推送
```
$ hexo clean
$ hexo g //產生發佈的文件
$ hexo d //發佈至github-master

//備份至github-Hexo
$ git add .
$ git commit -m "message"
$ git push origin hexo
```

***

# 參考網站

[Hexo利用Github分支在不同电脑上写博客](http://www.dxjia.cn/2016/01/27/hexo-write-everywhere/)

[實現不同電腦上 Hexo 部落格的同步](https://hsins.github.io/2016/12/30/hexo-sync-with-multiple-computer/)

[hexo备份小技巧](http://luckylau.tech/2017/01/21/hexo%E5%A4%87%E4%BB%BD%E5%B0%8F%E6%8A%80%E5%B7%A7/)