---
title: Hexo - 開啟旅途
date: 2021-05-02 16:17:35
tags: [hexo]
categories: [Hexo]
thumbnail:
banner:
---
### 踏上Hexo的旅途，開始為自己做紀錄 
  > *In three words I can sum up everything I have learned about life: it goes on.*
  > \- *Robert Frost*

**種一棵樹最好的時機是十年前，其次是現在。**
是否曾經想過紀錄生活的點滴，是否想要開始改變自己。每一天晚上睡前都想著要變得更好，隔一天早起床卻又是日復一日的一天，走到今日，是否忘記那些人生感動的片刻，是否覺得許多事情都變得不再有意義，面對生活的壓力，是否開始漫無目的，好像人本來就是要不斷奔波直到跑不動為止。

很多時候的沒意義，都只是因為忘記那感動的瞬間，可還記得聽見過讓你感動的音樂，可還記得令人樂此不疲的一件，可還記得最美的幾個瞬間。如果能夠回味，一定會很懷念吧!

#### 前言 用Hexo記住生活的每個片刻
今天開始Hexo系列文，希望能對想要有自己的Blog，卻苦於一些制式Blog不是那麼討喜的人，能夠帶來一些幫助，少走一點冤枉路。那，我們馬上開始吧!
以下是本篇文章講解的順序，盡情挑選自己想看的重點吧!
- Hexo是什麼?
- 踏上旅途整理行李 -> Git與Node
- 正式趟上Hexo旅途
- 出發吧! Hexo
- 屬於每個人的Hexo路
- 結語

***

#### Hexo是什麼?
簡單來說，Hexo是一個**部落格的框架**，部落客只需要撰寫**Markdown**格式的檔案就可以上傳到自己的部落格上，當然在這之前需要自己先踏上名為Hexo的這條路，才可以享受一路上的風景。Hexo非常容易入門，更有相當多元的主題(模板)可以套用到自己的部落格上，以更客製化的方式。

Hexo為想要有自己部落格卻又不想使用現有市面部落格平台的人，無疑帶來許多好處，具有高度客製化外，還能夠享受自己架設網站的感覺。對於我自己來說，雖然不盡理想卻是一個現況最佳的作法，畢竟完完全全架起自己的網站，知識層面的時間成本就相當高，若再算上購買網域和虛擬主機，長期下來也是相當可觀的。

***

#### 踏上旅途整理行李 Git與Node
執行Hexo需要使用到Git與Node，首先根據以下網址下載安裝對應電腦作業系統(OS)的檔案:
1.  安裝Node.js https://nodejs.org/en/
2.  安裝Git https://git-scm.com/downloads
   
![Version](https://i.imgur.com/NLE6I8P.jpg)

接下來會在Command Line(命令提示字元)上執行指令，以縮寫cmd代表Command Line，在macOS則是Terminal。
以上是我的Node和Git版本，檢驗是否成功在cmd輸入:
- node --version
- git --version


需要特別注意的是Hexo要求Node的版本必須高於 Node.js 10.13，官方推薦最好是 Node.js 12.0以上，安裝最新版的話基本上就沒有問題。

***

#### 正式趟上Hexo旅途
成功安裝Node和Git後，我們可以直接在cmd執行以下的指令安裝Hexo:
`> npm install -g hexo-cli` (-g代表global) 

若想單純安裝在專案的話則執行以下指令安裝Hexo:
`> npm install hexo`

在一切都平安無事安裝完後，讓我們正式來看看Hexo一路上有什麼特別的風景吧!
首先執行下方指令:
`>hexo init <folder name>` (init代表initialize: 初始化)
`>cd <folder name>`(cd代表change direcoty: 切換路徑)
`>npm install` (安裝需要的套件)

所謂的**初始化**是將既有的檔案回復預設的樣子，如果沒有檔案則建立檔案，把它**想成手機的回復原廠設定**就可以理解了，對吧!至於npm install則是會安裝我們在初始化**hexo init**時，產生一個叫做package.json的檔案裡面寫好的檔案，在執行後才會將所需的套件安裝到這個資料夾裡面，需要安裝的套件都寫在**dependencies**裡面。

![package.json](https://i.imgur.com/fH6vqTq.jpg)

不過在最新版Hexo執行初始化的時候，都會順便安裝完成，不執行npm install我想也是沒問題的!
接著可以看到建立好的資料夾(以下稱為專案)裡面的檔案有:

![init](https://i.imgur.com/koCqzQE.jpg)

可以看到有許多的資料夾，在下一篇會介紹這些資料夾各自的意義和用途。
目前為止已經完成所需的準備了!

***

#### 出發吧! Hexo
`>hexo server` (server別名 s)
上方指令代表啟動伺服器，執行後cmd上顯示

`>hexo s`
`INFO  Validating config`
`INFO  Start processing`
`INFO  Hexo is running at http://localhost:4000` 
在Chrome輸入localhost:4000，就可以看見以下驚奇的畫面!

![localhost](https://i.imgur.com/FNpSJNt.jpg)

恭喜你/妳，我們已經正式出發了!
接下來的問題是...怎麼在網路上連接到我們的網站呢?

***

#### 屬於每個人的Hexox旅途
我們會使用Github Pages作為網域去架起Hexo的部落格，若不喜歡或不想要使用Github Pages當然也可以自己購買網域和主機，只是成本就會相對高一點，若未來有機會我們再針對這部分做討論。

首先註冊一個Github的帳號後，在建立Repository頁面輸入Repository的檔案名為:
`/<username>.github.io`

![gitpage](https://i.imgur.com/TQemfg9.jpg)

完成後在cmd安裝發布Deployment的套件
`git install hexo-deployer-git --save`
再來修改**_config.yml組態文件**中修改url欄位與deploy部署的欄位為剛才在Github建立的Repository的名字
```
url: http://Mizukikouyou.github.io
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

```
deploy:
  type: git
  repo: https://github.com/Mizukikouyou/Mizukikouyou.github.io
  branch: main
```

最後執行以下的指令
`>hexo d`
就可以在你自己的github名稱的github.io裡面看到Git Pages與Hexo成功的搭載上去啦!\
以上就是今天的Hexo系列文的第一篇!

***

#### 結語
原本預期很快就可以完成第一篇文章，但卻在Github上面摔了一大跤，主要是在使用兩個不同的帳戶去做Deploy，卻一直遇到SSH的問題，測試了好幾種方法，最後卻還是沒辦法解決問題，折衷方案是先用Git Bash強硬推上去，卻遇到更詭異的問題，過幾天再找時間解決它!

我想成功建完Hexo的部落格多多少少會有一點成就感對吧!但，這只是一個開始，再接下來，讓我們一起研究怎麼客製化專屬於自己的Hexo部落格。辛苦看到結尾的你們，還有許多沒有提到，但礙於篇幅就留到下一篇吧!

若有任何缺失或錯誤，再煩請跟我說，謝謝您。

***