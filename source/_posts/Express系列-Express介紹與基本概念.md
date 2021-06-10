---
title: Express系列-Express介紹與基本概念
date: 2021-06-10 20:08:55
tags: [javascript, node, express]
categories:
  - [Programming, Javascript, Node]
  - [Programming, Javascript, Node, Express]
thumbnail:
banner:
---
### Express介紹與基本概念
> *I have not failed. I havve just found 10,000 ways that won\'t work.*
> *― Thomas A. Edison*

Express是伺服器或稱作**後端框架**，最近才認知到Express有別於React、Angular和Vue，說起來也算是天大誤會，好在還沒弄成笑話。今天一起來看看這個框架可以給予我們什麼樣的能力吧!

- Express介紹
  - Express使用
- 結語

***

#### Express介紹
誠如先前所說，Express是伺服器框架其一 ，其餘的類似的框架有Laravel、Flask等等。Express主要的用途在於協助Node去打造Web應用程式的框架和API，大幅減輕開發的負擔。以往在Express開發前，處理API和Web應用程式，需要針對網頁的請求與回應逐一處理，但現在只需要透過Express就可以達到一樣的結果。Express扮演的角色就像是遙控器一樣，開啟電視只需要按下開機關機鍵，若沒有遙控器的話則必須直接按電視螢幕**本身**的開關，但結果上來看是一樣的。

簡言之，Express就是Node處理網站的遙控器!

##### Express使用
在使用Express前，我們需要先認識**中介軟體**(Middleware)，中介軟體本身可以想成火車，假設火車從台中北上開往台北，這一路上必須經過苗栗、新竹、桃園、新北幾個城市，而中介軟體則是城市與城市間的溝通方式，並且溝通結果會影響到接下來的行程。

首先在一個資料夾內建立app.js的檔案，基本上檔案名不會影響，不過一般都會命名為app或index兩者其一。建立完成後，切換到資料夾內執行**npm init**設定專案檔。一路按Enter到底將專案檔設為預設即可。大致上會看起來像下方圖片的路徑。

![pic1](https://i.imgur.com/WyiVozE.jpg)

接著便於開發我們分別下載**nodemon**套件以及必備的**express**框架，執行以下指令:
`npm install nodemon --save-dev`
`npm install express --save`
第一個套件的主要功能是用於**自動重啟伺服器**，而不用手動一直重新開啟伺服器，安裝完成後，將**package.json**檔案寫入一個新的腳本檔案，就可以無痛使用nodemon從此不再手殘，如下:

![script](https://i.imgur.com/jAz6Hf7.jpg)

接著回到app.js檔案，這裡我們將Express套件引入檔案中:

```
// 匯入Express框架套件
const express = require('express');

// 建立Express應用程式 -> 物件具有許多Function和Settings
const app = app.express();

// 等同於
// const server = http.createServer();
// server.listen(3000);
app.listen(3000);
```

這段程式碼當中，我們建立Express的應用程式，透過它我們可以使用Express提供的各種Middleware來處理網站，從最後一行app.listen()可以看出Express已經將http核心模組打包起來。
`const server = http.createServer()`
`server.listen(3000)`
↓
`app.listen(3000)`

接下來我們將app.js稍微修改一下:
```
const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('Set off at Taichung City!');
  // 告訴Middleware可以前往下一個Middleware!
  next();
});

app.use((req, res, next) => {
  console.log('Mioali is passed!');
  next();
});

app.use((req, res, next) => {
  console.log('Hsinchu is passed!');
  next();
});

app.use((req, res, next) => {
  console.log('Taoyuan is passed!');
  next();
});

app.use((req, res, next) => {
  console.log('New-Taipei is passed!');
  next();
});

app.use((req, res, next) => {
  console.log('Taipei is arrived!');
});

app.listen(3000);
```

透過next()可以調動middleware不斷前往下一個middleware直到發生狀況或是抵達終點。
最後**npm start**啟動伺服器後就可以發現我們的中介軟體確實可以一路由台中抵達台北! 

***

#### 結語
今天簡單講解Middleware的概念，其實Middleware可以細分成不同層級的Middleware，就留到下次有機會再來討論。希望不會有人再跟我一樣傻傻分不清楚前後端框架。

最近真的是想到什麼就寫什麼，完全沒有規劃，不過總是想的比較多。這系列文章估計也會寫好幾篇，希望能和React結合在一起XD。
