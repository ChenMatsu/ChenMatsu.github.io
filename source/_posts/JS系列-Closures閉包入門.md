---
title: JS系列-Closures閉包入門
date: 2021-06-03 20:02:43
tags: [javascript, closures]
categories:
  - [Programming, Javascript]
thumbnail:
banner:
---
### Closures閉包入門
> *Faith is about doing. You are how you act, not just how you believe.*
> *― Mitch Albom, Have a Little Faith: a True Story*

最近在用React寫組件時，突然想到閉包(Closures)的使用。具體原因已經有點想不太起來，只記得當天研究閉包一段時間。上一篇文章已經是五月底的事情，時間過的很快，不知不覺也在公司的案子裡埋頭三天多，不過進度很緩慢，心情也是有點忐忑，不確定自己能不能好好完成，也不確定一路走來是對是錯。回歸正題，今天就來探究一下閉包的使用吧!

- 何謂閉包
  - 基本閉包運用
  - 實際閉包運用
- 結語

***

#### 何謂閉包
若沒有特別研究，可能會以為閉包是什麼神奇的東西，它很神奇卻也不神奇。在撰寫JS的日子中，我們很常就會運用到閉包的概念。

先來看看MDN關於閉包的敘述: 
**A closure is the combination of a function bundled together with references to its surrounding state.**

個人而言，第一次讀到這段話的時候似懂非懂，但唯一可以確信的是，某函數可以參照到它周圍的狀態，至於這個周圍如何判斷就需要透過實際範例來討論，這就好像讀一篇文章，我們必須透過上下文來判斷一句話真正的意圖，程式語言當然也不例外。

##### 基本閉包運用
首先來看一個簡單的閉包範例:
```
let findNinja = 'ninja';

const trackNinja = () => {
  if(findNinja === 'ninja'){
    console.log('We find the ninja!');
  }
};

trackNinja();
```
上述是很常見的函數，我們單純宣告一個全域變數以及函數，但若依照**函數可以參照周圍的狀態**，則其實我們正在使用閉包，卻完全沒有發現。不過僅僅是這樣無法理解閉包的奧義，接著看:
```
const findNinja = () => {
  let ninja = 'Matsu';
  const catchNinja = () => {
    console.log(ninja);
  }
  return catchNinja;
}

let realNinja = findNinja();
realNinja();
```
這次的範例特別的地方在於return catchNinja會先在catchNinja函數執行前先執行，因此若真是如此return過後findNinja就應該找不到Matsu忍者，同樣也無法列印出Ninja的名稱，但是在實際執行後，卻可以找到Matsu忍者，這就是閉包在展現身手的時刻。可是似懂非懂? 我們一步一步解釋，首先當realNinja被指派findNinja函數後參照到catchNinja函數，catchNinja則會建立閉包產生一個泡泡去包住範圍內的變數，而可參照的範圍則是findNinja內的所有變數。我們再看另外一個基本範例:
```
let ninja = 'matsu';
let ninja_hide;

const findMoreNinja = () => {
  let realNinja = 'Matsu';

  const moreNinja = () => {
    if(ninja === 'matsu') {
      console.log('I found ninja matsu!');
    }
    if(realNinja === 'Matsu') {
      console.log(realNinja, 'I found real ninja Matsu!');
    }
  }
  ninja_hide = moreNinja;
}

findMoreNinja();
ninja_hide();
```
先後執行findMoreNinja和ninja_hide後，可以發現ninja_hide參照到findMoreNinja裡頭的moreNinja函數，照理說在findMoreNinja結束後，就無法再取得moreNinja的內容，但我們卻能夠取得，換句話說，ninja_hide()本身建立起新的閉包!

***

#### 結語
今天簡簡單單講解閉包的存在，其實閉包可以更加的複雜，只是透過深入理解日常閉包存在的方式，在撰寫JS的過程中便能多留意這個概念，往後在撰寫網站或應用程式時，就更夠理解JS背後運行的方式。學習框架更需要理解這些基本的概念，當開始談起生態圈與優化時，沒有JS基礎的底子，很難去真正釐清程式寫不好的原因在哪裡。

我覺得自己的基礎不是很好，希望能夠透過一篇又一篇的文章，一天一天的進步。今天為了透過Git Pages展示UI給公司的同事了解狀況，研究兩個帳號要如何在本地端切換，數小時過後才終於釐清SSH的使用方式，說真的都能夠再寫成另外一篇文章，很有趣的是，在五月初，剛架好部落格時就遇到同樣的問題，當初是打算要透過另外一個Github帳號去重新實作架設Hexo卻在SSH上面碰壁，時隔一個月，在今天才真的理解SSH的使用方法...，突然在IT邦看見其他大神的文章，才頓悟他們的文章在寫什麼，只能說自己真的還太菜!

希望今天這篇文章能夠對想初步理解閉包的讀者有幫助，謝謝。


