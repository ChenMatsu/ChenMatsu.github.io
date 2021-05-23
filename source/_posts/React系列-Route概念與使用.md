---
title: React系列-Route概念與使用
date: 2021-05-18 11:16:19
tags: [react, javascript, component, route, spa]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Route in React
> *How did it get so late so soon?*
> *― Dr. Seuss*

每一個人都有喜歡的事情，只是有時候會忘記。若那件事情，使時間飛逝，那無庸置疑是你喜歡的事，至於它的意義，並不需要由他人來決定。

從Hooks抽身，今天想要來說說關於Route的基本概念，當然往後會再來說說關於其他Hooks。人生就像在一個又一個的岔路上，React也不例外。想到世界的盡頭，就勢必要懂Route。(笑)

- Route
  - Route的基本使用
    - NavLink
    - Switch
    - Link
    - Redirect
- 結語

#### Route
提到React就會聯想到SPA(Single-Page-Appliaction)，但是即便是React有時候也需要導向其他的頁面，以Netflix來說，總不可能所有影片都在同一個URL執行對吧? 不過若導向其他頁面，那麼還可以稱作SPA嗎? 當然可以，最簡單的判別方式就在於資料流是否與傳統的HTML網站相同。

經過磨練，我們都理解到React網站僅會在第一次載入網站時發出所有請求，在後續網頁的變動上，僅會更動需要更動的組件，這就是React相較於傳統網站最大的優勢。大幅減輕網站渲染的時間，降低使用者等待時間，達到提升網站使用的舒適度，仔細一想React比喻成跑車也不為過。因此只要每一個頁面都符合React的風格，那每一個頁面都可以稱做SPA，只不過搭配Route後，網站就會升級成Multi-SPA。廢話不多說，我們趕緊來看!

首先任何一個React專案下，我們只需要額外安裝react-router-dom第三方函式庫即可。在專案終端機上下`npm install react-router-dom`的指令，這裡要稍微注意react-router與react-router-dom兩個函式庫有些小差別。

安裝完成後，分別在components資料夾下建立Welcome.js和Travel.js兩個組件。接著App分別匯入兩個檔案，並且匯入react-router-dom的組件，如下:
```
import { Route } from 'react-router-dom';

import Welcome from './components/Welcome';
import Travel from './components/Travel';

function App() {
  return (
    <div>
      <Route path="/welcome">
        <Welcome />
      </Route>
      <Route path="/travel">
        <Travel />
      </Route>
    </div>
  );
}

export default App;
```
在這裡我們將**Route**組件匯入後就可以使用，使用Route組件方式和其他組件一樣，只不過它有專屬於自己的屬性。在Route組件的path可以用於設定URL的路徑，接著我們將Welcome和Travel兩個組件分別包在Route裡面，就可以告訴React在這個Url下，我們想要渲染的組件有哪些。但是僅只於此還不夠，因為我們必須要告知React如何去解析Route組件的path，我們還必須在index.js下裝上發動機制。
```
import ReactDOM from 'react-dom';
import App from './App';

import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
  document.getElementById("root")
);
```
在index中，我們匯入**BrowserRouter**這個組件，緊接著將App組件包覆在其中，如此一來，Route就可以正式發動! 現在我們實際執行`npm run start`後，分別在URL/(Slash)後方加上welcome或者是travel就可以切換到對應的組件。不過我們這裡會有一個相當大的問題，那就是在切換URL的時候，網站都會重新載入，這是一個大問題! 因為這樣就不能稱作SPA跑車!

##### NavLink

方便起見，我們多加入一個NavHeader.js的檔案，如下:
```
import { NavLink } from 'react-router-dom';

const NavHeader = () => {
  return (
    <header>
      <nav>
        <ul>
          <li>
            <NavLink activeClassName="active" to="/welcome">Welcome</NavLink>
          </li>
          <li>
            <NavLink activeClassName="active" to="/travel">Travel</NavLink>
          </li>
        </ul>
      </nav>
    </header>
  )
};

export default NavHeader;
```
此處多出導引列(Nav)，匯入**NavLink**的組件後，我們用一樣的方式去使用它。這邊注意到NavLink路徑屬性是**to**，務必不要跟Route的path搞混。NavLink很重要的一點就在於它能夠防止網頁重新載入，也就是每一個NavLink都有**event.preventDefault**這個函數功能。

NavLink還有**activeClassName**供我們使用，它的功能主要在於讓開發者為當下的Nav套上CSS的效果。為了方便觀看，我們在index.css直接加入下方的CSS，雖然這不是一個很好的做法，一般來說，都會為有各自的CSS檔案或者是CSS Module的檔案。

```
header {
  height: 3rem;
  width: 100%;
  background: #eee;
}

ul {
  list-style: none;
  display: flex;
  padding: 0;
  margin: 0;
  align-items: center;
  justify-content: center;
}

ul li {
  padding: 10px;
}

a {
  color: black;
  text-decoration: none;
  margin: 0 15px;
}

a.active {
  color:crimson;
}

h2 {
  text-align: center;
}

```
實際執行的畫面會如下:
![nav](https://i.imgur.com/nslDBfa.jpg)
點擊Welcome或Travel，可以發現網頁並不會重新載入，Chrome開發者工具的Network也確實沒有傳輸的動作，成功解決剛才的問題。

##### Switch
Switch的用法很簡單，基本上想成是JS的Switch也完全沒有問題。我們將App修改成如下:
```
import { Route, Switch } from 'react-router-dom';

import Welcome from './components/Welcome';
import Travel from './components/Travel';
import NavHeader from './components/NavHeader';

function App() {
  return (
    <div>
      <NavHeader />
      <Switch>
        <Route path="/welcome">
          <Welcome />
        </Route>
        <Route path="/travel">
          <Travel />
        </Route>  
      </Switch>
    </div>
  );
}

export default App;
```
直接用Switch包覆住整個Route，Switch會選擇其中一個Route執行，只要URL**符合就不會再繼續執行**。比方說我們多出新的一個Route是關於旅遊國家:
```
import 同上

function App() {
  return (
    <div>
      <NavHeader />
      <Switch>
        <Route path="/welcome">
          <Welcome />
        </Route>
        <Route path="/travel">
          <Travel />
        </Route>  
        <Route path="/travel/:country">
      </Switch>
    </div>
  );
}
```
這個時候，如果想要存取的URL是/travel/japan就無法成功存取，因為會在/travel就被Switch攔截下來。要解決這個問題，則必須多給Route屬性**exact**，確保完全一樣才執行特定的Route。因此需要在travel後面加上exact，變成`<Route path="/travel" exact>`，才能夠執行country的頁面。


##### Link
現在要來講講關於Link，其實原本想著應該先講Link，但發現似乎要先講完前面的NavLink和Switch才比較不會搞混。眼尖的讀者應該有注意到在最後一個Route裡面有 : 的符號，這個符號的意思其實就是變數，將後方的文字視為變數，可以用來動態更動網址。我們將Travel改成下方的樣子:

```
const Travel = () => {
  return <div>
      <h2>Travel</h2>;
      <ul>
        <li>
          <Link to="/travel/japan">Japan</Link>
        </li>
        <li>
          <Link to="/travel/uk">UK</Link>
        </li>
        <li>
          <Link to="/travel/us">US</Link>
        </li>
      </ul>
    </div>;
};
```
這裡透過Link去連接，同樣跟NavLink一樣都有防止網頁重載的功能。Link和Route的區別在於Route符合目前URL會渲染裡面的組件，而Link用於切換當下URL頁面的組件，但不會影響到上層的組件，以這個例子來說就是Travel。接著我們還得再多新增一個Country.js的組件，如下:
```
import { useParams } from 'react-router-dom';

const Country = () => {
  const params = useParams();

  return (
    <div>
      <h2>Here is {params.country}.</h2>
    </div>
  )
};

export default Country;
```
特別注意這裡要引入useParams這個Hook，它的功用主要就是用來抓取URL的動態變數的部分，在這裡就是指:country。實際執行過後，大致如下:
![japan](https://i.imgur.com/RwAlISv.jpg)

##### Redirect
最後說說Redirect，其實就是為了避免跑到一些奇怪的頁面，自動轉跳到正常頁面，我們修改一下App:
```
import { Route, Switch, Redirect } from "react-router-dom";

import Welcome from './components/Welcome';
import Travel from './components/Travel';
import Country from './components/Country';
import NavHeader from './components/NavHeader';

function App() {
  return <div>
      <NavHeader />
      <Switch>
        <Route path="/" exact>
          <Redirect to="/welcome" />
        </Route>
        <Route path="/welcome">
          <Welcome />
        </Route>
        <Route path="/travel" exact>
          <Travel />
        </Route>
        <Route path="/travel/:country">
          <Country />
        </Route>
      </Switch>
    </div>;
};
```
在`<Route path="/" exact>`的情況下轉跳回/welcome的頁面，沒有太大問題，只是一樣要有exact才不會導致其他Route都無法使用。

***

#### 結語
今天講述一下關於React的Route，稍微釐清一下基本概念以及一些小要點。相信Route並不有這些功能，但至少今天會是一個好的開始，先打穩根基再去研究更複雜的部分，才不會失焦。希望這篇文章能夠幫到需要的人。

謝謝各位看到最後。

