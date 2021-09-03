# 物件導向概念

### OOP 能吃嗎?

> C 語言程式設計是以函數 \(Function\) 為單元的結構化程式設計， C++ 語言程式設計則是以類別 \(Class\)為單元的物件導向程式設計。
>
> -- C++ 全方位學習，古頤榛

物件導向程式設計 \(Object-Oriented Programming, OOP\) 是使用類別物件為主的程式設計。類別觀念是由結構衍生而來。

類別可以包含資料變數與處理資料的函數:

* 資料成員: 我是類別中的資料!
* 成員函數: 我是類別中的函式!

對於沒有本科經驗的人來說，物件導向一直是個謎一樣的存在。

我依稀記得，在去年的 JSDC Workshop 的某個議程中，講者保哥提到了物件導向並且問了在座的各位有沒有聽過。

令人驚訝的是!還真的有很多人不知道 XDD

~~所以各位讀者如果學會了，說不定就能成為前10%的開發者喔~~

我自己對物件導向程式設計的見解是:

> 一種設計思維，利用該思維實作出一個物體的抽象概念。
>
> -- 帥氣的我。

~~公X小~~，如果以螃蟹機器人作為舉例，機器人完成組裝的那一刻有什麼是確定的呢?

大概有:

* 資訊
  * 製造商
  * 型號
  * 序列號碼
  * 諸如此類...
* 行為、行為所需的參數
  * 電量
  * 向右移
  * 向左移
  * 諸如此類...

以物件導向下去實作的話，大概會長這樣:

```text
class tableTopRobot {
    constructor(data){
        let {sN, vendor, material, power} = data;
        this.sN = sN;
        // ...
    }
	moveToRight(){
        // ...
    }
    moveToLeft(){
        // ...
    }
}
let data = {
    sN: '2030',
    vendor: 'ianToy',
    // ...
}
let peter = new tableTopRobot(data);
```

我們利用資料成員以及成員函數詳細的定義螃蟹機器人的各項細節，包含它要如何行走、電量、生產細節...等資訊。

此外，物件導向也包含了繼承的概念:

```text
class people {
    public name: string;
    constructor(name:string){
        this.name = name;
    }
    sayHi(){
        return `Hello! My name is ${this.name}`;
    }
}
class superHero extends people {
    public birthday: number;
    constructor(name: string, birthday: number){
        super(name);
        this.birthday = birthday;
    }
    tellMeBirthday(){
        return `${this.birthday}`
    }
}
let peter = new superHero('Peter',19990119);
console.log(peter.tellMeBirthday())
```

上面的程式是使用 TypeScirpt 撰寫，我們可以在範例中觀察到 `superHero` 類別繼承了 `people` 類別的特性。

這邊只是先讓讀者對物件導向有粗淺的了解，詳細的實作方面會用多篇文章細談。

### 至於 JavaScript 呢?

早期， JavaScript 是不支援物件導向的，它更傾向函式語言程式設計。

* 知識點:[函數式程式設計](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)

幸好，在廣大的開發者社群的努力下， JavaScript 終於在 ES6\(2015\) 正式的支援物件導向程式設計。

我們在更仔細的回想上一小節提到的:

> 類別可以包含資料變數與處理資料的函數。

有經驗的開發者可能就會問: 阿物件不是也可以嗎!

沒錯，其實 JavaScript 的 Class 就是物件的語法糖。

JS ES5:

```text
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

JS ES6:

```text
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

JS 提供了一個讓人勉強接受的 Class ，好消息是: TypeScript 當然針對這點做出了全面的加強，詳細的用法筆者會在後面幾個篇章向大家好好介紹。

### 原型鏈

JS 是一門奇怪的語言，你如果不知道原型鏈是什麼也能寫出程式碼，

但筆者認為，如果知道這個概念對於讀者會更好。

> 請先參考本日的延伸閱讀。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [\[筆記\]\[JS\]為何使用原型](https://medium.com/@iancodinghtml/%E7%AD%86%E8%A8%98-js-%E7%82%BA%E4%BD%95%E4%BD%BF%E7%94%A8%E5%8E%9F%E5%9E%8B-3f6330fc1311)

  > 趁機偷推一下筆者的垃圾筆記，~~真d無恥~~。

* 重新認識 JavaScript
  *  [物件與原型鏈](https://ithelp.ithome.com.tw/articles/10194154)

