---
title: Node系列-Node介紹與基本概念
date: 2021-06-08 21:05:08
tags: [node, javascript]
categories:
  - [Programming, Javascript, Node]
thumbnail:
banner:
---
### Node介紹與基本概念
> *Never memorize something that you can look up.*
> *― Albert Einstein*

在愛因斯坦的身上，總能夠得到發人省思的一番話，背後一席大道理，目前的我還不太能夠理解。

今天是新系列文想來聊聊關於Node的故事。經過React一番洗禮兼摧殘，終於成為一名React菜鳥，不過可能有人會想連前端都還沒搞專精，就來接觸後端是不是哪裡搞錯? 但是我個人的想法是，唯有學習前端與後端後，才有可能真正專精前端或後端，總不可能後端完全不懂，就串起各種API，就算真的串起來，估計後續也有很多延伸問題，我想至少這是我目前的看法，那，我們趕緊進入正題!

- Node介紹
  - Node核心模組
- 結語

*** 
#### Node介紹
最近才突然頓悟前後端的區別，以前總覺得自己好像有一定的認知，才發現認知完全是錯誤的。大學期間曾經透過Express和Pug樣板去製作簡單的應用程式，也曾經使用過Flask搭配Pug去處理簡單的網頁，但在學習完React的這陣子後，我才突然想到為何學習前端都沒有聽到Express和Flask，最近發現原來這兩個框架是屬於**後端框架**! 完全跟React八竿子打不著，話是這麼說，但究竟差異在何處?

隨著JS的強勢，2009年源自瑞安·達爾的Node逐漸竄起，甚至可以說是當前最熱門的後端程式語言其一，透過Google開發的V8引擎上而得以運行的伺服器端環境。從此脫離Apache的手掌心，JS更因此從前端語言一躍而出。

Node模組絕大多數以JS撰寫，因此JS本身就能夠自成一套完整的系統，感覺就像是3C產品都用Apple的感覺，一體感所帶來的快感實在美好。簡言之，沒有不用Node的道理！

##### Node核心模組
基本上Node的核心模組有http、fs、process等等，數不勝數，不過說這麼多還是直接來實際看看Node的使用方式吧!

面對不同面向會有不同的模組可以使用，不管是要處理HTTP的回應、檔案的讀取寫入、執行緒的控制，甚至是加密演算法都能夠透過Node去實現。

今天我們來透過運行伺服器去理解Node的使用，網路一般的運作方式是透過以下的方式:
`使用者請求 -> 瀏覽器請求 -> 伺服器回應 -> 瀏覽器回應 -> 使用者讀取`
大致上可以上述**粗略**的方式去理解瀏覽網頁的經過，接著我們嘗試透過Node去建立自己的伺服器。

```
// 匯入處理http的核心模組
const http = require('http');

// 建立http協議的伺服器，回傳Server物件
const server = http.createServer();

console.log(server);

// 伺服器監聽埠(PORT)3000的所有資訊
server.listen(3000);
```

先前我們提到Node有相當多的核心模組，http即是用來處理相關請求、回應的模組。在匯入模組後，建立http協議的伺服器給server，最後監聽server在3000埠口的所有資訊。一種簡單的想法是，server就是在稱作3000路口的監視錄影機，可以處理所有經過路口的車輛與行人。

為了處理經過車輛與行人資訊，我們必須告訴Server車輛與行人要去哪裡，要以何種方式經過路口。

```
const http = require('http');

// req: 接收的資訊(車輛與行人) res(監視器偵測的回應)
const server = http.createServer((req, res) => {
  // 擷取req請求的網址(URL)
  const url = req.url;
  // 擷取req請求的方式(GET, POST, PUT, DELETE)
  const method = req.method;

  console.log(url);
  console.log(method);
});

server.listen(3000);
```

在Server物件當中有相當龐大的預設資訊，但最主要的則是伺服器接收的訊息與給予的回應，在上方我們拆解出**請求**伺服器的網址，預設的情況下是/，而預設的請求方法則是**GET**。但網頁不僅止於此，我們同樣可以給予伺服器其他的資訊:

```
const http = require('http');

const server = http.createServer((req, res) => {
  // 寫入請求後回應的資訊
  res.write('<html>');
  res.write('<body><p>Welcome to Node.js</p></body>');
  res.write('</html>');

  // 結束接收回應
  res.end();
})

server.listen(3000);
```

我們直接在回應的部分寫入HTML格式的程式碼，實際執行後就能夠見到**Welcome to Node.js**的字樣出現在localhost:3000的網頁當中，不僅告訴Server所有資訊的流向與方式，我們同樣可以掌握車輛與行人的數量。這就是最簡單，最基本地使用Node核心模組http的方式。

![http](https://i.imgur.com/ZhIBjlv.jpg)

我們再看一個範例:

```
const http = require('http');
// 匯入處理檔案的核心模組
const fs = require('fs');

const server = http.createServer((req, res) => {
  // 同步寫入檔案 -> writeFileSync('檔案名稱', '檔案內容')
  fs.writeFileSync('message.txt', 'Node is powerful');
  res.write('<body><h2>You are writting file!</h2></body>');  
  res.end();
});

server.listen(3000);
```

匯入fs的模組後，我們呼叫fs模組內writeFileSync函數，這個函數主要用於同步處理檔案寫入，不過這邊暫且不討論同步非同步的問題。實際執行後，會在同樣的資料夾下建立message.txt的檔案，裡面確實寫入**Node is powerful**的字樣。

![fs](https://i.imgur.com/rOoKSFf.jpg)

***

#### 結語
我曾用Expres和Flask各自寫些小網頁，結果才發現是一大烏龍，就覺得自己很蠢很好笑。不過今天又重新學習Node，希望我在未來不久內可以了解更多關於後端的運作，特別是API的部分，該找時間寫篇關於API的文章，因為最近用React串接其他公司的API實在慘不忍睹，只能說自己還要多加油。

希望這篇能對剛入門Node的讀者們有點幫助，就讓我們一起成長吧!!