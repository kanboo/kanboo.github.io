---
title: Hexo-環境建置
date: 2017-10-14 13:32:05
categories: 
- Hexo
tags:
- Hexo
---

# 前言

建立Hexo環境，需要完成的事情有：

> 1. [建置github空間](https://github.com/)
> 2. [安裝 Node.js](https://nodejs.org/en/)（如果還沒有的話）
> 3. [安裝 Hexo](https://hexo.io/zh-tw/docs/index.html)
> 4. [初始化 Hexo，並在目標資料夾建立所需檔案](https://hexo.io/zh-tw/docs/setup.html)
> 5. [安裝 `hexo-deployer-git`](https://hexo.io/zh-tw/docs/deployment.html#Git) (很重要)
> 6. [在_config.yml 中設定 **Git** 等資訊](https://hexo.io/zh-tw/docs/configuration.html)

<!-- more -->

***

# 空間：GitHub

因為Blog需要上傳至雲端，所以這裡使用github的免費空間。

## 創建github page

**倉庫的名字** 要和 **你的帳號** 一樣，如：`用戶名`.github.io 

{% asset_img githubpage1.png %}

## 進入github page項目設置頁面

選擇Settings

{% asset_img githubpage2.png %}

## 創建一個默認的頁面

直接選擇Launch automatic -> Continue to layouts page generator -> Publish page,其他東西都不用修改。

{% asset_img githubpage3.png %}
{% asset_img githubpage5.png %}

## 查看效果

至此，你的github page已經創建完成，你可以訪問自己的站點了。
如：https://用戶名.github.io/

***

# Hexo安裝

安裝 Hexo 相當簡單；然而，在安裝前您必須先檢查下列您的電腦是否已經安裝下列軟體：

* Node.js
* Git

若您的電腦已經安裝上述的必備軟體，那麼恭喜您！只需要透過 npm 即可完成 Hexo 的安裝。

```zsh
$ npm install -g hexo-cli
```

## 建立

``` zsh
$ hexo init yourname # ( 初始化新的 Hexo )
$ cd yourname  # ( 進入您剛剛建立的 Hexo 資料夾當中 )
$ npm install #（ 安裝 Hexo )
```

> **yourname** 就是指在電腦裡的檔案名稱，可以隨意取，例如「myhexoblog」

## 設定

進入您 Hexo 主目錄下之後，先找到 _config.yml 這個檔案！

```
title: Kanboo Notes (輸入您的標題)
subtitle: 健忘筆記本 （輸入您的至理名言）
description:(輸入您的網站描述)
author: Kanboo （輸入您的姓名）
language: zh-TW （輸入您所使用的語言）
timezone: (留空可以使用系統時間！)
```

同樣 _config.yml 這個檔案底下，找到 **deploy** 設定github

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/用戶名/用戶名.github.io.git
  branch: master
```

> **用戶名** 就是指你自己的**github帳號**，記得改對，然後按存檔。

## 撰寫文件及常用指令

### 撰寫文件

至於內文採用的 Markdown 格式不難學，只需要記幾個常用的指令，遇到不懂的再上網查就好。

```
斜體： *斜體* 
粗體： **粗體** 
粗斜體： ***粗斜體*** 
刪除線：~~刪除~~ 
引言： > 引言 
章節： # 章節 次級章節（以此類推）： ## 次級章節 
表格（註1）：|一行|一行|疊| 
插入超連結： [超連結文字](網址) 
插入外部圖片： ![圖片描述](圖片網址)
```

> [Markdown文件](http://markdown.tw/)
> [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
> [Markdown 寫表格](http://www.jianshu.com/p/sTeAbC)

### 指令

``` zsh
$ hexo new "postName"  #新建文章
$ hexo clean #清除快取
$ hexo generate #生成靜態頁面至public目錄
$ hexo server #開啟預覽訪問端口（默認端口4000，'ctrl + c'關閉server）
$ hexo deploy  #將.deploy目錄部署到GitHub
$ hexo help   #查看幫助
$ hexo version   #查看Hexo的版本
```

指令簡寫

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

***

# 更換Blog主題(NexT)

主要完成的步驟如下：

1. [安裝 Blog主題(NexT)](http://theme-next.iissnan.com/getting-started.html#download-next-theme)
2. [_config.yml 修改theme:NexT ](http://theme-next.iissnan.com/getting-started.html#activate-next-theme)
3. [NexT細項功能設定](http://theme-next.iissnan.com/getting-started.html#theme-settings)


如果啟用 **分類、標籤** 的話，並不會自動幫你建立，可參考下列協助產生

```
新建分類、標籤、關於頁面：
$ hexo new page ‘categories’
$ hexo new page ‘tags’
$ hexo new page ‘about’
```

> [hexo-generator-category	產生分類頁](https://github.com/hexojs/hexo-generator-category)
> `$ npm install hexo-generator-category --save`
>
> [hexo-generator-tag	產生標籤頁](https://github.com/hexojs/hexo-generator-tag)
> `$ npm install hexo-generator-tag --save`

***

# 參考網站

[Hexo搭建GitHub博客 系列文章](http://zhiho.github.io/categories/hexo/)
[Github Pages + Hexo搭建博客](http://fanzhenyu.me/categories/Hexo/)
[當個部落客](http://blog.30cm.net/2017/01/21/%E7%95%B6%E5%80%8B%E9%83%A8%E8%90%BD%E5%AE%A2%EF%BC%81%E3%80%8C-Hexo-%E3%80%8D%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8/)
[McK-Note](https://www.mcknote.com/2016/04/04/Set-up-McK-Note/)

[Hexo 官網](https://hexo.io/zh-tw/)
[NexT 官網](http://theme-next.iissnan.com/)