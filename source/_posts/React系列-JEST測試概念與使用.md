---
title: React系列-JEST測試概念與使用
date: 2021-05-26 14:49:58
tags: [react, javascript, jest]
categories:
  - [Programming, Javascript, React]
  - [Programming, Javascript, Test]
thumbnail:
banner:
---

### React with JEST

> _The unexamined life is not worth living_.
> _― Socrates_

這幾天在思考值得研究的主題，但是腦中卻一絲想法都沒有，我想是因為不斷在思考眼前的煩惱，而沒有空出時間給大腦去自由翱翔。不過，今天剛好學習到一門挺有趣的課程也是我想特別討論的主題，在求職過程中，我發現不少資深的職缺都有這部分的能力要求，那就是 Test。

蘇格拉底說:沒有經過審視的生命不值得去體驗。我想沒有經過測試的產品，也不值得人們去使用，程式碼也是一樣的道理，仔細去思考，一個產品只要有瑕疵就要面臨下架或者回收的下場，程式碼又何嘗不是如此，我們立刻進入主題。

- React 與 JEST
  - 何謂 JEST
  - 單元測試、整合測試、端對端測試
  - 實際測試
- 結語

***

#### React 與 JEST

今天主要透過 Jest 去測試 React，因為最近比較熟悉 React 框架，覺得透過 React 來學習會比較快上手。一般來說，專案都需要引入第三方的函式庫才能夠去執行測試，不過幸運的是，當我們透過`create-react-app`指令建立專案時，測試函式庫都會一併安裝完成，在 package.json 檔案中，我們能夠發現下方這三個套件。
`"@testing-library/jest-dom": "^5.11.6"`
`"@testing-library/react": "^11.2.2"`
`"@testing-library/user-event": "^12.5.0"`
當然就算沒有也沒關係，手動安裝一下，應該也不成問題。

***

##### JEST

首先從 JEST 開始談起，JEST 其實是專門用來測試所有 Javascript 程式碼的測試函式庫，不僅限於 React，同樣可以用於 Vue, Angular, TypeScript, Node 等等...。

JEST 大致上有幾大特點:

- Fast/Safe: 平行執行測試且確保測試碼擁有唯一的全域狀態
- Code Coverage: 透過設定--coverage 旗標來設定測試範圍
- Easy Mocking: 執行模擬測試範圍外的程式碼

##### 單元測試、整合測試、端對端測試

在 Coding 的過程中除錯，其實就是一種測試，大致上可以將測試分為以下兩類:

- 手動測試
- 自動化測試

但是我們都知道，只透過手動的測試，必然會出現許多的盲點，因此才會有自動化測試，而自動化又可以根據不同的測試要點，分為以下三類:

- 單元測試(Unit Test)
- 整合測試(Integration Test)
- 端對端測試(End-to-End Test)

**單元測試:** 分離測試獨立區塊、函數、組件
**整合測試:** 整合測試數個區塊、函數、組件
**端對端測試:** 專案測試使用者體驗

基本上，最常看到的是單元測試，在一個專案當中，往往會將各種情境都盡可能測試過，因此大部分情況下都會針對個別的小區塊做測試。因此，一個專案可能會有數百數千個單元測試，數百個整合測試與數個端對端測試，根據情況去使用不同的測試。

但是要如何在專案中直接執行測試? 這就需要仰賴 JEST 去執行我們的測試碼，除此之外，我們還需要模擬 React App 的工具，也就是 React Testing 函式庫，上述兩個工具都已經在先前提到過。我們實際來測試看看。

***

##### 實際測試

首先我們在專案當中建立 components 的資料夾，接著在資料夾中建立 Greeting.js 這個組件，如下:

```
// In "Greeting.js"
const Greeting = () => {

  return (
    <div>
      <h2>Hello World!</h2>
    </div>
  );
};

export default Greeting;
```

現在我們已經有 Greeting 組件可以測試，但是現在要如何撰寫測試檔案? 一般來說，我們會在同一個資料夾直接建立同樣檔名的測試檔案，這裡我們在 components 建立 Greeting.test.js 測試檔。

```
// In "Greeting.test.js"
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

test('Render Hello World as a text', () => {
    render(<Greeting />);

    const findText = screen.getByText('Hello World', { exact: false });
    expect(findText).toBeInTheDocument();
});
```
測試Greeting組件要將它匯入測試檔中，此外還必須匯入兩個函數，分別為**render**和**screen**:
render: 模擬React渲染
screen: 模擬DOM的選取

接著透過JEST的test函數，告訴React這裡是要進行測試的檔案。`test('', {})`，參數一描述測試用途，參數二則是用來撰寫測試的程式碼，可以注意到我們在這裡透過render(<Greeting />)去模擬React渲染，接著透過screen去尋找是否有**Hello World**的文字，exact代表是否需要完全相符，預設情況下是false。最後透過expect函數去回傳測試結果。

實際跑過`npm test`就會出現下方測試通過的結果:
![test1](https://i.imgur.com/9WtRQL7.jpg)

這就是最簡單單單的單元測試，希望想對快速了解JEST和單元測試的朋友們有點幫助。

***

#### 結語
原本想說要一次將測試的文章打完，但才打沒一半就篇幅就已經有點多，因此打算後續再根據這篇文章撰寫整合測試以及處理API的問題。很高興今天自己終於能夠對Test有點基礎概念，希望未來有機會能夠將它派上用場。我想這只能算是初窺JEST的堂奧，但作為認識JEST的第一步，應該算是相當直覺的了。

謝謝看到最後的各位。