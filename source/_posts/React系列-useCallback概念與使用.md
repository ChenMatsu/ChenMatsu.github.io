---
title: React系列-useCallback概念與使用
date: 2021-05-20 16:37:59
tags: [react, javascript, component, usecallback]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Hooks家族 - useCallback

> *Beware of the man who works hard to learn something, learns it, and finds himself no wiser than before.*
> *― Kurt Vonnegut, Cat's Cradle*

來到第六篇關於Hooks的文章，不能說自己真的很懂Hooks，但多多少少，不再對Hooks感到陌生。今天要來聊聊關於useCallback這個Hooks，光是在JS就可以常常看見回呼函數的使用，那麼這個useCallback又與平常我們認知的有什麼不同? 趕緊來看看。

- useCallback
  - useCallback使用方法
- 結語

***

#### useCallback
官方精闢解析
**useCallback: Returns a memoized callback.**
useCallback會回傳記憶化的回乎函數，這邊要注意**Memoization**不等同於**Memorization**。有機會我們再來詳細探討這兩者間的區別。

所謂的記憶化(Memoization)是一種用於提升電腦程式速度的**優化技巧**，主要的概念是**儲存複雜函數結果**並且當相同結果出現時透過**快取**(Cache)存取。講白就是犧牲儲存空間，去提升電腦性能。

我們實際來看如何使用useCallback於React中。

##### useCallback使用方式
useCallback定義如下:
`const memoizedCallback = useCallback(() => { memoizedFn(a, b)},  [a, b])`
透過useCallback這一個Hook，可以直接將要記憶化結果的函數存取在memoizedCallback中，並且只有在a, b會影響memoizedFn函數的時候才會重新執行。

我們實際看看例子:
```
import {useState, useEffect, useCallback} from 'react';

function App() {
  const [isLoading, setIsLoading] = useState(false);

  const fetchHandler = useCallback(() => {
    setIsLoading(true);
    fetch('http://dabaseFetch').then(res => console.log(res));
  }, []);

  useEffect(() => {
    fetchHandler();
  }, [fetchHandler]);
}

export default App;
```
首先看到App中的程式碼，我們總共引入三個Hooks，其中兩個先前都是有討論過的Hooks。在這一段程式碼當中，我主要想要去抓到某些**資料**，可以看到fetchHandler函數裡面有個JS內建的**fetch**方法(API)，fetch就是拿來操作HTTP的各種請求，**預設**的情況下會是HTTP請求的**GET**。因此當fetchHandler執行時就會發送一個GET請求給伺服器，伺服器則會回傳某特定格式，而fetch這個API回傳的一定是Promise。

回到主題上，我們透過useEffect去更新fetchHandler，每當App渲染時，useEffect就會執行一次，接著再根據fetchHandler去執行，但是這邊就有一個很大的問題點是源自於React本身。

對於React來說，每當**重新渲染**時，所有的組件**都會是全新的組件**，不一樣的組件。什麼意思? 當我們實際執行React專案時，在我們重新載入的那一剎那，所有的組件都是新的組件，不同於先前所渲染的組件。換句話說，以我們fetchHandler來說，即便執行的名稱與內容都完全相同，React也會視這兩個fetchHandler為不一樣的函數，這一切都是源自於JS物件Call by Reference的本質而延伸出來。

因此當我們沒有透過useCallback去記憶化fetchHandler這個函數，就會發生不斷發出GET請求的慘劇。假如我們將程式碼改成如下:
```
import {useState, useEffect, useCallback} from 'react';

function App() {
  const [isLoading, setIsLoading] = useState(false);

  const fetchHandler = () => {
    setIsLoading(true);
    fetch('http://dabaseFetch').then(res => console.log(res));
  });

  useEffect(() => {
    fetchHandler();
  }, [fetchHandler]);
}

export default App;
```
也就是將fetchHandler的useCallback刪去後，實際執行相似的程式碼就會得到類似於下方的結果:

![infi](https://i.imgur.com/1ey45Lq.jpg)

可以在開發者工具發現fetch會不斷的發送請求到伺服器，這是非常嚴重的問題。但這個嚴重的問題，可以簡單地透過useCallback去解決。我們再度回到fetchHandler這個函數。

```
const fetchHandler = useCallback(() => {
  setIsLoading(true);
  fetch('http://dabaseFetch').then(res => console.log(res));
}, []);
```
概念和useEffect一樣，[]代表組件渲染完成後會執行一次。useCallback則會將箭頭函數後的所有函數都記下來指派給fetchHandler。這時候，每當有任何組件重新渲染，fetchHandler不會重新被渲染一次，因為已經透過Hooks將fetchHandler記憶化，因此不會出現fetchHandler不斷重新執行的問題，因為對於React來說，現在的fetchHandler都是同一個函數。

最後我們只需要確保useEffect在處理fetchHandler時所產生的副作用，而副作用就是當資料更新時，必須重新將資料庫添加進去的資料再度透過GET方法去獲得資料就結束這一連串的動作。
```
useEffect(() => {
  fetchHandler();
}, [fetchHandler]);
```
特別注意到useEffect的依賴只需要指向fetchHandler函數而不需要()，如果加上()代表要立刻在這一行執行，這又是另外一齣慘劇了...

***

-重點回顧-
  - React每次渲染都會是不一樣的程式碼，即便長得一模一樣
  - useCallback用於記憶化函數
  

#### 結語
今天簡單說一下useCallback，我想概念上還算挺好理解。不過如何正確使用就又是另外一門學問，不過先搞清楚它們各自的目的才不會摔的滿頭包。

這幾天心情一直很糟，不論是學習上還是生活上，真的可以說諸事不順，也不清楚要怎麼說出來比較好，只好像現在一樣打成文章，或是寫在日記上。但即便如此，好像還是悶在心裡一樣，可能是因為自己能力太差吧，覺得自己什麼都做不好，而現在又因為急於能夠找到一個落腳處，而更加心急如焚。現在都會回想大學不好好學習才會落的這個下場也算是自作自受，也有可能只是我一直往死裡鑽吧。

其實我還是不清楚程式是不是自己想走的一條路，我覺得我自己沒有天分，可是仔細一想，自己到目前為止，也不過一個月不中斷地在練習Coding，但是昨晚看完龍櫻2，卻又覺得自己都沒有好好思考**ものごと**的本質，慚愧不已。說真的，別人都在進步，為什麼我好像一直在原地踏步。心好累。

