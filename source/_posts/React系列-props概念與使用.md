---
title: React系列-props概念與使用
date: 2021-05-08 07:50:27
tags: [react, javascript, component, props, children]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Props講解與使用
> *Do what is right, not what is easy nor what is popular.*
> *- Roy T. Bennett*

- Props的概念
  - Props使用實例
  - Props也有孩子?!
- 結語

早上風很大，天氣很好，適合寫寫關於props的故事。那，我們馬上來說說props吧!

***

#### Props的概念
我們在第一篇React系列文提到React以組件的方式組成，單一個組件就好比合成金屬一樣，由HTML、CSS、JS三種元素所組成。但是當今天有數十、百個組件的時候，如何去把這些「金屬」組裝在一起，就是一門學問!

這時候就是Props(Properties)登場的時候了!React提供我們把每一個組件鑲嵌在一起就是透過使用Props。我們來直接看範例。

##### Props使用實例
建立專案的指令
`npx create-react-app <project name>`

延續我們系列文第一篇所建立好的專案，大致上會有下面這些檔案:
- first-react-project -> 專案資料夾名稱
  - node_modules -> NPM安裝後的套件
  - public  -> 放置圖片和唯一的index.html
  - src -> 放置組件的資料夾
  - package-lock.json -> 鎖定專案某版本描述檔
  - package.json -> 專案規範描述檔

首先我們把APP裡面的預設程式碼更改成像下面一樣:

![root](https://i.imgur.com/INabReS.jpg)

可以看到App.js這個檔案變得相當乾淨，我們通常稱App這個檔案為**根組件**，也就是說它會擁有許多的子組件，最後將這些子組件拼湊在一起。在Cmd輸入`npm run start`就可以看到本地端的瀏覽器出現「我是根組件!」這段文字。

![rootme](https://i.imgur.com/CihXdJh.jpg)

這就是我們第一個組件，接著我們在**src**檔案夾建立兩個資料夾分別是**components**和**styles**。首先我們在components的資料夾內建立叫**Calculate.js**的組件。組件其實都是由函數構成，因此一定是以函數起頭，而**函數名稱同檔案名稱**。特別注意到，必須把這個組件匯出才可以在其他組件使用。我們目前會在App.js這個根組件使用Calculate.js這個子組件。

![cal](https://i.imgur.com/KQjHmho.jpg)

這就是構建組件的第一步，緊接著我們在App.js匯入Calculate組件。

![appcal](https://i.imgur.com/IMl9SyZ.jpg)

我們匯入Calculate組件後，再將組件用HTML的形式包覆在App根組件當中，切換到瀏覽器後會發現多了一個12的數字。

![num](https://i.imgur.com/QdR9Ck3.jpg)

我們現在已經完成最最最基礎的組件使用方式。可以看到我們最後順利地使用Calculate這個組件。可是等等...，說了這麼多，到底跟props有什麼關係? 對，到目前為止都還沒有關係，因為必須先把組件嵌在一起的方式解說後才能理解使用props。總得先學會發動車子，才能開始練習開車對吧?

我們在App裡面輸入一組物件，這很Hard Core，不過沒關係，我們只是為了示範這個過程。請注意到return前的程式碼是寫JS，return後的程式碼則是寫JSX。接著我們把propme這個物件傳入給Calculate這個子組件。

![propass](https://i.imgur.com/D3K0ooW.jpg)

我們藉由JSX的形式給定一個變數名稱**value**，把在App組件裡的物件propme透過所謂的**props**方式傳遞給**Calculate**，接著才能回到**Calculate**組件裡面去接收這個資料。

回到**Calculate**組件(函數)後，注意到參數props可以任意命名，但一般會以props命名。為了讀取從**App**傳來的資料，必須在參數後方寫上我們在**App**設好的變數才能夠去存取**App**的物件屬性。

**props本身是一個物件**，因此可以容納相當多元的資料。

![cals](https://i.imgur.com/RWDDhmo.jpg)

我們可以看到**Calculate**組件讀取傳遞過來參數的方式，就跟讀取物件屬性一樣，差別只在於要先讀取到在**App**的變數名value。

![ass](https://i.imgur.com/w0pQcO9.jpg)

成功傳遞資料到其他組件!! 現在我們終於講解完props的概念和使用方式，接著要來討論有點神祕的children。

什麼? props也有孩子? 還真的有!

***

#### Props也有孩子?!
當我們想給我們組件的孩子保護，這時候children就出現了! 啊不是，在說什麼...。
所謂的**props.children**就是用外殼(nutshell)包覆子組件，詳細我們一樣來看例子比較清楚。

首先我們在components裡面再新建立一個組件叫做Card，Card比較類似統稱，只要有外殼功能的組件都會這樣命名。看看下圖:

![card](https://i.imgur.com/gWzwD0Q.jpg)

我們看到**Card**同樣透過props傳入資料，宣告一個const用來儲存class的名稱把card的css類別傳到包覆住的子組件，接著在JSX檔寫入**props.children**這段程式碼。**children是保留字**，每一個組件在建立時都會預設好。children獨到之處，在於它的使用方式，而不是單純的寫成組件而已。我們來看看它如何使用，看看下圖:

![calcard](https://i.imgur.com/xdN1T2r.jpg)
我們把div替換成Card，Card是客製化的組件。我們順便給Calculate本身的這個div區塊一個class名稱calculate。現在重新檢視瀏覽器，並不會有任何改變，因此我們可以給Card一點點樣式。選定styles資料夾裡頭，新增card.css這個CSS樣式表，並且給它簡單的框線以及背景顏色。
如下:

![cardcss](https://i.imgur.com/E1k72fY.jpg)

記得要在Card這個元件去匯入樣式表`import '../styles/card.css'`才會套用。

我們緊接著來檢視一下是否成功使用Card了呢? 打開你/妳的瀏覽器看看吧!

![final](https://i.imgur.com/bztgqIY.jpg)

哇! 成功套用到Calculate這個組件身上，至於組件外的則沒有套用成功，因為「我是根組件」那段文字是寫在App裡面。如果要套用到整個App，則修改App的div為Card就可以成功套用這段文字。各位讀者不妨自己動手試試看，一定會覺得很有意思。

-本日回顧-
- props使組件彼此傳遞資料
- children統一管理組件共同性質

***

#### 結語
今天主要把昨日學到關於props的概念和使用方式和大家分享，我相信我目前理解的程度還非常的淺，若有任何問題歡迎告訴我。

我在剛開始學children覺得有點抽象，但在實際動手做過幾次後，就對於它的概念比較清楚一些。不過我想children應該不會只有這點使用方式，這樣的話就太小看它了!以後若遇到進階的使用方式或有趣的應用再和大家分享!
 
寫文章總會花上數小時，但是很有趣，我們下回再見!
謝謝各位，祝大家都有美好的周末!




