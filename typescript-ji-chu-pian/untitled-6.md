# 泛型的概念與實作

在本篇中，筆者要向大家分享的是 TypeScript 中的泛型。

> **泛型程式設計**（generic programming）是[程式設計語言](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E8%AF%AD%E8%A8%80)的一種風格或[範式](https://zh.wikipedia.org/wiki/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B)。泛型允許程式設計師在[強型別程式設計語言](https://zh.wikipedia.org/wiki/%E5%BC%B7%E9%A1%9E%E5%9E%8B%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80)中編寫代碼時使用一些以後才指定的[類型](https://zh.wikipedia.org/wiki/%E7%B1%BB%E5%9E%8B)，在[實例化](https://zh.wikipedia.org/wiki/%E5%AE%9E%E4%BE%8B)時作為參數指明這些類型。各種程式設計語言和其編譯器、執行環境對泛型的支援均不一樣。[Ada](https://zh.wikipedia.org/wiki/Ada)、[Delphi](https://zh.wikipedia.org/wiki/Delphi)、[Eiffel](https://zh.wikipedia.org/wiki/Eiffel)、[Java](https://zh.wikipedia.org/wiki/Java)、[C\#](https://zh.wikipedia.org/wiki/C%E2%99%AF)、[F\#](https://zh.wikipedia.org/wiki/F)、[Swift](https://zh.wikipedia.org/wiki/Swift_%28%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80%29) 和 [Visual Basic .NET](https://zh.wikipedia.org/wiki/Visual_Basic_.NET) 稱之為泛型（generics）；[ML](https://zh.wikipedia.org/wiki/ML%E8%AF%AD%E8%A8%80)、[Scala](https://zh.wikipedia.org/wiki/Scala) 和 [Haskell](https://zh.wikipedia.org/wiki/Haskell) 稱之為[參數多型](https://zh.wikipedia.org/wiki/%E5%8F%82%E6%95%B0%E5%A4%9A%E6%80%81)（parametric polymorphism）；[C++](https://zh.wikipedia.org/wiki/C%2B%2B) 和 [D](https://zh.wikipedia.org/wiki/D%E8%AA%9E%E8%A8%80)稱之為[模板](https://zh.wikipedia.org/wiki/%E6%A8%A1%E6%9D%BF_%28C%2B%2B%29)。具有廣泛影響的1994年版的《Design Patterns》一書稱之為參數化類型（parameterized type）。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E6%B3%9B%E5%9E%8B)

雖然筆者自認是 C++ 白痴，不過第一次接觸到泛型也是因為在 [C++ Reference](http://www.cplusplus.com/reference/) 看到了樣板語法：

```text
template 
myType GetMax (myType a, myType b) {
 return (a>b?a:b);
}
```

不過說真的，第一次看到這麼特別的語法還真的是有看沒有懂 XD

於是筆者就跑去詢問了實驗室的怪物強者，也是我們的邦友 -- CCNode

> 如果今天有一個函式，你需要傳不同的型別參數給函式，則能使用泛型，你只需寫一次程式，  
>  不用複製貼上一堆重複的code，卻只改了參數的型別。
>
> `template`  
>  `T` 代表了任何型別或你宣告出來的 `class/struct` 。
>
> 除了用於 function ，也能用在 class 裡  
>  例如 C++ STL 中的 vector  
>  宣告一個 vector  
>  `vector`  
>  `<>`中就是能填任何的 `型別/class/struct` 。

後來為了準備鐵人賽而學習 TypeScript 時，就有看到與 `Template` 非常相似的用法出現。

簡單來說，若使用者在定義函式時並不想指定函式的輸入值型別，保留它的多樣化，就可以使用`泛型`。

```text
function createArray(length: number, value: any): Array {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

在上面的範例程式中，我們沒有針對函式回傳的陣列所儲存的內容做型別指定，因此我們就使用了陣列泛型： `Array`來告訴 TypeScript 不要因為我們沒有指定型別就不讓我們進行編譯。

這時，可能會有讀者想問：

如果泛型真的這麼棒，為什麼我沒有在 JavaScript 中看到泛型的概念和應用呢？

> 誒不是， JavaScript 根本就不管你輸入的東西是什麼，它都照單全收好嗎 XDD

話說回來，上面的程式碼雖然能夠順利編譯，不過還是有一個缺點：

它沒有精確的定義回傳值的型別。

這時候，泛型就派上用場啦！！

```text
function createArray(length: number, value: T): Array {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

如此一來，當我們呼叫函式時將位於函式名稱後方的 換成我們想要的型別，對於編譯器來說，我們的程式碼長這樣：

```text
function createArray(length: number, value: string): Array {
    let result: string[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

就算我們在呼叫函式時不去指定型別，聰明的 TypeScript 也會透過型別推論將適當的型別帶進 ：

```text
createArray(3, 'x');
```

同理，我們也可以：

```text
createArray(3, 5);
```

適當的利用泛型，可以讓我們的程式碼更富彈性。

### 在泛型中應用多個引數

```text
function swap(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

根據上面的程式碼，我們定義了一個 `swap` 函式，用來交換輸入的元組。

> 眼尖的讀者一定發現了本系列文截至目前為止還沒有提到元組，真的是十分抱歉。
>
> 10/2 更新：各位讀者可以參考 [Day16](https://ithelp.ithome.com.tw/articles/10247407) 。

### 泛型約束

```text
function loggingIdentity(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

如果我們直接編譯上面的程式碼， TypeScript 會告訴我們：

```text
index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'.
```

這是因為泛型 `T` 不一定包含了 `length`，所以編譯的時候才會報錯。

這樣的問題要如何解決呢？

我們可以考慮利用前些篇學到的介面概念，對泛型進行約束：

```text
interface Lengthwise {
    length: number;
}

function loggingIdentity(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

在上面的程式碼中，我們規定介面 `Lengthwise` 必須包含 `length` 屬性，並且在使用泛型時將引數 `T` 繼承了該介面：

```text
function loggingIdentity(arg: T): T {
//...
}
```

這樣一來，便能針對泛型當中的引數設下一定的規定，這樣的作法又稱為：`泛型約束`。

### 多引數的相互約束

> 引數的愛恨情仇（？）

```text
function copyFields(target: T, source: U): T {
    for (let id in source) {
        target[id] = (source)[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
```

根據上面的程式碼，我們看到了引數 `T` 繼承了引數 `U` 。如此一來，開發者便能確保引數 `U` 會具有引數 `T` 不具備的屬性，像是：

U: 我有百萬存款。

T: 我不止有百萬存款，我還有 GTR 。

> ~~筆者：我只有百元存款。~~
>
> 這邊要注意的是：引數也能夠繼承引數。

### 泛型介面

剛剛才在`泛型約束`中看到了利用介面一部分的定義了引數的規則。

那我們能不能在介面中使用泛型呢？

答案是：當然可以 Q\_Q

```text
interface CreateArrayFunc {
    (length: number, value: T): Array;
}

let createArray: CreateArrayFunc;
createArray = function(length: number, value: T): Array {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

或是直接將引數指定的行為提前在介面上進行：

```text
let createArray: CreateArrayFunc;
```

變成：

```text
interface CreateArrayFunc {
    (length: number, value: T): Array;
}

let createArray: CreateArrayFunc;
createArray = function(length: number, value: T): Array {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

### Reference

> 有些文章的內容非常完善，如果有些部分筆者沒有更好的方法詮釋，便會以其他文章作為參考。

* [TypeScript 新手指南 - generics](https://willh.gitbook.io/typescript-tutorial/advanced/generics)

  > 筆者在本篇參考了它的範例程式碼，並以此作為基礎修改。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [官方文件](https://www.typescriptlang.org/docs/handbook/generics.html)
*  [TypeScript FAQ](https://github.com/microsoft/TypeScript/wiki/FAQ)

