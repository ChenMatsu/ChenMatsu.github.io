---
title: React系列-玩轉Typescript待辦清單(下)
date: 2021-05-31 17:37:33
tags: [react, javascript, react, typescript]
categories:
  - [Programming, Javascript, React]
  - [Programming, Javascript, Typescript]
thumbnail:
banner:
---
### 玩轉Typescript
> *Work without love is slavery.*
> *― Mother Teresa*

沒有愛的工作是奴役，但要怎麼去確認自己的工作是不是有愛?

本篇內容主要源自於上一篇React系列-玩轉Typescript待辦清單(上)。

本來打算將內容都寫在同一篇文章，但還是有點勉強，因此才分為上、下篇，在上一篇文章我們定義完類別和ContextAPI後，這裡我們只需要將它們運用在組件上就大功告成了!

- 實作Todolist
- 結語

***

#### 實作Todolist
首先處理第一個組件**Todos.tsx**
```
// In "Todos.tsx"
import { useContext } from 'react';

import { TodosContext } from '../store/todos-context';
import TodoItem from './TodoItem';

const Todos: React.FC = () => {
  // 使用ContextAPI
  const todosCtx = useContext(TodosContext);

  return (
    <div>
      <ul>
        {todosCtx.todos.map((todo) => (
          <TodoItem
            key={todo.id}
            name={todo.name}
            // null告知this不需要參照，todo.id則為第一個參數傳入
            onDeleteTodo={todosCtx.deleteTodo.bind(null, todo.id)}
          />
        ))}
      </ul>
    </div>
  );
};

export default Todos;
```

在**Todos**當中使用到**TodoItem**組件，當然就需要再為它建一個組件囉! 

```
// In "TodoItem.tsx"
import React from 'react';

const TodoItem: React.FC<{name: string; onDeleteTodo: () => void}> = (props) => {
    return (
        <li onClick={props.onDeleteTodo}>{props.name}</li>
    )
};

export default TodoItem;
```
注意到在React.FC後方透過使用Generic去明確定義變數name和函數onDeleteTodo，因為對於TodoItem組件來說，這兩者都是陌生人。最後，因為我們需要新增項目，自然也就需要新增清單的組件。

```
// In "NewTodo.tsx"
import React from 'react';
import { useRef, useContext } from 'react';
import { TodosContext } from '../store/todos-context';

const NewTodo: React.FC = () => {
  const todosCtx = useContext(TodosContext);

  // HTMLInputElement代表DOM的元素類別
  const inputRef = useRef<HTMLInputElement>(null);

  const submitHandler = (event: React.FormEvent) => {
    event.preventDefault();

    const todoName = inputRef.current!.value;

    if (todoName.trim().length === 0) {
      return;
    }

    todosCtx.addTodo(todoName);
  };
  return (
    <form onSubmit={submitHandler}>
      <label htmlFor='name'>Todo Name</label>
      <input type='text' id='name' ref={inputRef} />
      <button>Add Todo</button>
    </form>
  );
};

export default NewTodo;
```
透過使用useRef Hook，可以簡單存取特定欄位的值。唯一需要注意的是HTMLInputElement，在使用Hooks也必須告訴TS相關的型別定義。其餘的都在React系列-useRef概念篇當中有詳細的了解，對於useRef使用有疑問可以參考那篇文章。

最後僅需要將App.tsx做點小修改，就可以成功執行我們的代辦清單囉!

```
import NewTodo from './components/NewTodo';
import Todos from './components/Todos';
import TodosContextProvider from './store/todos-context';


const App = () =>  {
  return (
    <TodosContextProvider >
      <NewTodo/>
      <Todos/>
    </TodosContextProvider>
  );
}

export default App;
```
稍微添加CSS樣式在其中後，實際執行程式後，大致上會如下圖一樣:
![pic](https://i.imgur.com/TtSomMd.jpg)

給自己拍拍手! 我想是因為對於TS理解的不夠深，個人覺得在使用起來沒有太大的差別，但確實對於明確的去定義每一個細節要如何使用都非常的清楚明瞭，這應該是最能體會TS的威力的一點! 當然還有很多值得我們去探討的，就交給明天的自己吧!(飄~)

***

#### 結語
結束五月，不知不覺在這一個月當中有很多成長，不管是學習還是找工作，可以說壓力不少，但卻覺得意外的有趣，看著自己一篇又一篇的文章。有時候都會想為什麼要這麼麻煩，但卻又因為學習的空虛而感到不安，對於我自己來說，忘記以前讀過的書、學會的曲子、寫過的字才是最可怕的，因為那似乎代表自己白活那麼一段時光，一想到此，身體自己就動了起來。

很多時候想的太多，真正需要的只是放手一搏而已，這段路途定然不容易，卻會是朝著理想的路上。
