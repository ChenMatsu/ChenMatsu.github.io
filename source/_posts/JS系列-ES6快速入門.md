---
title: JS系列 - ES6快速入門
date: 2021-05-05 14:40:50
tags: [javascript, es6, arrow-function]
categories: 
  - [Programming, Javascript]
thumbnail:
banner:
---
###  JS - ES6快速入門
> *The only true wisdom is in knowing you know nothing.* 
> *- Socrates*
####  JS語法釐清
今天重溫JS的基礎，徹底了解ES5與ES6間的差異，那麼廢話不多說，趕緊來看!
- 變數命名 let & const
- 箭頭函數 ES6
  - ES5函數
  - ES6函數
- 觀念釐清 Export & Import
- 類別與繼承 Class 
- 展開/其餘運算子 Spread & Rest
- 拆解 Destructuring
- 原始型態和參照型態 Primitive & Reference
- 結語

以下範例程式碼都可以在JSbin實際執行看看喔!不妨動手試試吧!
**JSbin: https://jsbin.com/?js,console**

***

####  變數命名 let & const
在ES6，var開始出現分身，就是let和const! 簡單來說，它們的功用都是用來定義變數，實際舉例:
`var number = 5;`
`let number = 5;`
`const number = 5;`
一般區別在於let所命名的變數是可變的，const所命名的變數則是不可變的。
其實還有Scope的問題，但暫且不討論那一塊。

***

####  箭頭函數 ES6
接下來比較大的改變莫過於箭頭函數，若沒有事先了解，肯定出現ES5和ES6是不同程式語言的錯覺。
##### ES5函數
首先先來看看ES5的函數寫法:
```
function say(name){
  console.log(name + " Hey! You cool!");
}

say('Johnny');
```
在ES5撰寫函數的時候，會先寫下**function**這個關鍵字建立函數，接著再寫上函數的名稱，以本例來說**say**就是函數的名稱，往後在呼叫這個函數時則要寫上**say**，而括號()內則填入參數，所以呼叫時我在這裡傳入**Johnny**，這就是ES5典型的函數呼叫方式。
實際在JSbin執行過後可以得到結果是`"Johnny Hey! You cool!"`
##### ES6函數
根據ES5的函數寫法，我們來改寫成ES6的箭頭函數:
```
const say = (name) => {
  console.log(name + " Hey! You cool!");
}
say('Johnny');
```
一般來說要將ES6寫成等同於ES5的程式碼都必須指派到某變數，同樣可以根據函數是否會更動而宣告為let或者是const。我們可以看到參數的傳遞寫在等號後方，接著才接上箭頭函數。好處在於簡潔的語法外，最重要的優點在於**this**關鍵字的範圍變得更加明瞭，而不會像ES5函數的this常常會超出我們所想。
同樣在JSbin執行過後可以得到結果也是`"Johnny Hey! You cool!"`

TIP:
**單一參數**，箭頭函數可以簡化成下方寫法:
```
const say = name => console.log(name + " Hey! You cool!");
```
**無參數或多參數**，括號()不得省略:
```
const say = () => console.log("Hey! You cool!");
const say = (fname, lname) => console.log(fname + " " + lname + " Hey! You cool!");
```

***

#### 觀念釐清 Export & Import
在專案當中，把程式模組化是一件相當明智的選擇。
首先模組化的檔案必須先匯出**Export**，才能由其他程式匯入**Import**後使用。
匯出Export有兩種方式，我們以ES6的函數為例:
--預設匯出--
`export default say`

--命名匯出--
`export const say = (name) => console.log(" Hey! You cool!")`
`export const name = "Johnny"`
預設匯出的檔案只有一個模組，以上例來說就是只匯出say這個函數，命名匯出則是會根據名稱匯出不同的模組，以上例來說就是匯出say這個函數外，還能夠再匯出其他函數或變數。

匯入Import則會根據匯出的方式對應:
--預設匯入--
`import say from './say.js'`
當然say可以當成alias任意命名，如下:
`import greet from './say.js'`

--命名匯入--
`import {say} from './say.js'`
`import {name} from './say.js'`
可以清楚看到若是預設匯入則可以任意命名，但若是命名匯入則必須指定要匯入的函數或變數等等。
總結來看，一個檔案只能包含一個預設匯出及數個命名匯出，因此若檔案包含數個模組或變數等，我們也可以一次匯入所有的模組如下:
`import * as all from './say.js`
存取則可以透過**all.say**和**all.name**存取我們匯出的模組或變數。

***

#### 類別與繼承 Class 
噹噹噹，Java跟Javascript就像是Ham跟Hamburger，除了開頭三個英文字其他都不一樣，但自從ES6加入了Class後，它們的相似性又提高了一點，對，就是那麼一點。對於熟悉物件導向程式語言的各位來說，肯定是一件再熟悉不過的事情，不過我們還是來看看JS的Class吧!畢竟三大框架Angular,React,Vue幾乎可以說是完全基於它的概念而發展出來。
關鍵字起頭不廢話:
```
class Fish {
  constructor(){
    this.species = 'fish';
  }
  swim () {
    console.log("swims...");
  }
}

const shark = new Fish();
console.log(shark.species);
shark.swim();
```
上方定義魚的類別，裡面有一個建構子與方法Swim。典型的類別寫法，同時也是比較**舊**的寫法。constructor可以理解為一開始實體化(初始化)時就具備的東西，以Fish類別來說，鯊魚剛生出來就有個名字叫做'fish'。
實際執行後顯示結果`"fish"`跟`"swims..."`

接著我們來看看繼承的範例:
```
class Shark extends Fish{
  constructor(){
    super();
    this.name = 'shark';
  }
  swim() {
    console.log("swims pretty fast...");
  }
}

const shark = new Shark();
console.log(shark.species);
console.log(shark.name);
shark.swim();
```
鯊魚這個類別繼承魚，鯊魚這次完成進化，游得非常快而且有個更酷炫的名字shark!需要特別注意的是，繼承父類別Fish必須要寫上super()才可以確保父類別屬性的建構子也經過實體化。
不過這樣看下來好像JS在class上沒有什麼獨到之處啊? No.No..No...
以上的程式碼都能改寫成更有ES6的味道:
```
class Fish {
  species = 'fish';
  swim = () => {
    console.log("swims...");
  }
}

class Shark extends Fish {
  name = 'shark';
  swim = () => {
    console.log("swims pretty fast...");
  }
}

const shark = new Shark();
console.log(shark.species);
console.log(shark.name);
shark.swim();
```
看到JS有多潮了嗎?大概就跟鯊魚一樣潮，海中霸主。可以注意到在JS裡的Class並不是純粹的Class而是經過簡化的Class，所以常常可以聽到有人說JS的Class是類Class也是有點道理在。因為ES6幫我們處理許多像是constructor跟super()，在JS這些都可以省略不寫，因此我們只需要專注在High-Level的部分，裝備穿好了，還不趕快出發嗎?

***

#### 展開/其餘運算子 Spread & Rest
ES6可以與箭頭函數與之比肩的大概就非屬於展開和其餘運算子了，讓我們來了解它們的各自功能。
**展開運算子(Spread Operator): 將陣列元素或者是物件屬性分割出來**
**其餘運算子(Rest Operator): 將一連串的函數參數合併到陣列中**

不需要懷疑，這兩個運算子放在一起講就是因為它們的寫法一樣都是**...**，沒錯就是點點點，想必大家看完心中也是點點點，但我們立刻來抽絲剝繭。
展開運算子其實相當好理解，就是將陣列分開再使用:
```
const oldYear = [2018, 2019, 2020];
const newYear = [...oldYear, 2021];
console.log(newYear);
```
我們可以了解到newYear第一個參數...oldYear會先將oldYear這個陣列本身切割開來成為單一元素，接著再建立一個新的陣列給newYear並且再加上後續的參數。
實際執行後顯示結果`[2018, 2019, 2020, 2021]`。
同樣地，我們也可以將展開運算子運用在物件上:
```
const fish = {
  species: 'fish',
}

const shark = {
  ...fish,
  name: 'shark'
};
```
實際執行後顯示結果
```
[object Object] {
  name: "shark",
  species: "fish"
}
```
可以發現fish的屬性一樣被添加到shark裡面，這就是展開運算子的功能，帶來意想不到的便捷性，尤其是在複製陣列或物件身上。

接著來討論其餘運算子:
```
const filterNums = (...args) => {
  return args.filter(el => el > 5);
}
console.log(filterNums(1,3,5,7,9,11,13,15,17));
```
我們在這個函數傳入其餘運算子(...args)，args可以任意命名，接著我們回傳args的內建函數filter。因為args在經過其餘運算子後已經轉換為陣列，因此可以套用陣列的內建方法，如:sort(), map(), slice(), splice()等等的方法。特別注意的是filterNums()並不能傳入一組陣列，會造成其餘運算子無法將陣列轉換成功而直接回傳空陣列。
實際執行後顯示`[7, 9, 11, 13, 15, 17]`
***

#### 拆解 Destructuring
藉由拆解我們可以很容易的去存取陣列或物件裡面的元素或屬性，我們實際來看點例子。
首先是陣列:
```
const breakMeUp = [2, 0, 2, 1, "is", "cool"];
const [a, b, , d, e] = breakMeUp;
console.log(a);
console.log(b);
console.log(d);
console.log(e);
```
透過直接拆解陣列可以對應到breakMeUp陣列裡面的元素，而不需要透過其他方式去索引。
實際執行後顯示
接著是物件:
```
const Shark = {
  species: 'fish',
  name: 'shark'
}

const {species} = Shark;
console.log(species);
console.log(name);
```
物件的拆解就像是一對一的對應，中括號{}內寫的變數能夠對應到物件的屬性，就能夠讀取，如果沒有對應的屬性則會回傳undefined.
實際結果:`"fish"和"Undefined"`

***

#### 原始型態和參照型態 Primitive & Reference
終於來到本日的最後一個段落，喘口氣~茶。
JS最重要的概念大概在於釐清原始型態(Primitive)和參照型態(Reference)，基本上數值、字串、布林值、Null、undefined都隸屬於原始型態，至於在JS常常操作的陣列與物件則是所謂的參照型態。
等等，什麼型態? 難不成我還得會開二、三檔，接著伸縮機關槍嗎? 不會可能真的打不出機關槍來。
首先是原始型態:
```
let num = 1111;
let num2 = num;
console.log(num);
console.log(num2);
num = 2222;
console.log(num); 
console.log(num2)
```
上述程式我們可以理解到何謂原始型態，簡單來說就是**參值**(Call By Value)，因此num在後續變更後並不會改動num2的值。可以理解為num2模仿num蓋房子，蓋完後即便num改建，num2的房子還是長的跟num之前的房子一樣。原理就是因為num2其實擁有自己的記憶體位置0x0f03。
實際執行結果顯示:`1111 1111 2222 1111`

```
const fish = {
  species: 'fish'
}
const shark = fish;
console.log(shark.species);
fish.species = 'animal'
console.log(shark.species);
shark.species = 'fish';
console.log(fish.species);
```
上述程式我們理解參照型態，簡單來說就是**參址**(Call By Reference)，既然它們住的地方都一樣，只要有人去改動房子的，那自然它們就會彼此影響，換句話說，fish和shark都參照到同一個記憶體位址，理解這一點後，就可以理解JS不論是展開、其餘運算子還是陣列內建方法，都會產生新的陣列的原因在，一方面是要避免副作用，試想如果不小心改動陣列，卻把最一開始的值都搞混了，那還需要繼承什麼呢? 可能哪天fish的種類變成dog都不一定對吧? 
實際執行結果顯示:`"fish", "animal", "fish"`

***

#### 結語
哇! 今天一個興起就把文章打好打滿，花了不少時間。其實會突然想打這篇文章主要是因為在網路上學React的課程，剛好講師複習JS一些比較需要去注意的改動,
像是箭頭函數、展開運算子、類別其實都逐漸讓JS在程式語言的世界裡稱霸，當然還是有許多很好的語言，但JS的強勢卻是無庸置疑，以前或許沒人敢說，但現在肯定大家都會認同JS是很強勢的語言，即便它是弱型態語言，仔細想一想JS根本就是扮豬吃老虎，有夠過分，但大家還是對它愛不釋手，我也是。

希望今天的系列文能夠快速幫助需要的人複習ES5.6間的改動，其實有些許改動牽連到ES7，有機會再拿出來討論，相信應該不會等上太久的時間。許多細節沒有講到，但我想理解基本概念，等到實務上遇到問題再去深入理解也不遲，畢竟要學的東西太多了!

謝謝各位看到最後!!













