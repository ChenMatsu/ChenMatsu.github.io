---
title: React系列-useRef概念與講解
date: 2021-05-16 11:30:27
tags: [react, javascript, component, usereducer]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Hooks家族 - useRef
> *We write to taste life twice, in the moment and in retrospect.*
> *― Anais Nin*

昨晚提及useReducer，說說它其實就是useState的強化版。今天要來講述另外一個不錯用的Hook，useRef。不學不會怎樣，但學了useRef，Code都變得簡潔俐落。我們趕緊進入正題!

- useRef
  - useRef使用方式
- 結語

***

#### useRef
同樣地，我們一樣先來看看官方的文件敘述:
**useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue). The returned object will persist for the full lifetime of the component.**
從Ref這個英文縮寫來看，可以簡單猜測出這個Hook可以用來參照某樣東西。上述官方解釋useRef會回傳一個傳入參數後初始化current屬性的**變動參照物件**，這個物件將持續在整個組件的生命期。換句話說，useRef會去對應到DOM的節點，只要節點一變動就會跟著變動。useRef特別的地方在於它所擁有的屬性**current**是可變動的值，但current變動並不會造成React重新渲染組件。

##### useRef使用方式
useRef使用方式:
`const inputRef = useRef()`
在某些情況下，我們會想要直接去存取某些DOM的元素，以此直接控制這些DOM元素本身的一些屬性。再者，我們或許會希望能夠直接在父組件控制子組件的DOM元素，這時候就會需要使用到useRef。先從最簡單的範例開始:
```
const Ref = () => {
  const inputName = useRef();

  const [name, setName] = useState('');

  const nameSet = () => {
    setName(inputName.current.value);
    console.log(inputName.current.value);
  }

  return (
    <div>
      <label htmlFor="name">Name</label>
      <input type="text" id="name" ref={inputName} value={name} onChange={nameSet}/>
    </div>
  )
}
```
在上述的例子當中，我們透過useRef建立一個用於儲存參照物件的常數變數。為了讀取input元素的值，我們將inputName設為input的ref屬性值。目前我們已經可以直接透過current屬性物件裡的value去抓取input這個欄位的值，同時我們設定一個state去抓取使用者變動過後欄位的值，當然我們也可以抓取name這個狀態的值，但useState並不是本篇的主角，就留給各位自行嘗試。

現在展示完最簡單的範例，我們接下來討論如何從父組件，去操作子組件的DOM。
Step1: 修改Ref.js
```
const Ref = React.forwardRef((props, ref) => {

  const [name, setName] = useState('App Mount Start');

  const nameChange = () => {
    setName(ref.current.value);
    console.log(ref.current.value);
  };

  return <div>
      <label htmlFor="name">Name</label>
      <input type="text" id="name" ref={ref} value={name} onChange={nameChange} />
    </div>;
});
```
我們先將原先的程式碼修改成上方這樣，特別注意在組件函式新增的**React.forwardRef**，這段程式碼可以告知React將Ref這個組件視為可參照DOM的組件，包覆裡面的組件可以經由傳入ref供其他組件去參照定位。接著我們來到根組件。
Step2: 修改App.js(父組件)
```
const App = () {
  const inputRefByApp = useRef();

  useEffect(() => {

    // console.log(inputRefByApp.current); -> <input type="text" id="name" value="App Mount Start">
    // console.log(inputRefByApp.current.value); -> 'App Mount Start'
    inputRefByApp.current.focus();
  }, [])

  return (
    <div>
      <Ref ref={inputRefByApp}/>
    </div>
  );
```
匯入Hooks的部分就不再贅述。我們在根組件新增一個ref，這個ref基本上和一開始在Ref.js建立的概念是一樣。只是要特別注意必須將inputRefByApp傳入Ref這個組件。Ref組件必須是React.forwardRef型式的組件，才會擁有ref的屬性，這也是為什麼我們必須特地宣告的用途。測試過後，我們發現App根組件確實可以操作Ref組件的DOM，這裡指的是input元素。至於App，我們緊緊只是透過useEffect去幫我們自動在組件載入完成後(參考React系列-useEffect)，focus在Ref組件的input元素上，不妨試試看列印出inputRefByApp看看，看看究竟裡頭藏著那些秘密!!

***

#### 結語
今天講解useRef，因為我自已真的想要對這個Hook有更深的理解。一開始學習到的時候，真的是一頭霧水，或者說現在也還是半頭霧水吧?(笑)。有時候都會想自己是不是真的瞭解一件事情，總是在打文章的過程中甚至舉範例的過程中，反覆去仔細思考自己打出來的這段話是否正確。既擔心自己不是真的懂，但真正擔心的是誤導他人，或許在不遠的將來，會回來嘲笑現在的自己也說不定。我確實很擔心自己弄錯，但是在Coding的路上一定是跌跌撞撞的吧，你們相信有人完全沒有出現Bug就成功打造出一個產品嗎?也許吧!但，那肯定不是我。我並不是想要貶低、否定自己，而只是很單純單純地，認清自己。

今天的天空好藍，澄澈的天空多久沒有看見了呢? 但也是有點擔心不再下雨的日子，祈禱在不久的未來會下雨，不用多，剛剛好就好。

謝謝看到最後的你/妳們。

