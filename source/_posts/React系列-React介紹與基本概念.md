---
title: React系列-React介紹與基本概念淺談
date: 2021-05-06 11:27:46
tags: [react, javascript, component]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### React介紹與基本概念淺談
> *Live as if you were to die tomorrow. Learn as if you were to live forever.*
> *- Mahatma Gandhi*

React是近年來盛行的三大框架其一，在ES6後甚至更加盛行。國外統計程式設計師學習React的人數逐年上升，可見React的強勢，因此今天想和大家一起來學習使用React。當然不是隨波逐流，而是在嘗試使用過React後，發現它的強大!更不消說有超級多的第三方函式庫可以使用。那，廢話不多說，我們趕緊往下看!

- React的定義與用途
- React的基本概念
  - React的組件(Component)
  - React的兄弟JSX(JS-XML)
- 結語
***

#### React的定義與用途
- **React是用來打造使用者介面(UI)的JS函式庫。**
- **React是由組件(Component)所組成。**

讀者可能會想說不是有HTML、CSS、JS就可以打造UI了嗎? 為什麼要大費周章去學習一項新的技術? 其實一項新的技術的產生，都是因為舊有的東西的缺點存在，才會促使新的科技、技術的發展。好比人類覺得走路太久才能到達目的地，所以研究出動力機械，這些道理都是一樣的。可想而知，React產生固然是因為傳統的HTML和CSS在撰寫介面上，不管是維護還是新增介面，盡管有PUG和SCSS這些預處理器多少可以加速開發，但是卻還是有其致命的缺點。

使用傳統的HTML和CSS的主要缺點:
- 頁面轉跳時都需要發送GET請求到Server端後再次渲染頁面
- 當APP變得複雜且龐大時難以維護以及管理
- 網頁載入速度慢

使用React的優點:
- 整個Web只會在第一次開啟時發送GET請求
- 以組件的方式開發網站
- 網頁載入速度超快

***

#### React的基本概念
網頁最後呈現給使用者的始終是HTML、CSS以及JS。
React使用宣告式方法(Declarative Approach)去執行程式，因此開發者只需要專注在程式的狀態(State)，接著React就會幫我們處理JS和DOM的程式碼，聽起來很像魔法，但其實就是我們只需要懂得駕駛React這台跑車，至於引擎是V8還是V12，不懂也沒關係。

有機會我們再來深入研究宣告式(Declarative)與指令式(Imperative)程式設計!

##### React的組件(Component)
React的核心概念就是組件，所謂的組件就是將HTML、CSS、JS三者合在一起，最後再由組件去建立整個使用者介面(UI)。
因此組件的優點有:
- 可重複使用(Re-usable)
- 可回應的(Reactive)

React的架構大概如下:
![react](https://i.imgur.com/9LTFVrR.png)

大概就像圖表一樣的架構，抱歉，我沒有美術天分XDD。

##### React的兄弟JSX(JS-XML)
React的程式碼如下:
![jsx](https://i.imgur.com/Gu2EQPY.jpg)

剛開始接觸React一定會充滿困惑，會什麼JS檔案裡面的程式碼相當奇怪? 居然包含像是HTML的標記語言，其實這是React開發團隊為了方便撰寫組件所開發的程式碼，叫做JSX(JS-XML)。在渲染畫面前，React會透過第三方函式庫將JSX轉換成瀏覽器讀得懂的方式。

今日回顧:
- React是HTML、CSS、JS整合後的框架
- React的特殊語法JSX
- React組件的概念

#### 結語
今天只稍微了解一下React框架以及它的由來，接下來我們就會逐步進入React專案實作囉! 能夠用React實作各種大大小小的網站，用想的就覺得興奮對吧!如果重複打繁瑣的Code還不足以讓你學習它的話，那再接下來的文章你一定會愛上React。
