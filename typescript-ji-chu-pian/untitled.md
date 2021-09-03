# 函式宣告

如果讀者有寫過程式，想必大家對於函式都不陌生。

> 除非你把邏輯都寫在 `main()` 啦，那就另當別論了 XDDD

### 什麼是函式?

函式 \(function\) 是執行特定工作的敘述集合，它的主要功能如下:

* 將程式碼中的重複邏輯抽取出來，使程式碼更簡潔。

  原始程式碼:

  ```text
  let a = 1 + 3;
  let b = a + 2;
  let c =  a + b;
  let d = c + 1;
  ```

  使用函式後:

  ```text
  sum(para1, para2){
      return para1 + para2;
  }
  let a = sum(1,3); // 1 + 3 = 4
  let b = sum(a,2); // 4 + 2 = 6
  ```

* 當程式碼非常龐大時，可以依據不同的功能將程式碼分成好幾個片段，讓程式碼結構化以方便管理。

### 學習宣告函式!

在瞭解函式的定義以及用途以後，我們一起來學習如何宣告函式吧!

* 原始作法

  我們將函式宣告以後沒有賦予它名字，便直接將其存放在 `sum` 變數中，這類函式又稱為匿名函式。

  ```text
  let sum = function(para1, para2){
      return para1 + para2;
  }
  sum(1,2); //3
  ```

  當然，如果你希望它也有屬於自己的名字，也可以考慮這樣做:

  ```text
  let sum = function myName(para1, para2){
      return para1 + para2;
  }
  ```

  > 注意 ! `myName` 這個名字只有在函式本身內有效，跳脫該函式的範圍後就沒有人認識他了 !

  ```text
  let sum = function myName(para1, para2){
      console.log(myName); //valid
      return para1 + para2;
  }
  console.log(sum) //valid
  console.log(myName) //invalid
  ```

* 簡潔作法

  ```text
  function sum(para1, para2){
      return para1 + para2;
  }
  sum(1,2); //3
  ```

* 使用 `new Function` 建構函式

  > `new Function (arg1, arg2, ... argN, functionBody)`
  >
  > -- [MDN web docs](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions)

  如果利用該方法去建構函式，先前的範例會變成:

  ```text
  let sum = new Function ('para1', 'para2', 'return para1 + para2');
  ```

  > 筆者並不建議利用這個方式去宣告函式，因為該作法會讓編譯器需要花費多餘的性能去解析字串。

* 物件函式縮寫

  一般情況，若我們需要在物件內宣告函式會需要這麼做:

  ```text
  let obj = {
    sum: function(para1, para2){
       return para1 + para2; 
    }  
  };
  ```

  JavaScript ES2015 \(ES6\) 提供給我們更簡潔的作法:

  > 如果讀者對於 ES2015, ES2020... 等常見字眼感到疑惑，可以參考今天的延伸閱讀唷 !

  ```text
  let obj = {
    sum(para1, para2){
       return para1 + para2; 
    }  
  };
  ```

### 補充: 立即函式 \(IIFE\)

> 懶人包: 定義完就執行的函式。

一般情況下，函式被定義後還需要被呼叫才會執行。不過，在 JavaScript 中有個特別的存在可以允許函式被定義後就立即執行。

下面範例為一般的函式，定義後需要呼叫才會將結果回傳。

```text
function sum(para1, para2){
    return para1 + para2;
}
let result = sum(1,2); //3
```

而 IIFE 比較特別，並不需要呼叫:

```text
let result = function sum(para1, para2){
    return para1 + para2;
}(1,2)
```

如果今天筆者不打算使用變數存放函式的結果，而是希望利用 IIFE 去做程式碼的初始化設定，可以這麼做:

```text
(funtion init(){
   console.log("Init!") 
})()
```

或是利用 `void` 達到同樣的目的:

```text
void funtion init(){
   console.log("Init!") 
}()
```

* 知識點: void

  > 請參考延伸閱讀。

記住，當函式被宣告為立即函式後，便無法利用一般的方式呼叫它:

```text
init()
```

錯誤代碼:

```text
Uncaught ReferenceError: init is not defined
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [Wiki-ECMAScript](https://zh.wikipedia.org/wiki/ECMAScript)
* [void](https://kuro.tw/posts/2019/08/04/JS-%E5%86%B7%E7%9F%A5%E8%AD%98-%E4%BD%A0%E6%89%80%E4%B8%8D%E7%9F%A5%E9%81%93%E7%9A%84-void/)
* [ES6 的縮寫概念](https://wcc723.github.io/javascript/2017/12/23/javascript-short-hand/)

  > 六角學院共同創辦人 - 卡斯柏 大大的部落格也是學習前端技術的必讀網站，筆者學習相關知識時經常能夠在該網站獲得解答。因此，在這邊推薦給各位讀者。

