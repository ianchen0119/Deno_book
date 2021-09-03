# 變數宣告

在介紹完 Deno 由來以後，我們正式進入到了 TypeScript 的學習路程上，開始之前還是想先讓讀者知道為何我們需要學會TypeScript。Deno 由於使用了 V8 引擎以及內建 TypeScript 編譯器，所以不管是 Js 或是 Ts 都能直接在 Deno 運行。

> 因此本系列文並不會花時間教大家如何安裝 TypeScript 的編譯器，因為我們的重點是**在 Deno 中利用 TypeScript 寫出可執行的程式碼**，而不是純粹的 TypeScript 教學文章。

TypeScript 相比 JavaScript ，多出了型別系統、型別預判、對物件導向的實作方面...等也有更全面的支持，不過 TypeScript 其實就是 JavaScript 的超集合， JavaScript 支持的 API 在 TypeScript 都是可用的。

* 知識點：應用程序接面 \(API, Application Programing Interface\)

  > 太神奇了傑克，不用寫程式就能動誒。
  >
  > -- CC.T

### 從 JS 搬遷到 TS

> 本小節將探討如何將現有的 JavaScript 程式檔搬遷到 TypeScript 上。

1. 將你的`.js`檔案準備好。
2. 將`.js`副檔名重新命名為`.ts`。
3. 搬遷成功！

> 前面提到 `TypeScript 其實就是 JavaScript 的超集合`，所以搬遷後的程式碼是完全可以正常運行的！

> 注意：其實這樣做一點意義都沒有，因為這樣並不會讓我們體驗到型別預判所帶來的好處。

### 變數宣告

> 注意！ `TypeScript 其實就是 JavaScript 的超集合`，或許筆者會認為我不斷的提醒很多餘，但在這個觀念根深蒂固以後，之後才能更快上手，不會有`這個到底是 JS 還是 TS 的語法啊？`這種充滿不確定的想法出現。

在加入強型別之前，我們先讓大家暸解在 JavaScript 上，我們是如何宣告變數的吧！

主要宣告方式有三種，分別是 `const` 、 `let` 、 `var` ，至於這三種宣告方式的主要差異到底在哪裡，就讓我們繼續看下去吧！

#### const

Const 通常用於宣告永恆不變的數值（識別值），什麼意思呢？像是 `PI=3.14...` 就是確定不會改變的數字。

> 如果哪天被數學家推翻的話，那就另當別論了。

```text
const pi = 3.14159;
```

那我們試試看更改`PI`變數的數值吧！

```text
const pi = 3.14159;
pi = 50;
```

果不其然，編譯器還是報錯了。

> 沒事還是不要挑戰程式語言的規則，就像是：開車不喝酒，喝酒不開車一樣。

![](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch3/3-1.png)

介紹完 `const` 後，我們就來看看 `let` 以及 `var` 的差異吧！

> 三種宣告方式之中，只有 `const` 有不可改變的特性唷！

#### var

> 宣告一個變數, 同時可以非強制性地賦予一初始值。
>
> -- [MDN web docs](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/var)

相較 `const` ，利用 `var` 所聲明的函數可以重複的進行賦值。

```text
var x = 'hello';
x = 'hi';
```

#### let

> **`let`** 用於宣告一個「只作用在當前區塊的變數」，初始值可選擇性的設定。
>
> -- [MDN web docs](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/let)

```text
let y = "123";
y = "456";
```

當然， `let` 同樣也可以讓我們對其宣告的變數重新賦值。

### 修但幾勒， let 與 var 差在哪？

> `var` 可以重新賦值， `let` 也可以重新賦值，那他們到底差在哪？

`var` 與 `let` 的差別為：其作用域不同，講到作用域便會牽扯到像是：

* 範圍鍊
* 暫時性死區

等觀念，這些看似艱深的名詞都會在本章中做解釋，詳細內容就讓我們繼續往下看吧！

### 我全都要！：一次掌握範圍鍊＆＆暫時性死區

> 範圍鍊是什麼呢？為何聽起來這麼專業？

在解釋範圍鍊之前，讀者可以先看看下面這段程式碼：

```text
let x = 'a';
function print(){
  let x = 'b';
  console.log(x)
}
print() //'a'or'b'?
```

如果你的答案是`b`，那恭喜你，你答對了。

> 會出現這樣的結果是因為當 `print()` 執行時，直譯器讀到 `console.log(x)` 便會開始尋找 `x` 這個變數，如果函數內就能找到，
>
> 便將 `let x = 'b';` 所指定的值（字串 `b` ）印出在終端機上。

相對的，如果今天程式碼長成這樣:

```text
let x = 'a';
function toy(){
  let x = 'c';
}
function print(){
  console.log(x)
}
print() //'a'or'b'?
```

透過`範圍鍊`的觀念我們可以知道，當直譯器無法在`print()`當中找到`x`，我們就會跳到更上一層去尋找

`x`（從範例當中看剛好是全域。），因此我們最後會在終端機上看到：

![](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch3/3-2.png)

> 那`toy()`是？都沒有用到幹嘛出現在範例上呢？

喔，沒有啦！那只是單純要混淆讀者，看大家的觀念清不清晰罷了XD

會先告訴讀者`範圍鏈`的概念是因為：`let` 與 `var` 界定範圍的方式不一樣！

> 怎麼個不一樣法呢？

`let` 主要是以 `block` 作為其作用域。

> 阿什麼是 `block`？

```text
{}
```

這就是 `block` 。

所以，當我們執行下列程式碼時：

```text
if(true){
  let x = 'Hi! I'm Peter.';
}
console.log(x);
```

便會看到:

```text
ReferenceError
```

因為對於編（直）譯器來說， `x` 變數僅僅在`{}`中有效，當我們在`{}`外嘗試存取該變數，便會有來源錯誤的問題。  
 此外，使用 `let` 宣告的變數也無法在相同作用域重複宣告：

```text
if(true){
    let x = 'Hi! I'm Peter.';
    let x = 'Hi! I'm Ian.';
}
```

錯誤訊息：

```text
SyntaxError: Identifier 'x' has already been declared
```

至於 `var` 則是使用 `function` 作為界線的。  
 同理，照下列程式的邏輯也會出現 `ReferenceError` 的問題。

```text
(function(){
var x = 10;
})();
console.log(x);
```

但有趣的是，剛剛 `let` 做不到的事， `var` 卻做得到：

```text
(function(){
var x = 10;
var x = 20;
})();
```

對 `var` 來說，重複宣告根本就是小菜一碟啦！

此外，在以下情況下，使用 `var` 與 `let` 宣告的作法也會產出不一樣的結果：

```text
console.log(x)
console.log(y)
var x = 'Ian';
let y = 'Peter';
```

使用 `var` 所宣告的變數會產生 `undefined` 的錯誤，而使用 `let` 宣告的變數則會發生 `causes ReferenceError: y is not defined` 的錯誤。

> 為什麼對於使用 `var` 宣告的變數，編譯器僅告訴我們該變數沒有賦值呢？

其實無論是 `var`, `let` 還是 `const` 都具有 `hoisting` 的特性，在編譯器眼中，剛剛的程式碼是長這樣：

```text
var x;
let y;
console.log(x)
console.log(y)
x = 'Ian';
y = 'Peter';
```

我們可以看到變數的宣告動作都被放到了`作用域`的最前方，  
 儘管如此，在 Javascript 中， `let`&&`const` 與 `var` 的初始化定義有些不同。  
 前兩者都需要確定賦值後，才會被認定初始化完成，因也為這樣，我們才會在剛剛的範例中看到兩種不同的錯誤產生。

調皮的讀者可能會想問：

```text
let x = x;
```

這樣做行嗎？

答案是： 當然不行。

因為這樣做並沒有給予該變數確切的數值，我們可以透過上述一連串的範例觀察到： 變數宣告時機錯誤有可能會讓你的程式碼在某些情況或是區塊中產生 `ReferenceError` 或是 `undefined` 的錯誤，這樣的概念便是開發者常提到的： `暫時性死區 (TDZ)`。

身為負責任的技術文章寫手，還是要為剛剛丟出的奇怪問題負責的對吧，我們可以將 `x` 賦值 `null`, `undefined` 便可以幫助編譯器完成變數的初始化動作啦～～！

> 如果讀者還不瞭解 `function` ，可以考慮看完後面幾天的函式介紹再回來細細品味`範圍鏈`的概念。

### 基礎型別有哪些？

> 雖說JS是弱型別語言，但那不代表JS不存在型別！
>
> 只不過是宣告變數時不需要宣告型別罷了。

* number

  就是我們所熟悉的數字，如: 12345...。

* string

  如果讀者有寫程式的經驗，肯定也不會覺得陌生。

  至於要如何宣告呢? 方法如下:

  ```text
  let string = 'Hello!';
  ```

  當然，使用雙引號也是可以的。

  ```text
  let string = "Hello";
  ```

* boolean

  ```text
  let white = true;
  let black = false;
  function yesOrNo(props){
      console.log(props ? return yes:return no);
  }
  yesOrNo(white); //yes
  yesOrNo(black); //no
  ```

* null

  > 萬物皆空。

* undefined

  > 你什麼都沒問，是要我回答什麼!!

* object

  ```text
  { name: 'Peter' }
  [apple, banana, mango]
  function foo() { /*...*/ }
  ```

  > 如果讀者是第一次接觸 JavaScript ，應該會有疑問: 怎麼陣列還有函式都是物件哩?

  ~~我只能說: 恩，這就是 JavaScript 的神奇之處。~~

* symbol

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* 重新認識 JavaScript

  > 重新認識 JavaScript 是由台灣 Vue.js 社群的大大 - Kuro 所編寫，該系列將許多重要觀念羅列出來並且寫的淺顯易懂，十分牛逼。

  * [變數與資料型別](https://ithelp.ithome.com.tw/articles/10190873)

    > 筆者認為自己實在有點過度貪心，想要在30天中講一卡車的東西。
    >
    > 但是又深怕讀者對於哪些部分想要有更多的認知，如果對於資料型別還有更深層的渴望，請參考這篇。

  * [範圍鍊](https://ithelp.ithome.com.tw/articles/10193009)

    > 該篇介紹範了圍鍊、閉包的文章十分推薦讀者欣賞。

