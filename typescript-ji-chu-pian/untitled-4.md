# 類別的封裝與繼承

前一篇章節向大家帶來了 OOP 的概念，本篇開始將針對 Class 的實作開始講解並介紹: `繼承`、`封裝`、`多型`等重要概念與實現。

### 屬性和方法

* class

  class 是宣告類別名稱的關鍵字。

* constructor\(\)

  Js/Ts 利用 `constructor()` 定義好類別實體化的那一刻要做什麼樣的初始化設定。

  > constructor\(\) 也可以有一個很專業的名詞 : 建立者函數。

* new

  宣告完類別以後還沒有結束! 我們需要使用 `new` 將物件實體化。

  > 剛開始學習 JavaScript 時，常常會出現 `這什麼鬼 Moment` ， `new` 這個關鍵字也不例外。

```text
class Animal {
    constructor(name) {
        this.name = name;
    }    
    sayHi() {
        return `My name is ${this.name}`;
    }
}
let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

### 類別的繼承

* extends
* super\(\)

```text
class Cat extends Animal {
    constructor(name) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);    
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()    
    }
}
let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

### 存取器

* getter

  定義 `getter` 函式作為取得參數的唯一管道 。

* setter

  定義 `setter` 函式作為更改參數的唯一管道 。

```text
class Animal {
  private name: string;
  constructor(name: string) {
    this.name = name;
  }
  get Name() {
    return this.name;
  }
  set Name(value) {
    this.name = value;
    console.log("setter: " + this.name);
  }
}
let a = new Animal("Kitty");
console.log(a.Name); // Kitty
a.Name = "Tom"; // setter: Tom
console.log(a.Name); // Tom
```

為了讓讀者了解 `getter`, `setter` 的意義，筆者在這邊將 `name` 資料成員設為私用變數。

> 私用變數 \(Private\) 不能在宣告它的類別的外部訪問，所以不能繼承，也無法直接訪問。

什麼叫做直接訪問呢?

我們把 `console.log(a.Name);` 改成 `console.log(a.name);`試試看。

```text
error: TS2341 [ERROR]: Property 'name' is private and only accessible within class 'Animal'.
```

這麼做會被提醒 `name` 是私用的，只有 `Animal` 類別有資格存取它。

> 注意 ! `a.Name` 是成員函數， `a.name` 則是類別名稱，可別搞混囉 !

### 靜態方法

* static

  當在宣告時使用 `static` ，就代表我們宣告了一個靜態方法。

  靜態方法代表我們不需要將 `class` 實例化就能直接使用。

```text
class Animal {    
    static isAnimal(a) {        
        return a instanceof Animal;    
    }
}
let a = new Animal('Jack');
```

呼叫 `Animal` 類別中的靜態方法:

```text
Animal.isAnimal(a); // true
```

呼叫 `a` 實體中的靜態方法:

```text
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

### 公用私用傻傻分不清楚?!

除了常用的公用變數 `public` 以外，在 TypeScript 中還有 `private` 以及 `protected` 可以使用。

| 種類 | public | private | protected |
| :--- | :--- | :--- | :--- |
| 內部存取 | Yes | Yes | Yes |
| 子類別繼承/訪問 | Yes | No | Yes |
| 實例化訪問 | Yes | No | No |

#### 內部存取

使用成員函數 `say()` 取得資料成員 `birthday` 這樣的行為就是內部存取。

```text
class Animal {
  public name: string;
  protected constructor(name: string) {
    this.name = name;
  }
  sayHi() {
    console.log(this.name);
  }
}
class Cat extends Animal {
  protected birthday: number;
  constructor(name: string) {
    super(name);
    this.birthday = 8;
  }
  say() {
    console.log(this.birthday);
  }
}
let a = new Cat("Jack");
a.say();
```

#### 子類別繼承/訪問

```text
class Animal {
  public name: string;
  protected constructor(name: string) {
    this.name = name;
  }
  sayHi() {
    console.log(this.name);
  }
}
class Cat extends Animal {
  protected birthday: number;
  constructor(name: string) {
    super(name);
    this.birthday = 8;
  }
  say() {
    console.log(this.birthday);
  }
}
let a = new Cat("Jack");
a.sayHi();
```

我們可以發現在 `Cat` 類別的宣告區塊中是沒有 `sayHi` 這項成員函數的。

```text
let a = new Cat("Jack");
a.sayHi();
```

`a.sayHi()` 之所以能正常工作，是因為 `Cat` 類別繼承了 `Animal` 類別的 `sayHi()` 成員函數唷!

#### 實例化訪問

```text
class Animal {
  public name: string;
  protected constructor(name: string) {
    this.name = name;
  }
  sayHi() {
    console.log(this.name);
  }
}
class Cat extends Animal {
  protected birthday: number;
  constructor(name: string) {
    super(name);
    this.birthday = 8;
  }
  say() {
    console.log(this.birthday);
  }
}
let a = new Cat("Jack");
a.sayHi();
a.birthday;
```

我們在程式碼的末端加上一行 `a.birthday` ，來看看會發生什麼事吧!

```text
error: TS2445 [ERROR]: Property 'birthday' is protected and only accessible within class 'Cat' and its subclasses.
a.birthday;
```

果不其然，筆者得到了錯誤訊息。

這樣的操作稱之為實例化訪問，筆者將 `Cat` 類別實例化後再訪問 `a` 實體的資料成員。

> 為什麼會出錯呢?

```text
protected birthday: number;
```

範例中， `birthday` 是受保護的，代表它並不能被實例化存取。

> 如果讀者到這邊還不熟悉這幾項特性，可以透過嘗試修改範例程式碼親身體驗 : `public`, `protected`, `private` 的差異唷 !

### Reference

> 有些文章的內容非常完善，如果有些部分筆者沒有更好的方法詮釋，便會以其他文章作為參考。

* [TypeScript 新手指南 - Class](https://willh.gitbook.io/typescript-tutorial/advanced/class)

  > 筆者在本篇參考了它的範例程式碼，並以此作為基礎修改。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [ECMAScript 6 入門 - Class](https://es6.ruanyifeng.com/#docs/class)

