---
title: React系列-useContext概念與使用
date: 2021-05-13 13:45:45
tags: [react, javascript, component, usecontext]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Hooks家族 - useContext
> *Believe in yourself. You are braver than you think, more talented than you know, and capable of more than you imagine.*
> *-Roy T. Bennett*

昨晚聊聊關於useEffect的使用方式，今天要來介紹管理變數的問題。React發展得相當迅速，常常會看到不同的寫法，卻能達到一樣的結果。對於初學者的我來說，其實會常常搞混，反倒有點困擾，但是一想到當自己完全釐清各種不同寫法，能夠在適當的時機，選擇最適當的方式去處理同樣的問題就覺得很有意思，就好像身懷各種武藝。那，我們開始吧!

- useContext
  - useContext使用方式
- 結語

***

#### useContext
我們都對於React的運作方式有一定程度的理解，對於props的傳遞，我們更是了解。我們不僅可以將props往子組件傳遞，甚至能夠從子組件傳遞到父組件當中。什麼意思? 請看下圖:
![app](https://i.imgur.com/AaPZaie.jpg)

假設一個購物網站，App分別有兩個子組件，一個是Login，另外一個則是Cart。當我們想要將使用者的購物資訊與清單分別傳遞到Cart，再由Cart將資料傳遞給Cartlist，最簡單的作法就是將User的資料一步步往上prop到App，再由App一步步往下Prop到Cartlist，如下圖所示:
![app2](https://i.imgur.com/gFoP6zy.jpg)

沒錯，這是最有React風範的方式，也是官方文件最先告訴我們的實作方式。甚至我們可以利用**提升狀態**(Lifting the state up)的方式去將我們想要傳遞的資料，放置在子組件共有的父組件上，在範例中，就是放置在App身上。

乍看下，這的確是解決方式其二，但是仔細想想，有可能我們的App不會用到User的任何資料，甚至若是我們的組件變得更多，中間有其餘的組件是不需要使用到User的資料，那這樣不是很難管理資料嗎? 我們現在希望的是有個方式可以直接將User的資料指向Cart，如同下圖的概念:
![app3](https://i.imgur.com/x6Dm2gV.jpg)

這個時候useContext就會跳出來說: 選我!選我!選我! 我們先來看看useContext的定義與概念。

##### useContext使用方式
useContext的定義大致上官方定義是: 接收一個Context Object(React.createContext回傳的值)並回傳。是不是有點難懂? 我也這麼認為，對於直接下手Hooks來說，其實不太好理解，因為底層包覆的東西有點多，但我們還是試著努力看看!

useContext的長相大概如下所示:
`const context = useContext(AuthUser)`
我們設立一個context的變數用於儲存useContext回傳的值，在useContext裡面則寫入一個**內文物件**(context object)。
這個內文物件在我們的範例包含AuthUser的值，換句話說，我們會透過React.createContext建立AuthUser這個內文物件，看看下方:

```
<!-- In AuthContext.js -->
import React from 'react';

const AuthUser = React.createContext({
  isLoggedIn: false,
  onLogin: (email, password) => {},
  onLogout: () => {}
});
```
要使用createContext函數必須匯入React，這邊應該沒有太大問題。我們可以看到說AuthUser是一個內文物件，一般來說，我們會在source的資料夾下建立一個store的資料夾負責統一管理資料，盡量讓資料和組件邏輯隔離開來，但當然，我們也可以直接在AuthContext檔案裡面直接更改Provider寫入共用的方法，讓AuthUser變得更完整:

```
<!-- In AuthContext.js -->
...上方的程式碼...

export const AuthContextProvider = props => {
  const [isLogged, setIsLogged] = useState(false);

  const loginHandler = () => {
    setIsLogged(true);
  }

  const logoutHandler = () => {
    setIsLogged(false);
  }

  return 
    <AuthContext.Provider 
      value=
      {{
        isLogged, 
        onLogin: loginHandler, 
        onLogout: logoutHandler
      }}>
        {props.children}
    </AuthContext.Provider>
}

export default AuthUser;
```

在AuthUser內文物件中，改寫加入是否登入的狀態。
我們要注意到createContext本身會有兩個子組件分別是
- Provider組件: 傳遞內文物件的資料
- Consumer組件: 接收內文物件的資料

這也是為何我們會看到在AuthUser出現Provider，因為在這邊我透過props一次傳入內文物件的物件，物件包含isLogged的狀態以及登入登出的方法。可以注意到Provider中間有props.children，還記得我們提到children的那篇文章嗎? 透過這一點，我們可以知道Provider包覆的組件都可以接收剛才透過props的資料。

回到圖表，我們要在User和Cart使用到關於User的資訊和方法，現在我們可以統一在App處理，如下:
```
<!-- In App.js -->
import {useContext} from 'react';
import AuthContext from './components/store/AuthContext';
import Login from './components/User/Login';
import Cart from './components/Cart/Cart';

const App = () => {
  const ctx = useContext(AuthUser);
  
  return (
    <>
      {!ctx && <Login />}
      {ctx &&＜Cart />}
    </>
  );
}

export default App;
```
因為User是在Login下方，所以我們此處匯入Login，接著再透過Login傳遞到User。這邊在顯示組件會先檢驗是否已經登入，如果已經登入就會顯示Cart的組件，若沒有登入則會顯示Login的組件。透過這樣的方法，我們同時有把**提升狀態**也有將**資料隔離**開來的感覺，而不需要一層一層傳遞，像洋蔥一樣撥開我們的組件。

我們到目前為止，檢視如何使用useContext的方法。我們在使用useContext先前必須建立內文物件才能夠使用這個Hook，目前還沒能直接看到Cart使用User的感覺，但其實我們已經朝向這一步邁進。

在以往沒有useContext這個Hook的情況下，我們在建立React.createContext後，必須在每一個需要使用到某個內文物件的組件裡面，去接收(Consumer)這些資料，換句話說，我們沒辦法直接透過指令變數去將資料傳遞到組件，程式碼大概會像下方這樣:
```
<!-- In Cart.js -->
import AuthContext from '../../store/AuthContext';
import CardList from './CardList';

const Cart = props => {
  return (
    <AuthContext.Consumer>
      {(context) => {
        <CardList />
        <Button onLogout={onLogout}>
      }}
    </AuthContext.Consumer>
  )
};

export default Cart;
```
大致上會像上方這樣的寫法，需要多寫Consumer這一段語法糖，去將組件包裹起來。在沒有Hook的出現確實是一件再正常不過的事情，但是現在有了useContext，實在是沒有理由將程式碼寫的過於繁瑣，畢竟React是屬於宣告式，若能減輕開發網站的負擔，沒有理由堅持使用上方的寫法。但，寫程式沒有對錯，這些都是非常棒的方法!

-重點整理-
- useContext接收內文物件
  - React.createContext建立內文物件
    - Provider子組件傳遞資料
    - Consumer子組件接收資料

***

#### 結語
今天主要講述useContext的使用方式，算是非常粗淺的概念。要在這裡像各位讀者說一聲抱歉，希望未來我能夠對React更加熟稔，蠻擔心自己寫的不夠清楚或者是寫錯，但即便如此還是想要挑戰自己試試看，以教學的角度，來檢視自己學習的成效。若有誤，請不吝於糾正。

最後，謝謝看到結束的各位。


