---
title: Hexo-備份
date: 2017-10-14 14:14:39
categories: 
- Hexo
tags:
- Hexo
- 備份
---


# 初次備份環境建立

此動作僅需在`第一台撰寫Blog電腦`上執行即可，
如果日後換新電腦或要在不同台電腦撰寫Blog的話，
請參考下面的 `更換環境` 的操作。

## 實作順序

第一次執行備份至 github的分支(Hexo) 時，他會自動幫你建立新的分支(Hexo)。

1. 安裝 hexo-git-backup 
2. _config.yml文件中，設定好**github路徑**及**分支名稱**
3. 執行 `hexo b`

<!-- more -->

## 安裝插件

``` npm
npm install hexo-git-backup --save
```

> [hexo-git-backup插件](https://github.com/coneycode/hexo-git-backup)

## 設置

在 hexo/_config.yml文件中为插件添加设置

``` npm
# 備份Hexo
backup:
    type: git
    theme: next,landscape  //可選，主题備份
    repository:
       github: https://github.com/用户名/用户名.github.io.git,Hexo  //備份的目的路径，必填branch分支
```

## 備份指令

``` npm
hexo backup
```

或者

``` npm
hexo b
```

***

# 更換環境

如果是第一次在新電腦的話，請先參考下列動作，先建立Blog的環境。

``` npm
#1.克隆到本地1
git clone https://github.com/用户名/用户名.github.io.git Kanbooblog

#2.切換到blog目錄;
cd blog目錄

#3.切換備份的分支分稱
git checkout Hexo

#4.安装各种npm包
npm install -g hexo-cli
npm install
npm install hexo-deployer-git --save
npm install hexo-git-backup --save

#5 部署
hexo clean
hexo g //產生發佈文件
hexo d //發佈至github-master
hexo b //備份至github-Hexo
```
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
hexo clean
hexo g //產生發佈的文件
hexo d //發佈至github-master
hexo b //備份至github-Hexo
```

***

# 參考網站

[實現不同電腦上 Hexo 部落格的同步](https://hsins.github.io/2016/12/30/hexo-sync-with-multiple-computer/)

[hexo备份小技巧](http://luckylau.tech/2017/01/21/hexo%E5%A4%87%E4%BB%BD%E5%B0%8F%E6%8A%80%E5%B7%A7/)