---
title: React系列-開始打造React專案
date: 2021-05-06 12:26:24
tags: [react, javascript, component]
categories: 
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### 建立屬於你/妳自己的React專案!
> *Yesterday is history, tomorrow is a mystery, today is a gift of God, which is why we call it the present.*
> *― Bill Keane*
今天要講解的東西很輕鬆，就是用指令去建立一個React的專案。
--預備知識--
- 知道命令列介面(Command Line Interface)
  - Windows -> cmd
  - Mac -> terminal

--講解流程--
- 安裝Node.js
- 建立React專案
- 結語

#### 安裝Node.js
在透過指令安裝React專案前，我們必須先下載Node.js，如果已經安裝好可以直接跳到下一個步驟。
- Node.js: https://nodejs.org/en/
  
![node](https://i.imgur.com/w0EDv6j.jpg)

直接安裝16.1.0Current版本也可以，接著就照著安裝指示安裝即可。

#### 建立React專案
安裝完成後打開命令列介面，切換到自己想要建立專案文件夾的路徑後，輸入以下指令建立React的專案。

![react](https://i.imgur.com/OzGORev.jpg)

可能會有提示需要安裝create-react-app套件的指示出現，就輸入Y就可以安裝了!
接著就會看到專案成功建立完成!把路徑切換到資料夾裡面後，輸入**npm start**就可以成功在本地端(自己的電腦)運行。

![react-run](https://i.imgur.com/baMBc6M.jpg)

接著打開瀏覽器就可以看到React專案預設好的畫面!
![react-home](https://i.imgur.com/4ujriXZ.jpg)

這就是用React開始撰寫網站的第一個步驟!

所謂的套件，在這裡指的是由其他人統整好的函式庫，整個套件含有許多我們在開發React專案的時候需要引入的函式庫等等。因此我們可以知道**create-react-app**就是負責幫我們把需要的工具都整理好，畢竟工欲善其事，必先利其器，就是這個道理。

***

#### 結語
其實這個部分只是想要對開發環境重新做一個認識，順便記錄下來心想或許會有剛接觸到React的程式設計師可以快速閱覽一下。我其實還有遇到webpack版本問題，因為之前在開發的時候把webpack安裝到全域裡，導致建立好的React專案的webpack套件版本被覆蓋過去而跳出警示訊息，所以還是應該好好的把全域弄得乾乾淨淨是最好的選擇。畢竟一天到晚都在搞開發環境，而沒有任何心力專注在開發的話，大概會發瘋吧?






