---
title: React系列-useState概念與使用
date: 2021-05-09 11:37:12
tags: [react, javascript, component, usestate]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Hooks家族 - useState
> *The measure of intelligence is the ability to change.*
> *- Albert Einstein*

今天要來談談Hooks家族的第一位成員useState。Hooks的出現使React更加脫穎而出，以往處理State時都必須透過建構、綁定去撰寫，而且每一個步驟都必須明確，但在今天Hooks都幫我們偷偷處理完成，我們只需要告訴React我們想做什麼，那就是Hooks的強大。

- useState
  - 幕後推手JSX
  - 鬼鬼祟祟State

*** 

#### useState
認識useState這個Hook前，我們先來理解一下JSX。

##### 幕後推手JSX
我們看看下方這段程式碼:

![calculate](https://i.imgur.com/TeKdMCu.jpg)

這段程式碼裡面有一個組件**Count**，我們給它一個變數count用來倒數，接著把count傳到HTML的h3元素上。可是問題來了，我現在想要做的是從10開始倒數，這樣一來我不就永遠不能倒數了嗎? 可能這段程式碼本身有問題，那我們試著把程式碼更改一下，用JS的思維寫它。

![calculatejs](https://i.imgur.com/8X8RPGG.jpg)

對，用迴圈去改變值似乎很合理，但是等等，為什麼一堆紅標? 那自然是因為我們的語法出問題，可是這在JS裡面不應該有問題啊! 問題出在我們並不是在寫JS。因為我們沒辦法用這種方式將JS和HTML合併在一起，我們可以選擇使用HTML與JS，但就是沒辦法單純使用JS去達到倒數的結果，除非我們用**JSX**的方式寫。

提到JSX，就會想到React。我們在React基本概念文章有提到React的兄弟JSX，但是我們並沒有太深入研究它的由來與經過，只知道它負責轉譯瀏覽器看得懂的程式碼。

JSX起初構想就是想在JS檔案裡面去撰寫HTML，並且只要使用唯一的HTML檔案作為埠口去連接。若想編譯JSX就必須在每一個JS檔案匯入React，像這樣子`import React from 'react'`，現在我們沒有匯入是因為在下`create-react-app`指令時，已經將React編譯JSX的函式庫都匯入到每一個JS的檔案。往後，我們才能順利編譯JSX的檔案，那問題回到最一開始，我們要如何在**JSX倒數**?

##### 鬼鬼祟祟useState
要在JSX裡面**控制數值同時又控制介面**，就比需仰賴React核心概念其一的State。所謂的**狀態**(State)就是一個組件的各種樣式，比方說有兩顆螺絲，它們都叫做螺絲，一個是比較大的螺絲，一個是比較小的螺絲，它們的長寬高都不一樣，這時候長寬高就是它們的狀態。

若我們想要改變狀態，就要去更改State。但我們並不能夠直接改變變數的值，因為React不會因為更動變數的值改動組件。這時候useState就出場了!

首先我們把Count程式碼改成下方這個樣子:

![countdown](https://i.imgur.com/hju2UOF.jpg)

我們在return前加入了`const [count, setCount] = useState(10)`，這就是典型的使用useState的方式。useState有三點要記住的是，useState會回傳**一個陣列**，陣列裡只有兩個元素，第一個元素是**初始值**，第二個元素則是**更新初始值的函數**。ES6版本釋出後，我們在宣告時可以直接將陣列元素拆解，這就是所謂的解構賦值(Destructuring)，講白就是單純拆開陣列對應的元素順便給它們變數名稱。我們在ES6快速入門那篇文章也有提到這部分，若想回顧可以點一下下面的連結。

解構賦值: https://reurl.cc/DvzmzR

接著可以注意到我們在h3下方加入一個按鈕，單純只是因為要去觸發倒數這個事件。特別注意到，為了區分純JS和JSX，JSX命命往往會用`onClick, className`的方式去命名，是為了避免跟JS關鍵字對撞的關係。

實際打開瀏覽器，我們就可以開心地倒數了。

![final](https://i.imgur.com/QjQdPlm.jpg)

-重點回顧-
- JSX就是HTML和JS的混合體
- React不因變數而更動組件
- useState回傳陣列
  - 第一個參數是初始值
  - 第二個參數是更新函數

***  

#### 結語
這篇文章對於使用useState講的非常淺，但若要快速上手使用這個Hook，我想沒問題。組件有兩種寫法，一個就是目前文章範例的函數寫法，另外一種寫法則是使用類別(Class)，同樣也是受惠於ES6才出現的寫法。不過在Hook出來前，類別寫組件在調整狀態的時候還必須透過建構子、綁定才能夠去實現更新狀態。甚至還必須去顧及React的生命週期，Mount來Mount去的。但是現在有Hooks，很多情況下，其實並不需要考慮到這麼細，幾乎可以說是減輕React工程師極大的負擔，卻能做到更多的事情，要說是福音也不為過。

useState寫法相當多元，除了上述最常見的以外還有其他方式可以去達到同樣的效果，甚至用物件一起打包所有的狀態等等。礙於小弟學識淺薄，就先在此停筆。未來對於React有更深入理解，會再來補充現在沒能提到的部分。

最後，謝謝看完這篇文章的各位，希望對你們多少有點幫助就好了。










