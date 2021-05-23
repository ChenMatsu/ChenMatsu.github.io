---
title: Leetcode-20.Valid Parentheses(C)
date: 2021-05-21 10:21:43
tags: [c, leetcode, valid parentheses]
categories:
  - [Programming, C, Leetcode ]
thumbnail:
banner:
---
### Valid Parentheses in C

昨晚久違的回歸Leetcode，回歸的原因是因為推坑好友到Leetcode裡相約要一起開始一天一題，結果我過了一周才開始，有夠邪惡。但總而言之，希望今天會是一個好的開始。其實去年也才寫沒個幾題，真的可以說自己是個沒有毅力的人，現在也真的是不確定自己會不會堅持下去，畢竟光是React和Node就把自己搞的半死，現在想想大概就是解多少算多少了吧!

昨晚不知道為什麼會挑選Valid Parentheses這題，中途寫不出來還想說是不是要換題目，但仔細想想換題目大概只是遇到另外一個瓶頸，還是咬緊牙關寫下去，沉沒什麼的早已不在乎。礙於太久沒有用C，說實在想到要寫的語法都是JS...實在有點慚愧，看了題目的敘述後，想說用堆疊(Stack)去處理，結果搞個半天，還是沒有成功，想說是不是因為把Pop和Push方法都寫進去，就在網路上複習一會。結果昨晚還是沒解出來!!原本以為洗完澡會想出來，結果到早上腦袋變得清晰才解出來。來看看題目吧。

- Valid Parentheses
  - 題目規則
  - 解法
- 結語

***

#### Valid Parentheses
我們先看一下題目要求:
**Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.**
簡單來說，題目會輸入一個包含這三種符號的字串，字串是透過字元指標(Char Point)傳入，我們需要驗證字串的括號是否成對。就是一般在IDE中Coding打錯括號也會跑出錯誤訊息的括號檢驗。

##### 題目規則
大致上分為以下幾種驗證標準:
Example1:
`Input: s = "([{}])`
`Output: true`

Example2:
`Input: s = "({[}])"`
`Output: false`

因為彼此交錯回傳false，我們可以得到事實是若某字串符合括號驗證，字串中的**子字串會通過驗證**。以上述Ex1來說，`{}`、`[{}]`都屬於Ex1的子字串，它們同樣通過括號驗證。

#### 解法
```
bool isValid(char *s){
  int f = -1, len = strlen(s);
  // 宣告儲存左邊括號的陣列
  char stackVal[len];

  // 當*s字串不為'\0'空白字元
  while(*s){
      // 先確認*s是否為某右括號
      if(*s == ')'){
          // 確認stackVal括號類別
          if(f >= 0 && stackVal[f--] == '(');
          else return 0;
      } else if (*s == ']'){
          if(f >= 0 && stackVal[f--] == '[');
          else return 0;
      } else if (*s == '}'){
          if(f >= 0 && stackVal[f--] == '{');
          else return 0;
      } else {
          // 推進不是右括號的所有符號
          stackVal[++f] = *s;
      }
      // 移動位址空間到下一個字元(指標常有的存取方式) -> 也可使用陣列方式存取
      s++;
  }
  // 確認最後stackVal是否清空(概念上)
  return f == -1;
}
```

***

#### 結語
主要的解題概念在於能不能追蹤堆疊的索引值，若能夠追蹤就能夠模仿堆疊的Pop和Push的概念，而不需要實際去實作這兩個方法。嚴格上來說，有點偷吃步的概念，因為stackVal這個陣列並沒有真的清空，而是透過旗標f去驗證個別的值。