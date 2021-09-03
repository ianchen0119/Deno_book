# 在函式中應用強型別

### 在函式中應用強型別

在 TypeScript 問世前，開發者需要耗費額外的心力去避免下面的情況發生:

```text
function sum(para1, para2){
    return para1 + para2;
}
let result = sum('Ian',3);
```

雖然這樣的錯誤並不會讓程式崩潰，但我們會得到超乎開發者預期的結果。

若我們今天是開發一項線上的大專案，是不會允許這種低級錯誤發生的。

然而， TypeScript 的出現，幫助我們解決這個看似簡單，但又惱人的問題。

```text
function sum(para1: number, para2: number){
    return para1 + para2;
}
```

如果你希望程式碼更容易被理解，可以將回傳的參數也標記上型別:

```text
function sum(para1: number, para2: number): number{
    return para1 + para2;
}
```

事實上，在我們在 Deno 上撰寫 TypeScript 時，編譯器也會要求我們對函式的做型別註記。

```text
function sum(para1, para2) {
  return para1 + para2;
}
```

若以上面的程式碼下去編譯，編譯器便會跳出錯誤訊息:

```text
TS7006 [ERROR]: Parameter 'para1' implicitly has an 'any' type.
function sum(para1, para2) 
// ...
```

```text
[ERROR]: Parameter 'para2' implicitly has an 'any' type.
function sum(para1, para2)
// ...
```

### 函數表達式

在一般的情況下，我們會這樣定義函數\(函式\)表達式:

```text
function sum(para1: number, para2: number): number{
    return para1 + para2;
}
```

實際上，這麼做僅僅是對右側的匿名函式進行了類型定義並賦值給 `sum`:

```text
let sum = function(para1: number, para2: number): number{
    return para1 + para2;
}
```

那如果我們希望對 `sum` 定義型別，我們可以這麼做:

```text
let sum: (x: number, y: number) => number = function(para1: number, para2: number): number{
    return para1 + para2;
}
```

> 注意: 不要把 TypeScript 中的 `=>` 和箭頭函式搞混了!  
>  在 TypeScript 的類型定義中， `=>` 用来表示函式的定義，左側是輸入參數的型別 \(需要使用`()`包住\)，右側則是回傳參數的型別。

### 善用 Spread/Rest 運算子

> JavaScript ES6 引入了新的運算子: `...` 來表示展開或其餘運算子。

假設我們希望修改一下前面常常看到的 `sum` 函式，讓它能夠把多個參數相加，一般的作法如下:

```text
function sum(para1: number, para2: number, para3: number): number{
    return para1 + para2 + para3;
}
```

我們可以發現這樣的做法沒有什麼彈性，這時我們可以使用 `rest` 運算子解決這個問題:

```text
function sumAll(...args) { 
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}
sumAll(1,2,3,4,5)
```

使用 `rest` 運算子後，參數都會被壓縮在陣列中，因此在上述範例中才會用到 for-loop 將參數一一取出並做疊加。  
 若我們希望有部分參數被獨立出來，可以這麼做:

```text
function sumAll(para1, para2, ...args) { 
  let sum = 0;
  console.log(para1, para2)
  for (let arg of args) sum += arg;

  return sum;
}
sumAll(1,2,3,4,5)
```

至於 `spread` 可以幫助我們將陣列展開:

```text
function sum(para1: number, para2: number): number{
    return para1 + para2;
}
let params = [1, 2];
sum(...params)
```

或是應用在多個參數之間:

```text
function sum(para1: number, para2: number, para3: number, para4: number): number{
    return para1 + para2 + para3 + para4;
}
let params = [1, 2];
sum(5, ...params, 7)
```

我們可以觀察到，同樣是 `...` 符號，應用在不同的情境中會有不同的功能:

* 在函式定義的輸入使用有 `rest` 的功用。
* 在函式呼叫的輸入使用有 `spread` 的功用。

> 該運算子算是非常靈活好用的功能，想要看更多的話也可以參考今天的延伸閱讀唷!

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [fooish 程式技術](https://www.fooish.com/javascript/ES6/spread-rest-operator.html)
*  [Typescript 入門教程](https://ts.xcatliu.com/basics/type-of-function.html)
*  [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

