---
title: MAC-讓終端機變好看
date: 2017-10-15 18:57:32
categories: 
- Mac
tags:
- Mac
- iTerm2
- ZSH
- 終端機
- terminal
---

# 前言

在安裝 MAC終端機高亮的過程，主要是參考下列的文章，跟著步驟安裝及設定即可，
不過安裝過程中，還是有遇到卡卡的地方，所以特別記一下 `卡卡的歷程`。

> [為 MAC 的 Terminal 上色 - 透過 iTerm 2 和 Oh My Zsh 高亮你的終端機](https://pjchender.blogspot.tw/2017/02/mac-terminal-iterm-2-oh-my-zsh.html)

> [讓 MAC 終端機變好看](https://paper.dropbox.com/doc/MAC--JrxAtuSA7DKDCsPpK3jf1)

> [iterm2-solarized 原文](https://gist.github.com/kevin-smets/8568070)

<!-- more -->
***

# 問題紀錄

## zsh指令失效

安裝完zsh，在使用相關shell命令，出現了
`zsh: command not found Node`
`zsh: command not found: hexo` 
等一系列error

字面意思是相關命令沒有沒有找到，其實就是bash shell 以及 zsh shell是 `兩種讀取系統環境變量`，
簡單來說就是說 `node、Hexo`...等套件 是在使用 `bash` 時候，安裝好的，所以會設定在 `.bash_profile`裡，
後來改使用 `zsh shell` 的時候，你並沒有把相關的環境變量的配置設置到 `.zshrc` 中（功能上類似bash 的.bash_profile），
所以 .zshrc 沒有配置相關環境變量設置，就把bash 中.bash_profile 全部環境變量加入 .zshrc 就好。

加入方法：
``` 
#打開 zsh 的設定檔
open ~/.zshrc

# 最后一行加入下面指令
# 解決OSX使用oh-my-zsh後.bash_profile自定義失效
source ~/.bash_profile
```

> [zsh: command not found](http://www.chongchonggou.com/g_841616996.html)
> [zsh: command not found](http://www.jianshu.com/p/d5cab693c5d4)
> [解决OSX使用oh-my-zsh后.bash_profile自定义失效](http://to-u.xyz/2016/08/07/zsh-bash/)

## 安裝字型

原先是跟著最上面文章教學，安裝 字型(Meslo LG M Regular for Powerline)，再修改相關設定後，
此字型在 iTerm 上，顯示箭頭是正常的，不過在VScode上面，顯示 `箭頭` 卻還是有亂碼，
所以去Google一下，結果如下


1. 先安裝 字型
``` zsh
# 安裝字型
$ git clone git://github.com/powerline/fonts ~/.powerline_fonts
$ cd ~/.powerline_fonts
$ ./install.sh
```
2. iTerm設定 字型
iTerm --> Preferences --> Profiles --> Text --> Change Font
推薦使用 `Source Code Pro` (Adobe的）與 `Ubuntu Mono`，這兩個字型都相當適合寫程式使用。

3. VSCode設定 字型
在設定參數裡，新增下列二行
```
"terminal.integrated.fontFamily": "Source Code Pro for Powerline",
"terminal.integrated.fontSize": 14
```

> [安裝Powerline專屬字型](http://oomusou.io/osx/iterm2-setup/#安裝Powerline專屬字型)
> [美化你的終端機](https://medium.com/appworks-school/%E7%BE%8E%E5%8C%96%E4%BD%A0%E7%9A%84%E7%B5%82%E7%AB%AF%E6%A9%9F-7af0dabcf368)