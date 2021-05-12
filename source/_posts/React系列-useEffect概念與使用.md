---
title: React系列-useEffect概念與使用
date: 2021-05-12 10:43:20
tags: [react, javascript, component, useeffect]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
--- 
### Hooks家族 - useEffect
> *It is better to fail in originality than to succeed in imitation.*
> *― Herman Melville*

距離上一次寫useState的文章已經事隔些許日子，今天想來寫寫關於useEffect的事蹟。useState和useEffect在React當中非常有份量，甚至因為它們的出現改寫React的Coding Style。這幾天在研究useEffect，但卻又覺得自己似懂非懂，因此決定透過文章來整理自己所學，我想要試著放慢腳步，再繼續往前。那，我們開始吧!

- useEffect
  - useEffect使用方式
- 結語

***

#### useEffect
程式往往都會有許多的**副作用**(Side Effect)，useEffect這個Hook正是React開發團隊為了解決副作用的問題而研發出來的程式撰寫方式，就像是JSX是為了將JS和HTML合併的概念。

##### useEffect使用方式
useEffect大致上寫法如下:
```
useEffect(()=> {
  console.log("Hi! I\'m useEffect in Hooks Family!");
}, [])
```
我們可以注意到在useEffect這個Hook函式中有兩個參數，參數一是匿名函數，參數二則是一組陣列。如同先前所述，這個Hook是為了解決副作用的問題，那具體來說，我們應該如何在React專案中使用他? 我們一樣來看點範例。

```
import { useState, useEffect } from 'react';
const Hook = props => {
  const [email, setEmail] = useState('');

  const submitHandler = (event) => {
    event.preventDefault();
  };

  const setEmailHandler = (event) => {
    setEmail(event.target.value);
  };

  useEffect(() => {
    console.log("Hi! I\'m useEffect in Hooks Family!");
  }, []);

  return (
    <div className="hook">
      <form onSubmit={submitHandler}>
        <label htmlFor="email">E-mail</label>
        <input type="text" id="email" value={email} onChange={setEmailHandler} />
        <button type="submit">Submit</button>
      </form>
    </div>
  )
};
```
使用Hooks前先匯入，接著我們試著建立一個簡單的Email表單，並且給它一個state，到目前為止都和上一篇文章提的差不多。我們透過useState這個Hook來控管狀態，緊接著我們直接在setEmailHandler下方加上useEffect Hook。實際將這個組件匯入後執行後，可以在Chrome的Console裡面發現以下我們在useEffect列印的字串。

**補充：** event.preventDefault()是為了防止submit按鈕送出請求給Http後端伺服器而導致頁面重載

![console](https://i.imgur.com/LF9ZWGy.jpg)

我們發現useEffect執行過的痕跡，但是它究竟如何執行呢？我們試著在它給定的第二個陣列裡面填入我們的**email**狀態值。
```
useEffect(() => {
    console.log("Hi! I\'m useEffect in Hooks Family!");
  }, [email]);
```
同樣地，我們回到Chrome去執行，試著在欄位裡面隨意輸入。接著我們發現如下圖:
![console2](https://i.imgur.com/BSi2jE4.jpg)
useEffect會在Hook組件完成載入後執行一次，接著隨著我們輸入N個字元，就多印出N次訊息。這個結果我們可以理解useEffect這個Hook會根據陣列二的狀態值而改動，而這個狀態值指的就是email這個狀態(變數)的改變。


接著我們再將useEffect更改成以下的形式:
```
useEffect(() => {
  console.log("Hi! I'm useEffect in Hooks Family!");
  return () => {
    console.log("Return comes first!");
  }
}, [email]);
```
在開發者工具觀察實際在欄位輸入後的訊息:
![console3](https://i.imgur.com/aJlCI5X.jpg)

我們發現**return**所列印的訊息會**優先**於匿名函數內的訊息，我們現在可以清楚理解藉由useEffect，可以很輕鬆地去解決在撰寫React時可能出現的副作用問題。能夠透過useEffect去決定特定情況下的程式碼，降低程式邏輯的模糊與不確定性。

-重點回顧-
- useEffect用於處理程式的副作用
- 參數一是執行的函數
  - 函數內的return會優先執行
- 參數二是狀態陣列
  - 根據狀態的改變而執行useEffect內的函數
  - 若為空陣列則僅執行一次

***

#### 結語
本篇算是淺談useEffect，在useEffect身後還有useReducer的存在。礙於小弟的才疏學淺，僅能夠將最基礎的應用和各位讀者分享。useEffect往往還會搭配計時器的使用去處理問題，舉例來說，我們可以驗證Email欄位是否包含@符號，但我們不會想在每輸入一個字就對欄位執行驗證，而是在使用者輸入完畢後才進行驗證，在這樣的情況下，就需要配合計時器去處理。

未來若有任何想法想補充都會來更新這系列的文章，期望能夠將Hooks講述的更完善。

最後，謝謝各位的閱讀!
