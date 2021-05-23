---
title: JS系列-同步與非同步入門
date: 2021-05-23 10:39:13
tags: [javascript, async, await]
categories:
  - [Programming, Javascript]
thumbnail:
banner:
---
### JS - Async / Await入門
> *Be who you are and say what you feel, because those who mind don\'t matter, and those who matter don\'t mind.*
> *― Bernard M. Baruch*

今天來說說關於JS的同步與非同步問題。JS本身屬於**單執行緒**的語言，因此指令會依照順序或者透過特殊語法來控制執行的順序，這就是要來討論的主題。

- 瑪德蓮蛋糕
  - 同步與不同步(Synchronous/Asynchronous)
  - Callback
  - Promise
  - Async/Await
- 結語

***

#### 瑪德蓮蛋糕
透過製作瑪德蓮來學習同步與非同步的概念，我想應該會相當有趣。瑪德蓮蛋糕是我偶爾會在家裡做的一種法式糕點，有興趣的朋友們也不妨嘗試做做看!
首先，我們先來看看瑪德蓮的製作過程:
- 準備食材(食譜)
  - 雞蛋*2
  - 牛奶20g
  - 蜂蜜30g
  - 細砂糖40g
  - 低筋麵粉100g + 泡打粉3匙
  - 無鹽奶油85g
- 攪拌全蛋
- 加入砂糖、牛奶
- 加入過篩的低筋麵粉、泡打粉
- 加熱無鹽奶油
- 加入融化奶油攪拌
- 裝入擠花袋冷藏6小時
- 烤箱預熱15分鐘170度
- 擠上模具烘烤15分鐘
- 取出完成

大致上的製作過程如下，其中有許多步驟是需要一步一步去做，但是這裡就牽涉到不少的同步與非同步的概念。

##### 同步與非同步
同步(Synchronous): 同時執行動作
非同步(Asynchronous): 等待完成後執行下一個動作

因為JS特性的關係，我們沒辦法一次執行多個動作，舉例來說: 攪拌全蛋時加熱無鹽奶油，這對於JS本身來說是不可能的。況且有些時候，同步執行不會帶來好處，只會帶來災難，舉例來說，我們如果先預熱烤箱，但是我們根本還沒將瑪德蓮裝入擠花袋並且冷藏6小時，預熱的動作就完全只是徒勞。由此可見，同步與非同步都必須視情況而定。

我們嘗試將完成製作蛋糕的步驟簡單寫成JS的程式碼試試看:
```
const Madeleine = () => {
  prepare_ingredient(); 
  stir_egg();
  add_sugar_milk();
  add_flour();
  melt_butter();
  add_butter();
  put_to_bag();
  freeze();
  pre_heated();
  bake();
};

// 準備食材
const prepare_ingredient = () => {};
// 攪拌全蛋
const stir_egg = () => {};
// 加入糖、牛奶
const add_sugar_milk = () => {};
// 加入麵粉
const add_flour = () => {};
// 加熱奶油
const melt_butter = () => {};
// 加入奶油
const add_butter = () => {};
// 裝入擠花袋
const put_to_bag = () => {};
// 冷藏六小時
const freeze = () => {};
// 烤箱預熱 
const pre_heated = () => {};
// 烘烤
const bake = () => {};

Madeleine();
```

我將一連串的動作都寫成函數，接著在最後Madeleine函數去模擬做蛋糕，可以看到我們確實依照食譜的順序去逐一執行動作，JS也會根據程式的先後順序去執行。可是這裡似乎會發生一點問題，問題在哪裡? 問題在於，每一個動作間並沒有彼此認識，舉例來說: 在準備完材料後要攪拌全蛋，但是有沒有可能材料還沒準備完，就開始攪拌全蛋，而導致沒有確認無鹽奶油準備到，而後續造成melet_butter函數執行失敗，這就是此處的問題點所在。

##### Callback
先來聊聊關於Callback，Callback是JS裡面有趣的函數使用方式。我們先來看一下MDN上面的解釋:
**A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.**
簡單來說Callback可以理解成函數執行完後要執行的函數，以上述Madeleine內的步驟來說:
```
const Madeleine = () => {
  prepared_Ingredient((res) => {
    if(res === 'Ingredient prepared.'){
      stir_egg((res) => {
        if(res === 'stir finished.'){
          add_sugar_milk(...其他Callback);
        }else{
          console.log('Not stirred yet!');
        }
      });
    } else {
      console.log('Ingredient not prepared yet!');
    }
  })
};

Madeleine();
```
上述的prepared_gradient在執行完成後，才呼叫stir_egg()函數，此外也可以透過Callback去處理錯誤的情況，這樣子動作就能夠認識彼此，更能夠了解彼此的優先順序，相較於先前全權交給JS執行緒處理，是不是更令人安心了? 但是這時候就出現所謂的Callback地獄，雖然Callback很方便也很好使用，但是過度使用，就會造成難維護的窘境。

##### Promise
MDN的描述:
**A Promise is an object representing the eventual completion or failure of an asynchronous operation.**
簡單來說，Promise是表達**非同步執行**結果的一個物件，會根據執行成功或失敗給予開發者回饋。因此，只要回傳Promise物件，我們都能夠使用Promise物件擁有的then函數和catch函數。then函數可以擁有兩個Callback代表Promise執行成功與失敗後的動作，而catch函數只擁有Promise執行失敗後的Callback。直接更改上述烘焙的程式碼如下:
```
// 假設Madeleine回傳Promise
const Madeleine = new Promise((resolve, reject) => {
  //暫定每1s成功執行後執行下一個then()
  setTimeout(() => {
    resolve('Madeleine');
  }, 1000);
});

Madeleine()
  .then(prepare_ingredient(), {
    console.log('Ingredient not prepared yet!');
  })
  .then(stir_egg(), {
    console.log('Not Stirred yet!);
  })
  .then(add_sugar_milk(), {
    console.log('No milk to be added!');
  }
  ...其他的thenc函數
  .catch(() => {
    console.log('Failed to make Madeleine');
  });
```
透過Promise的使用，我們可以更好的去控管整個製作的流程，也就是將Callback管理的更好。

這邊要特別注意到new Promise這一段程式碼，這裡其實就是將Madeleine指派一個Promise的物件，而new Promise這個物件通常會有兩個參數resolve和reject，前者代表成功後要執行的行為，後者代表失敗後要執行的動作，相似於then和catch的概念，因為then和catch就是透過它們包裝而來的呀!

##### Async/Await
最後要講的是JS改版至ES7後的大明星Async和Await，因為它們的出現，我們得以從Promise Chain的枷鎖中獲得解放。實際看一下如何透過Async/Await來改寫我們在Promise的程式碼:
```
const Madeleine = async() => {
  console.log('Start making Madeleine');
  const ingredient = await prepared_ingredient();
  const stirredEggs = await stir_egg(eggs);

  ...其餘動作

  console.log(ingredient);
  console.log(stirredEggs);
  ...其他結果
};

const prepared_ingredient = () => {
  return new Promise((resolve, reject) => {
    if(egg && milk && sugar && honey && butter && flour){
      setTimeout(() => {
        resolve('Ingredient is prepared.');
      }, 3000);
    } else {
      reject('Ingredient is not prepared!');
    }
  });
}

const stir_egg = (eggs) => {
  return new Promise((resolve, reject) => {
    if(eggs === 2){
      setTimeout(() => {
        resolve('Eggs are stirred.');
      }, 5000);
    } else {
      reject('No enough eggs!');
    }
  });
}

... 其他動作的函數
```
透過async的關鍵字，我們可以告知JS要將Madeleine當作非同步的函數，此外因為其他動作函數也會是非同步，可以確定在Madeleine內執行的動作都是await等待過後，才會接續執行，而不會發生動作還沒完成就執行下一個動作的問題。

***

#### 結語
花了很多時間查看許多文章，對於Callback, Promise, Async/Await，釐清了許多問題，但是還是覺得文章打得不夠好，還不能講解的淺顯易懂，代表我自己還沒有真的很熟悉這些概念以及實作方式。期望接下來能夠對這三者有更深的見解，屆時會再來修正這篇文章。


