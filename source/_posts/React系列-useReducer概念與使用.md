---
title: React系列-useReducer概念與使用
date: 2021-05-15 10:32:54
tags: [react, javascript, component, usereducer]
categories:
  - [Programming, Javascript, React]
thumbnail:
banner:
---
### Hooks家族 - useReducer
> *Don\'t be pushed around by the fears in your mind. Be led by the dreams in your heart.*
> *― Roy T. Bennett*

上一次講述useContext的性質主要是管理變數。今天則是要來介紹useState的強化版，**useReducer**!最近這幾天在學習使用Hooks，雖然我還是沒有開竅，但卻對於Hooks所帶來的改變深深著迷，像是useRef、useCallback，甚至是客製化useHooks，我都覺得相當有趣。好的，廢話不多說，我們馬上來認識useReducer吧!

- useReducer
  - useReducer使用方式
- 結語

***

#### useReducer
為什麼我會說useReducer是useState的強化版? 我們來看一段官方文件所述的話: 
**useReducer is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one.**
大致上的意思是說當我們有複雜的狀態邏輯或者是狀態依賴先前的狀態時，我們就可以考慮使用useReducer，這句話聽起來就像是useState的強化版。我們一般處理狀態時會為各自的狀態寫一個useState:
```
const Counter = () => {
  const [count, setCount] = useState(0);
  const [addText, setAddText] = useState('');
  const [minusText, setMinusText] = useState('');
  
  const addCountHandler = () => {
    setCount(count+1);
    setAddText("I am adding!")
  }
  
  const minusCountHandler = () => {
    setCount(count-1);
    setMinusText("I am minusing!");
  }
  
  return (
    <div className="counter">
      <button onClick={addCountHandler}>Add</button>
      <p>{addText} to {count} </p>
      <button onClick={minusCountHandler}>Minus</button>
      <p>{minusText} to {count}</p>
    </div>
  )
}
```
其實這邊寫得相當的繁瑣，有點不符合實際情況。不過沒關係，這邊只是單純展示使用useReducer的時機。從上述程式碼，我們可以看見當我們要設定增減按鈕時，需要為增減的行為設定一個useState，若我們想再加上文字的變更，必須再多新增文字各自的狀態。透過useState，已經減輕不少工作，但，礙於人類的天性，我們還是覺得這樣過於繁瑣。

這時候useReducer的就派上用場了! 
useReducer往往會搭配useContext使用，但我們這邊先專注在useReducer本身。

##### useReducer使用方式
useReducer是useState的一種替換寫法。它的長相大概如下所示:
`const [state, dispatch] = useReducer(reducer, initialArg)`

主要的概念是透過useReducer將**reducer**這個**函數**串接進來，reducer則會根據dispatch去決定要執行的動作。state會根據initialArg去決定型態，若initialArg是物件，則state也會是物件。同樣地，dispatch也會是物件的型態，裡頭會儲存各種方法，以這邊的例子來說，就是增減的方法。我們來實際看看如何改寫上述的計時器:

```
const defaultCountState = {
   count: 0,
   text: ""
};

const countReducer = (state, action) => {
  switch(action.type){
    case 'ADD':
      return { 
        count: state.count + 1,
        text: "I am adding."
      };
    case 'MINUS':
      return { 
        count: state.count - 1}
        text: "I am minusing."
      };
    default:
      throw new Error();
  }
}

const Counter = () => {
  const [countState, dispatchCount] = useReducer(countReducer, defaultCountState);
  
  const addCountHandler = () => {
    dispatchCount({type: 'ADD'});
  };

  const minusCountHandler = () => {
    dispatchCount({type: 'MINUS'});
  }

  return (
    <div className="counter">
      <p>Count: {countState.text} {countState.count}  </p>
      <button onClick={addCountHandler}>Add</button>
      <button onClick={minusCountHandler}>Minus</button>
    </div>
  )
}
```
寫好count的reducer，裡面包含增減對應的方法，接著再透過useReducer將方法以及我們預設的變數傳入Counter組件。最後當我們實際執行上述的程式碼的時候，就可以發現，透過useReducer，我們成功同時控制文字與數字的狀態。好像有點小興奮!沒錯，這就是我們useReducer的Power所在! 
![count](https://i.imgur.com/rlvovbM.jpg)

-重點回顧-
- useReducer可以控管複雜的狀態
  - reducer是一組函數
  - state、dispatch一般都是物件

***

#### 結語
今天簡單講解useReducer這個Hook，熟悉Redux的各位大大一定不陌生這個Hook，但心中可能會有疑惑為何有Redux又需要useReducer呢? 這就因人而異，只要能達到目的，我想用哪種方法都沒有對錯的分別，全取決於個人。在這篇章，我們還沒提到global變數的概念，一般來說useReducer都會搭配useContext的使用去處理資料，接下來的篇章也許會討論這塊也說不定。

useReducer並不是要完全取代useState，它只有當狀態變得複雜時才適合使用，換句話說在處理簡單的狀態時，就不需要去使用它，畢竟殺雞焉用牛刀是吧?

題外話: 今天一直在想要怎麼去寫useReducer，最後坐在馬桶上還是覺得簡單用Count去解釋好了，原先考慮用自己實作的訂餐網站來講解，但似乎不適合當作介紹篇章的範例。

最後，謝謝各位大大看到最後!



