---
title: Tool-CSScomb
date: 2018-05-31 10:19:16
categories:
- tool
tags:
- tool
- vscode
- CSScomb
---

{% cq %}

{% asset_img csscomb.png %}

<font style="font-size:20px;">CSScomb - CSS 屬性排版插件</font>

{% endcq %}

<!-- more -->

---

## 前言

以往再寫 SCSS 時，都會用此插件，整理 SCSS 的排序，這樣很整齊看起來滿爽的，

不過最近遇到在寫 SCSS 時，同時有使用二個以上的 `mixinx` 的話，就會造成排版錯亂的效果。

{% asset_img error.png %}

---

## 修正

紀錄一下，如何解決自動排版錯亂的問題。

### 1.Settings 設定

因為預設沒有 `mixin` 的排序，所以需調整 csscomb 的參數設定，下列附上我的設定值。

{% asset_img sortorder.png %}
<br/>

<div class="note info">[我的 csscomb_preset](https://gist.github.com/kanboo/c91db53644b8389dffce659db02c46f0)</div>

### 2.CRLF 與 LF 設定

將原先的 <font color="red">CRLF</font> 更改為 <font color="red">LF</font>，然後再重新執行一次，即可。

{% asset_img changelf.png %}
<br/>

<div class="note info">[解決問題的issue](https://github.com/csscomb/csscomb.js/issues/562)</div>

<div class="note primary">補充說明
[CRLF 跟 LF 之區別 --- 隱形的 bug](http://violin-tao.blogspot.com/2016/05/crlflf-bug.html)
[Git 在 Windows 平台處理斷行字元 (CRLF) 的注意事項](https://blog.miniasp.com/post/2013/09/15/Git-for-Windows-Line-Ending-Conversion-Notes.aspx)</div>

---

## 參考文件

<div class="note info">[Gua - 好用的CSS屬性排版插件](https://guahsu.io/2017/10/CSScomb-css-sort-format/)
[官方vscode-csscomb](https://github.com/mrmlnc/vscode-csscomb)</div>
