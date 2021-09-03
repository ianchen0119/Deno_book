# 型別補充

本章為本系列 TypeScript 基礎教學的最後一篇，整理了一些 TypeScript 的特殊型別:

* 元組 \(Tuple\)
* 列舉 \(Enum\)
* Never
* Unknown

### 元組

元組讓我們能夠對固定長度的陣列的內容作型別指定，陣列中每個元素的型別可以是不同的:

```text
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
```

或是我們也可以分開來賦值:

```text
x[0] = "hello";
x[1] = 10;
```

相對的，若我們將 `hello` 和 `10` 的位置對調，便會發生錯誤:

```text
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

此外，當我們對元組中的元素賦值或訪問一個已知索引的元素時，會得到正確的型別：

```text
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// OK
console.log(x[0].substring(1));
```

> `substring()` 用於分割字串，舉例:
>
> ```text
> const str = 'Mozilla';
>
> console.log(str.substring(1, 3));
> // expected output: "oz"
>
> console.log(str.substring(2));
> // expected output: "zilla"
> ```
>
> 根據上面的程式碼可得知: 我們可以輸入 `substring(起點，終點)` 或是 `substring(希望廢棄段落的終點)`。
>
> -- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

如果超出一開始宣告的範圍，會發生什麼情況呢?

```text
let x: [string, number];
x = ["hello", 10];
x[2] = {};
```

如果新增超出一開始預定範圍的元素，需要注意新增元素的型別只能是元組中每個型別的聯合型別:

```text
x[2] = {};// NOPE
x[2] = 12;// OK
x[2] = "Ian";// OK
```

### 列舉

列舉用於取值被限定在一定範圍內的地方，最常見的就像是一周有七天，一年有十二個月等等。

```text
enum month {
    JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC
}
```

不過需要注意的是:

定義在 `enum` 的成員會被賦值為從 `0` 開始遞增的數字，就像是:

```text
console.log(month["JAN"] === 0); // true
console.log(month["FEB"] === 1); // true
console.log(month["JUL"] === 6); // true
```

相對的，列舉值與列舉名也是對映的:

```text
console.log(0 === month["JAN"]); // true
```

* 知識點: 相等 `==` 與全等 `===`、自動轉型

  > 請參考本日的延伸閱讀。

根據上面的範例仔細觀察，會發現 `JAN` 對應到的是 `0` 而不是 `1` ，該怎麼做呢?

```text
enum month {
    JAN = 1, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC
}
```

我們可以對 `enum` 中的元素手動賦值，如此一來其他元素也會依循前面的元素遞增:

```text
console.log(month["JAN"] === 1); // true
console.log(month["FEB"] === 2); // true
console.log(month["JUL"] === 7); // true
```

### Never

`Never` 型別用於以下情況:

* 沒有回傳值的函式，像是無窮迴圈。
* 一天到晚出錯的函式

```text
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

根據第一項提到的: 沒有回傳值的函式，如果讀者有學好 JavaScript ，第一個會聯想到的應該是 `void` 。

不過， `Never` 與 `Void` 還是有些不同:

`Void` 代表的是沒有回傳值的函式，而 `Never` 會用於表示本就不會有回傳值，或是會拋出錯誤的函式:

```text
function errMsg(msg: string): never {
  throw new Error(msg);
}
```

就上面的程式碼來看，我們可以定義了一個負責發出錯誤的函式，這時候 `Never` 型別就派上用場了。

此外，被宣告為 `Never` 型別的變數僅能被賦值給另外一個 `Never` :

```text
let value :never; 
value = 123;// NOPE
value = ((msg: string)=>{ throw new Error(msg); })('Ian')// OK
```

### Unknown

`Unknown` 與 `Any` 型別有一個共同點，就是: 所有類型都可以被歸類成 `Unkown`, `Any` 。

話雖如此， `Unknown` 還是有其特別之處，畢竟它的誕生就是為了解決 `Any` 型別不夠嚴謹的問題。

```text
let value: unknown = 3;
value = "Ian";
value = undefined;
value = null;
value = true;
```

型別為 `Unknown` 的變數也能夠像型別 `Any` 的變數一樣被任意賦值。

至於與 `Any` 的差別到底在哪，就讓我們繼續往下看:

#### 賦值限制

在 TypeScript 中，僅有 `Any` 與 `Unknown` 型別可以被存放任意值，作為更嚴謹的 `Unknown` 型別，就會有更嚴格的規範。

宣告為 `Unknown` 的變數僅能在被賦值為 `Any` 或是 `Unknown` 型別的變數:

```text
let value: unknown;
let value1: unknown = value;   // OK
let value2: any = value;       // OK
let value3: number = value;   // Nope
let value4: boolean = value;    // Nope
let value5: string = value;    // Nope
// ...
```

筆者認為，這樣的規定是非常合理的，畢竟若 TypeScript 允許一個不確定的變數能夠被賦值給確定型別的變數，會產生很多執行上的錯誤。

> 看看 `Any` 。

#### 限制操作

宣告為 `Unknown` 的變數無法進行任意操作，因為對於 TypeScript 來說，它會將該變數認定為內容不明確的變數。為了避免錯誤操作，所以不允許使用者任意操作 `Unknown` 型別的變數。

至於 `Any` 型別的變數，在被宣告的那一刻就被 TypeScript 當成放棄治療的孩子了，所以你想怎麼做都行:

```text
let value: unknown;
let value2: any;
value.substring();
value2.substring();
```

錯誤訊息如下:

```text
Object is of type 'unknown'.
```

### 總結

本章將一些之前沒有多加補充的型別一次補上，也正式將 TypeScript 基礎系列告一段落了，明天開始便會進行一系列的 Deno 入門與實戰，筆者真的是既期待又害怕趕不出文章來啊啊阿!

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [官方文件](https://www.typescriptlang.org/docs/handbook/generics.html)
*  [\[译\] TypeScript 3.0: unknown 类型](https://juejin.im/post/6844903866073350151)
*  [重新認識 JavaScript: Day 07 「比較」與自動轉型的規則](https://ithelp.ithome.com.tw/articles/10191254)
*  [深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/typings/neverType.html#%E7%94%A8%E4%BE%8B%EF%BC%9A%E8%AF%A6%E7%BB%86%E7%9A%84%E6%A3%80%E6%9F%A5)

