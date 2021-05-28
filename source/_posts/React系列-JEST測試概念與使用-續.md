---
title: React系列-JEST測試概念與使用(續)
date: 2021-05-28 16:26:56
tags: [react, javascript, jest]
categories:
  - [Programming, Javascript, React]
  - [Programming, Javascript, Test]
thumbnail:
banner:
---

### React with JEST
> *You cannot change what you are, only what you do.*
> *― Philip Pullman, The Golden Compass*

- 實際測試
- 結語

今天延續上篇JEST測驗概念與使用的內容，若還沒看的朋友們，可以點開下方連結讀過再來看本篇。

**React系列-JEST測試概念與使用: https://tinyurl.com/yh5puf3t**

前幾天稍微聊過關於測試的類別與JEST的使用，最後展示基本範例的測試過程。在本篇我們將來討論其他方面的測試。話不多說，現在就來進入正題。

***

#### 實際測試
首先建立新的檔案叫做**GoodBye.js**:
```
// In GoodBye.js
const GoodBye = props => {
  return <p>{props.children}</p>
};

export default GoodBye;
```
建立這個檔案用途是能夠在**Greeting.test.js**當中，一同測試GoodBye組件，去試著了解整合測試的概念。同時，我們在Greeting組件匯入GoodBye並且將檔案修改一下，相信大家對於props.children不會太陌生，用一句話來說的話，就是打包內容。
```
// In "Greeting.js"
import GoodBye from './GoodBye';

const Greeting = () => {

  return (
    <div>
      <h2>Hello World!</h2>
      <GoodBye>It is good to see you.</GoodBye>
    </div>
  );
};

export default Greeting;
```
接著修改測試檔案的內容:
```
// In "Greeting.test.js"
describe('<Greeting />', () => {
  test('Render Hello World as a text', () => {
    render(<Greeting />);

    const findText = screen.getByText('Hello World', { exact: false });
    expect(findText).toBeInTheDocument();
  });

  test('GoodBye in Greeting', () => {
    render(<Greeting />);
    
    const findGoodBye = screen.getByText('It is good to see you.', { exact: true});
    expect(findGoodBye).toBeInTheDocument();
  })
});
```
特別的是**describe**函數主要用來告知Test要測試的是一個組合(Suite)，每一個describe可以有數個test，使用方式和test函數相似。延續上篇文章的測試後，出現下方結果。

![test](https://i.imgur.com/07nGgkM.jpg)

緊接著來討論關於事件的測試方式，將Greeting組件修改，增加一個狀態管理文字是否顯示:
```
import { useState } from 'react';

import GoodBye from './GoodBye';

const Greeting = () => {
  const [text, setText] = useState(false);

  const changeText = () => {
      setText(true);
  };

  return (
    <div>
      <h2>Hello World!</h2>
      {!text && <GoodBye>It is good to see you.</GoodBye>}
      {text && <GoodBye>Not good to see you.</GoodBye>}
      <button onClick={changeText}>Change Text!</button>
    </div>
  );
};

export default Greeting;
```
因為測試事件需要模擬使用者點擊的行為，必須先引入**userEvent**物件進行模擬。
```
// In "Greeting.test.js"
import userEvent from '@test-library/user-event

describe('<Greeting />', () => {
  ...同上述測試碼...

  test('Check original text is invisible', () => {
      render(<Greeting />);

      const button = screen.getByRole('button');
      userEvent.click(button);

      const originalText = screen.queryByText('It is good to see you.');
      expect(originalText).toBeNull();
  });
});     
```
新增模擬按鈕的測試後，此處只有一個按鈕，直接透過getByRole去找到唯一的一個按鈕。實際上選擇方式和JS處理DOM的概念一樣，接著透過userEvent物件的方式click去模擬按鈕的行為，最後搜尋是否有原先的文字。注意到queryByText作用和getByText一樣，差別在於**query若找到目標會回傳相符的節點，若沒有找到目標則會回傳null**。現在執行測試就會看到三個測試通過囉!

![test2](https://i.imgur.com/CdV1WJR.jpg)

最後我們來看看本日的最後一個測試，主要針對fetch API和資料庫互動的測試。我們來仔細思考一下若測試時也真的與資料庫互動，會產生哪些問題? 以下是可能發生的問題:
- 資料庫流量擁擠
- 影響資料庫資料
- 測試成本暴增

當我們主要想測試是否可以連線成功時，其實是完全不需要真的跟資料庫有後續的互動，即便是真的需要驗證資料庫是否可以成功運作，我想也會有一個模擬的資料庫，而不是與實際使用的資料庫溝通。我們先新增一個新的組件Async。
```
// In "Async.js"
import { useEffect, useState } from 'react';

const Async = () => {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    // 網路上公開測試用API
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then((response) => response.json())
      .then((data) => {
        setPosts(data);
      });
  }, []);

  return (
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
};

export default Async;
```
使用fetch API需要處理非同步的問題，因為從資料庫傳來的資料需要時間傳遞。在Async組件當中使用到useEffect和useState兩個Hooks，用途可以參見先前的文章。另外建立Async.test.js的檔案。
```
import { render, screen } from '@testing-library/react';

import Asnyc from './Async';

describe('Asynchronous API Testing', () => {
  test('Check request successes or not', async () => {
    // 建立Mock(Spy)
    window.fetch = jest.fn();
    // 覆寫fecth API的動作，連線後回傳資料
    window.fetch.mockResolvedValueOnce({
      json: async () => [{ id: 'p1', title: 'First Post' }],
    });

    render(<Asnyc />);

    // 等待fetch動作完成後檢查是否有listitem陣列
    const listItem = await screen.findAllByRole('listitem');
    expect(listItem).not.toHaveLength(0);
  });
});
```
處理非同步的問題需要覆寫fetch API的方法，這裡必須呼叫**window.fetch**才能夠正確覆寫，單純只呼叫fetch API會直接使用這個函數而無法覆寫。接著透過**jest.fn**函數去模擬回傳資料，jest.fn會建立所謂的**Mock**，Mock常常又稱作間諜，因為它可以模仿函數的行為，最後則是透過**mockResolvedValueOnce**去模擬資料的運作。一連串過程中，可以測試是否連線成功，唯一的差別在於資料是由測試人員自行建立。最後確認回傳的list長度是否為0，就能確認模擬狀況。這裡特別要注意到find的方法和get與query都一樣，差別在於它會回傳Promise，因此會等到模擬fetch完成後才執行。

最後我們執行測試，出現下方的結果。

![test3](https://i.imgur.com/XmcR3lu.jpg)

Test Suites共用兩個，因為我們分別有Async和Greeting兩個測試，此外內部分別有1和3個測試，總共有四個測試項目。

-重點回顧-
- Query回傳DOM節點或Null
- Find回傳Promise
- Mock透過jest.fn建立


***

#### 結語
今天把上一篇文章後續的內容討論一下，順便在腦中重新複習一下JEST測試的過程，不過如何針對props去測試還沒有仔細想過，或許會是未來一個有趣的內容。最近花兩天時間讓大腦好好放鬆，看完Netflix上的救命倒數這部美劇，第一季挺不錯的，但第二季就好冗長，跳過的內容相當多。另外艾莉西亞才真的值得珍惜，茱莉亞趕快下去...看的有夠討厭，連恩頭殼壞去，編劇要不要這麼搞?

最後謝謝看到這裡的妳/你們。