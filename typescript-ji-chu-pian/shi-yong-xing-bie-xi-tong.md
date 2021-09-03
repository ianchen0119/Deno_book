# 使用型別系統

本章要跟各位讀者介紹 `TypeScript` 中最大的賣點，也就是`型別系統`以及`型別預判`！

### 學會宣告之後：華麗變身強型別

我們在前一章節學習到如何宣告變數並且認識了 JavaScript 的基礎型別。

本小節要在這個基礎上教大家為變數增加型別系統，我們先看看以下範例:

```text
let num = 3;
```

筆者在上面宣告了 `num` 變數，其值為 `3` 。

`TypeScript` 非常的聰明，當你為宣告的變數賦值的那一刻開始，它便已經為變數添加型別了。

```text
num = 'Deno';
```

我們將 `num` 重新賦值，來看看會發生什麼事吧!

```text
TS2322 [ERROR]: Type 'string' is not assignable to type 'number'.
```

結果就是: 編譯不過。

> 為何編譯不過呢?

因為當我們在宣告 `num` 並賦值為 `3` 的那一刻起， `TypeScript` 會這樣認定我們的程式碼:

```text
let num: number = 3;
```

我想看到這邊，各位讀者就能明白剛剛為何會有編譯不過的狀況發生了。

> 'Deno' 是字串，並非數字。

當然，如果你希望這個變數可以存取 `string` 以及 `number` 類型的值，你可以這樣做:

```text
let numOrString: number | string = 3;
numOrString = 'Peter';
```

> 這樣的概念在 `TypeScript` 的官方文件中稱為: 聯合型別 \(Union Types\) 。

至於 `TypeScript` 的型別種類，那可以說是不勝枚舉，多到爆阿!!!

* 原始型別

  JavaScript 原本就有的型別，也就是筆者前一章所提到的: `number`, `string`, `null`, `undefined` ...。

* 物件型別

  包含了我們經常使用的 `Json`, `Object`, `Array`, `Function` ...。

  > 對， `JavaScript` 真的很奇怪， 陣列跟函式都是屬於物件的一種喔 !
  >
  > 想當然爾， `TypeScript` 也怪怪的。
  >
  > ~~就像是如果一個怪人變成了蜘蛛人，當他穿上戰服時，他也還是個怪人，超級英雄的身分並不能改變他身為人的本質。~~

  > 喔對了 ! 我們之後學習物件導向時會使用到的 `Class` ，在實例化以後也是屬於物件型別唷 !

* 明文型別 Literal Type：一個值本身也可以成為一個型別，比如字串 "Hello world" ...等。

  ```text
  let foo: 'Hello world';
  foo = 'Hi'; //Error:Type '"Hi"' is not assignable to type '"Hello world"'.
  foo = 'Hello world'; // OK
  foo = null; //OK
  foo = undefined; //OK
  ```

  > 在 TypeScript 中，預設情況下 `null` 和 `undefined` 可以是任何型別的值。
  >
  > 除非打開 `strictNullChecks` 設定，至於在 Deno 上要如何調整這些設定，筆者會花時間研究再補充上來。

* 特殊型別： `any`、`never` 以及 `unknown` 。

  > 這個部分可以參考本章最後 - 延伸閱讀 中所提供的系列文，該系列文的作者對於這些特殊型別進行了非常深入的探索。

### 你怎麼什麼都知道？！：超牛逼的型別預判系統

TypeScript 讓人驚艷的並不僅僅是加入了型別系統，搭配 VSCode 以後更能彰顯這個優勢。

![4-1](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch4/4-1.png)

VSCode 與 TypeScript 具有高度的整合，就上圖來看，當筆者宣告 `x` 變數並賦予它值時，

VSCode 便能明確的指出 `x` 變數的預設型別。

> 某種層面上來說，我們可以認定編輯器看得懂我們的程式碼。

我們在更大膽一點，丟錯誤的程式碼給 VSCode 看看:

```text
let x = "a";
x = 4;
```

即便沒有指定型別， VSCode 也能立刻提醒使用者這麼做是錯的:

![4-2](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch4/4-2.png)

這麼聰明的開發環境，有誰能不愛上呢?

> 仔細回想，筆者是在 JSDC 2019 Workshop 第一次看到 VSCode 搭配 TypeScript 的魔力。
>
> 永遠不會忘記那時候保哥將大量的 JS 程式碼搬遷到 TypeScript 環境只花了不到 5 分鐘 !
>
> 因此，能在今年暑假學習 TypeScript 並且將它分享給各位實在是一件讓人興奮的事情啊 !
>
> ~~一不小心，筆者就又偏題了 XD~~

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [讓 TypeScript 成為你全端開發的 ACE！](https://ithelp.ithome.com.tw/users/20120614/ironman/2685)

  > 如果讀者希望看到繁體中文的相關資料，我認為這系列真的必看，講的非常詳細。
  >
  > 筆者在這 30 天中很難將 `TypeScript` 的各項觀念一一帶出，如果有需要繼續向下探索，
  >
  > 真的大推、怒推這個系列文。

