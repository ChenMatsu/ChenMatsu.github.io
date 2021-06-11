---
title: Express系列-基本Route和Middleware
date: 2021-06-11 14:44:51
tags: [javascript, node, express, routes, middleware]
categories:
  - [Programming, Javascript, Node]
  - [Programming, Javascript, Node, Express]
thumbnail:
banner:
---
### Route和Middleware
> *Have no fear of perfection - you'll never reach it.*
> *― Salvador Dali*

昨天第一次介紹Express後端框架，簡單說明如何在Node當中使用以及它的主要功用，另外也提到Middleware的基本概念，今天就來更有效利用Express的超能力吧!

- Express Middlewares
  - Express Routes
- 結語

*** 

#### Express Middlewares
比起中介軟體，我還是習慣直接唸英文，或者想成是橋接器都比中介軟體念起來順手多，在概念上也更容易成形。首先我們建立新的檔案app.js，上一篇文章都有提到，忘記的話可以回上一篇回顧。

```
const express = require('express');
// Express處理檔案路徑的核心模組
const path = require('path');
const app = express();

// 內建函數用於提供靜態檔案
app.use(express.static(path.join(__dirname, 'public')));

app.listen(3000);
```

這裡特別注意的地方是app.use這段程式碼當中，我們透過Express內建函數static()提供靜態檔案，而路徑的設定方式則是使用核心模組path處理，**主要用途在於可以匯入CSS檔案給HTML。**

至於**path.join**函數則可以根據作業系統(OS)將檔案串接起來，舉例來說: 
假設路徑路徑為public -> css -> index.css，則會對應如下 
`Windows: \public\css\index.css`
`Linux: /public/css/index.css`
不一樣的地方在於預設路徑的寫法，現在我們不用去煩惱不同作業系統間的寫法啦!此外，若沒有透過path模組，在Express應用程式當中必須使用絕對路徑。

基本上，我們只需要安裝需要的套件就可以執行功能，省去相當多繁瑣的細節。

##### Express Routes
最後想特別提及的部分是關於Routes的處理，在Express當中我們一樣是經由Middleware的概念去處理Routes。因為Routes大部分都和網頁URL有關，固然app.use本身就可以拿來處理Routes。

可以根據不同請求方式，改寫Middleware啟動的時機，如下: 

```
app.get((req, res, next) => {
  console.log('Mioali is passed!');
  next();
});

app.post((req, res, next) => {
  console.log('Hsinchu is passed!');
  next();
});

app.put((req, res, next) => {
  console.log('Taoyuan is passed!');
  next();
});

app.delete((req, res, next) => {
  console.log('Taipei is arrived!');
});
```

Express Middleware的get, post, put, delete剛好能夠各自對應到http的不同傳輸方式。現在，我們實際上來操作看看Routes。

首先新增routes的資料夾，接著分別建立welcome.js和users.js。程式碼分別如下:
`welcome.js`
```
const express = require('express');
const path = require('path');
// 使用Express內建函數來指向切割後的Routes
const router = express.Router();

router.get('/', (req, res, next) => {
  res.sendFile(path.join(__dirname, '..', 'views', 'welcome.html'));
});

// 匯出檔案的Routes
module.exports = router;
```
`users.js`
```
const express = require('express');
const path = require('path');

const router = express.Router();

router.get('/users', (req, res, next) => {
  res.sendFile(path.join(__dirname, '..', 'views', 'users.html'));
});

module.exports = router;
```
我們將原本寫在app.js處理Routes的Middleware切割到其他的檔案當中，透過Express提供的**Router**函數，我們因此可以指向相同的URL。在GET發出請求後，我們回傳**res.sendFile**()函數，它的功能如同函數名稱一樣，用途在於傳送檔案給使用者，聽起來似乎很奇怪，但有時候我們必須在開發者和使用者角度不斷切換，才有助於理解程式開發。在res.sendFile當中，一樣需要透過path模組去調整我們要存取的路徑，以welcome.js檔案當中，get最後會回傳的檔案會指向類似C:\Users\user\tutorial\views\users.html的檔案路徑。

最後因為我們指向兩個html檔案，自然就需要建立它們，因此先建立views資料夾後再各自建立welcome.html和users.html，內容簡簡單單如下即可:
`welcome.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome</title>
</head>
<body>
  <h2>Welcome</h2>
</body>
</html>
```
`users.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Users</title>
</head>
<body>
  <ul>
    <li>Max</li>
    <li>Matsu</li>
    <li>Complex</li>
  </ul>
</body>
</html>
```
最後實際啟動npm start後，我們就能夠順利看到welcome.html和users.html的畫面囉!

**localhost:3000的畫面**
![welcome](https://i.imgur.com/14i8Ndu.jpg)
**localhost:3000/users的畫面**
![users](https://i.imgur.com/18IKsOk.jpg)

此外若想要套用CSS，只需要將.css檔案，直接寫在HTML檔案各自的head即可。
***

#### 結語
今天再Review過一次Express的一些基本概念和設定，其實完全算不上真正的開工哈哈哈。不過沒關係，一步一腳印，至少今天已經搞懂在Express切割Routes以及如何在HTML檔案當中參照到專案中的CSS檔案，很多隱藏在背後的細節其實需要深思過後才能夠漸漸明瞭運作，若只是單純的套用別人已經寫好的API，我想久而久之會失去自己的思考能力吧! 

**「物事の本質を考えろう！」と阿部寛がこう言った。真正重要的是思考事物的本質。**

繼續苦等東大特訓班2...

