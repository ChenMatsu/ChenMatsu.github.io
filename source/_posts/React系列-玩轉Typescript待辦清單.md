---
title: React系列-玩轉Typescript待辦清單(上)
date: 2021-05-31 10:54:34
tags: [react, javascript, react, typescript]
categories:
  - [Programming, Javascript, React]
  - [Programming, Javascript, Typescript]
thumbnail:
banner:
---
### 玩轉Typescript
> *Yesterday is gone. Tomorrow has not yet come. We have only today. Let us begin.*
> *― Mother Theresa*

五月的最後一天，明天開始將掀開六月的篇章，未來還有很多挑戰，但今天才是最重要的。

今天想透過React使用Typescript去建立一個簡單的Todolist清單，在四月初其實有使用過React實作過，但當時的自己對於React可以說是完全不了解，很多東西都是到處參考，相當雜亂。本篇將透過介紹Typescript開始玩轉待辦清單(Todolist)。

-預備知識-
- useRef、useContext Hooks使用方式

*** 

- Typescript待辦清單
  - 介紹Typescript
  - 實作Todolist

***

#### Typescript待辦清單
開始實作前需要先理解一下Typescript是什麼，乍看下，跟Javascript很相似。我們的直覺並沒有錯，Typescript確實就是Javascript的超集合，換句話說，有點像是C++與C的關係，C++整體而言多出OOP的概念。Typescript則是比Javascript多出更嚴謹、明確的類別定義。

##### 介紹Typescript
個人而言，Typescript給我的感覺很像是C，不知道為什麼在寫Typescript的時候，腦中一直浮現C的感覺，說起來相當微妙，不過卻又不盡相同。我們看一下範例:

`let num: number = 2021;`

唯一不同於JS的地方在於變數名稱後方多了冒號:，冒號後方再接上變數型態。Typescript的名稱很明顯告訴我們，它的主要概念就在於定義Type，嚴謹的定義，可以免於許多不明確的程式碼。

變數可以定義，函數沒有道理不能定義:

`function add(a: number, b: number): number| string {
  return a + b;
}`

參數的定義是number外，回傳值則是設定為number或string，不覺得很像是C語言嗎? 好吧，可能我深受C的荼毒有點深。特別去注意到，若我們沒有給參數任何型態，一般來說，IDE都會跳出警訊，如下:

`function printEverything(value) {
    console.log(value);
}`
↓
`function printEverything(value: any) {
    console.log(value);
}`

此時就會跳出警訊，解決方式就是在value後方加上類別，若不想指定特定類別，至少也得加上**any**，TS才能理解我們並不想指定特定類別，才不至於產生錯誤。

重複定義其實會有點惱人，因為若需要更改物件的某些定義，就必須要逐一修改，因此TS有個更方便的定義類別方式**type**。

```type Person 
type Person = {
  name: string;
  age: number;
  address: string;
};

let Matsu = Person;
let People = Person[];
```

往後若需要修改這個類別，僅需要修改Person，使用方式則和先前提到的number, string相似，若要使用類別作為陣列，僅需要在後方加上[]即可。

最後關於Typescript最特別的大概是所謂的**通用型類別**(Generic Type)，實際看一下例子:

```
function insertValue<Type>(array: Type[], value: Type){
  const newArray = [value, ...array];
  return newArray;
}

const nums = [1, 2, 3];
const newNums = insertValue(nums, 4);

const strs = ['Matsu', 'Chen'];
const newStrs = insertValue(strs, 'Taichung');
// ↓
// const newStrs = insertValue<String>(strs, 'Taichung');
```

最特別的地方在於<Type>這段，這就是所謂的Generic Type，在此處會根據array和value的類別，去決定函數回傳的類別是數值或是字串，當然我們也可以明確地定義它們的類別，甚至是自行定義Generic Interfaces，不過超出本篇內容就先不提。

##### 實作Todolist
首先要在React專案中使用Typescript，必須匯入相關的lib，幸運的是create-react-app同樣有指令可以幫助我們一塊兒處理這部分的問題:
`npx create-react-app --template typescript`
執行上述的程式碼後就可以正式使用Typescript於React當中。

首先建立Todolist項目的類別型態，單純用於**明確定義**資料的型別。因此在src資料夾下建立models的資料夾後在其中建立todo.ts的檔案，基本上，一個專案會有各種models去定義各自使用的類別，不過我們目前只需要todo項目的資料型別即可:

```
// In "todo.ts"
class Todo {
  // 定義建構子(Contructor)使用到的變數類型
  id: string;
  name: string;

  constructor(todoName: string) {
    // 傳遞進來的值作為名稱指派給新實體 -> 新增新的項目時傳遞
    this.name = todoName;
    // 轉換日期作為辨識的id值
    this.id = new Date().toISOString();
  }
}

export default Todo;
```

完成建立後，TS與JS間的差別在於TS必須先行定義變數的類別，另外todoName乍看下還不太懂用意，它主要會在後續新增項目的函數內使用到。完成最重要的類別定義後，接下來我們使用ContextAPI管理使用的資料，而不是透過props去處理資料，相關優缺點可以參考先前的幾篇文章。

同樣地，src資料夾下新建立資料夾store存放Context，在store中建立**todos-context.tsx**的檔案，注意到先前是jsx這裡自然而然就要改寫成tsx才能夠編譯TS相關的程式碼，往後就不再贅述。

```
// In "todos-context.tsx"
import React, { useState } from 'react';
import Todo from '../models/todo.ts';

// 定義通用類型 
type TodoContextObj {
  todos: Todo[];
  addTodo: (name: string) => void;
  deleteTodo: (id: string) => void;
}

// 建立Context API
export const TodoContext = React.createContext<TodoContextObj>({
  todos: [],
  addTodo: () => {},
  deleteTodo: (id: string) => {}
});

// React.FC告訴TS這是一個React函數組件，需要有props可以傳遞
const TodoContextProvider: React.FC = (props) => {
  // 剛開始初始化時為空陣列，後續更新過後則為Todo類別
  const [todos, setTodos] = useState<Todo[]>([]);

  // 新增項目
  const addTodoHandler = (todoName: string) => {
    const newTodo = new Todo(todoName);

    // 透過先前的陣列狀況去更新陣列
    setTodos((prevTodos) => {
      return prevTodos.concat(newTodo);
    });
  }

  // 刪除項目
  const deleteTodoHandler = (todoId: string) => {

    setTodos((prevTodos) => {
      // 透過Array內建函數filter過濾掉id相符的項目
      return prevTodos.filter( todo => todo.id !== todoId);
    })
  }
  
  const todoContextValue: TodosContextObj = {
    todos: todos,
    addtodo: addTodoHandler,
    deleteTodo: deleteTodoHandler
  }

  // 傳遞狀態
  return (
    <TodoContext.Provider value={todoContextValue}>
      {props.children}
    </TodoContext.Prodiver>
  )
}

export default TodoContextProvider;
```
使用TS最大優點在於嚴謹明確，但相對的也必須負擔較多的責任，透過使用Generic的方式，將自定義類別**TodosContextObj**分別傳入contextAPI與todosContextObj也是為了解決這部份的問題。相對於為各自的props重複撰寫，我想透過contextAPI去處理已經省略相當多重複的程式區塊。

到目前為止，我們已經完成最重要的兩步驟，定義類別與ContextAPI，處理完這兩個檔案後，最後僅需要將資料運用在組件上就算正式完成。

![pic](https://i.imgur.com/9iAiaPc.jpg)

***

#### 結語
這篇文章主要講解Typescript的基礎及結合React使用的方式，透過搭配React Hooks展現出更強大的應用能力。希望能夠將內容講解得更詳盡，因此會分上下兩篇文章，若有任何問題歡迎私訊。 

我個人挺喜歡Typescript給我的感覺，使用上雖然有比較多地方需要注意，卻給我一種不一樣的美，程式碼變得更加純淨的感覺。我想這就是學習的路上有趣的地方，Typescript明明要求得更多卻更簡潔有力，這不是一件很厲害的事情嗎? 看起來限制更多卻更能夠發揮自己的想法，雖然我理解的還很少，但我覺得Typescript很值得去學習。

