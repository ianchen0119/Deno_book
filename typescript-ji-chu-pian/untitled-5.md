# 介面與類別、抽象類別

經過前幾章，我們學到了 `Interface` 、 `Class` 的使用技巧，本章將會以先前的知識為基礎，教大家如何將這兩項技巧一同應用在程式碼當中。

### 使用類別實現介面

在先前的章節中，我們利用 `Interface` 描述物件的實作，同理， `Interface` 能被用來描述類別的部分行為。

```text
class Animal {
    public name: string;
    constructor(name: string) {
        this.name = name;
    }    
    sayHi() {
        return `My name is ${this.name}`;
    }
}
class Cat extends Animal{
    constructor(name: string) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);    
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()    
    }
    sayMeow(){
        return 'Meow~';
    }
}
let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

上面的範例是單純的 `Cat` 類別繼承 `Animal` 類別的作法，如果我們今天希望對 `Cat` 的類別先做好描述再開始實作，我們可以使用 `Interface`：

```text
interface cat{
    sayHi() :any;
    sayMeow(): any;
}
```

定義好 `Interface` 後，我們可以在 `Class` 關鍵字後方加上 `implements` + `YourInterface` 進行實作：

```text
class Cat extends Animal implements cat{
    constructor(name: string) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);    
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()    
    }
    sayMeow(){
        return 'Meow~';
    }
}
let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

此外，我們同樣可以讓一個 `Class` 實現多個 `Interface`：

```text
interface Meow{
    sayMeow(): any;
}
interface Hi{
    sayHi() :any;
}
class Cat extends Animal implements Meow, Hi{
    constructor(name: string) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);    
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()    
    }
    sayMeow(){
        return 'Meow~';
    }
}
let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

> 筆者在這邊對於介面的定義比較隨性一些，實際上應該不會有人把這兩個 `Function` 拿出來獨立描述才是。

### TS 的奇怪部分：介面繼承類別

不愧是作為超級奇怪的語言： JavaScript 的超集合語言， TypeScript 可以讓介面繼承類別，至於具體的作用：

> 筆者也不太清楚這是什麼鬼 XDDD

使用介面繼承類別：

```text
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

經過實測該方法在 Deno 上好像不可用，不知道是否是因為新版的 TypeScript 將這個怪奇用法取消了呢？（錯誤代碼如下）

```text
error: TS2564 [ERROR]: Property 'x' has no initializer and is not definitely assigned in the constructor.
    x: number;
    ^
TS2564 [ERROR]: Property 'y' has no initializer and is not definitely assigned in the constructor.
    y: number;
```

答案是 `No` ，我在 TypeScript 的[官方文件](https://www.typescriptlang.org/docs/handbook/classes.html)的最尾端仍然有看到這奇特的用法，有趣的是，在按下範例程式碼上的 `Try it!` 後，我一樣可以從官方的線上 Compiler 看到一模一樣的錯誤訊息。

因此，我也順手試了幾個範例程式碼，也是多多少少有會有 Error 提示....

> 筆者都搞不清楚是我自己不會使用 TypeScript ，還是它本身真的有問題了...

最後，我在 `Class` 中定義了建構者函式便能順利執行了：

```text
class Point {
    x: number;
    y: number;
    constructor(x: number,y:number){
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}
let point3d: Point3d = {x: 1, y: 2, z: 3};
```

只是...這樣真的有意義嗎 XD

### 抽象類別

最後， `TypeScript` 支持抽象類別，至於什麼是抽象類別呢？

> 簡單來說，就是無法被建構出實例的 Class 。

不過，它仍然可以使用建構者函式： `constructor` ：

```text
	abstract class Animal {
    public name: string;
    constructor(name: string) {
        this.name = name;
    }    
    sayHi() {
        return `My name is ${this.name}`;
    }
}
```

在加上 `abstract` 以後， `Animal` 類別就只剩下繼承用途了：

```text
abstract class Animal {
    public name: string;
    constructor(name: string) {
        this.name = name;
    }    
    sayHi() {
        return `My name is ${this.name}`;
    }
}
class Cat extends Animal{
    constructor(name: string) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);    
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()    
    }
    sayMeow(){
        return 'Meow~';
    }
}
let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

至於抽象類別存在的意義在哪裡呢？

筆者認為是：

當今天開發者只希望這個類別是用來作為其他類別的原型，而不是最貼近使用者的那一層面時，我們就可以使用抽象類別。

白話一點說：如果我們今天定義了類別`腳`跟類別`人`，我們在實作人的時候，必定會繼承腳，但不會有人考慮只實作出一隻腳出來。

### 總結

對於 TypeScript 中物件導向實作的介紹就在本章告一段落，如果有任何地方讀者覺得不夠清楚，也都歡迎直接留言與我一起討論唷！

### Reference

> 有些文章的內容非常完善，如果有些部分筆者沒有更好的方法詮釋，便會以其他文章作為參考。

* [TypeScript 新手指南 - Class](https://willh.gitbook.io/typescript-tutorial/advanced/class)

  > 筆者在本篇參考了它的範例程式碼，並以此作為基礎修改。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [ECMAScript 6 入門 - Class](https://es6.ruanyifeng.com/#docs/class)
*  [TypeScript FAQ](https://github.com/microsoft/TypeScript/wiki/FAQ)

