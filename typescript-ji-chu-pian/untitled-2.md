# 型別別名

在前一章節中，筆者提到了 `Interface` 的概念並且利用範例讓讀者了解如何使用它。

本章要介紹的是與 `Interface` 十分相似的功能: `型別別名`。

### 進入正題!

在 TypeScript 新手指南中有提到:

> 型別別名用來給一個型別起個新名字。
>
> -- [TypeScript 新手指南](https://willh.gitbook.io/typescript-tutorial/advanced/type-aliases)

```text
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

> 上述範例同樣取自 TypeScript 新手指南。

不過與其說型別別名只能用於取新名字，不如說它可以用來描述一個型別。

此外，型別別名更適合簡化程式碼的型別宣告:

```text
function returnPara(para: string | number){
    //...
}
```

上述程式碼可以利用型別別名改成:

```text
type stringOrNumber = string | number;
function returnPara(para: stringOrNumber){
    //...
}
```

在實際上，型別別名: `type` 就像上面的程式碼一樣，經常被應用在聯合型別的精簡。

### 必備觀念: 繼承

在談真正的主題前，我們先來看看何謂`繼承`吧!

> **繼承**（英語：inheritance）是[物件導向](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1)軟體技術當中的一個概念。如果一個類別B「繼承自」另一個類別A，就把這個B稱為「A的子類」，而把A稱為「B的父類別別」也可以稱「A是B的超類」。繼承可以使得子類具有父類別別的各種屬性和方法，而不需要再次編寫相同的代碼。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E7%BB%A7%E6%89%BF_%28%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6%29)

簡單來說，有點像是 `TypeScript` 與 `JavaScript` 的關係，前者就繼承了後者的所有特性與使用方法並且再以該基礎擴充更多的功能。

### 型別別名的繼承

上面有提到:

> 不過與其說型別別名只能用於取新名字，不如說它可以用來描述一個型別。

會這麼說是因為我們同樣可以利用它來描述物件、函式，就像是 `Interface` 一樣:

```text
type Biological = {
  name: string
  age: number
};
```

不僅如此，它也具有繼承的特性:

```text
type Human = Biological & { IQ: number };
```

更有趣的是， `type` 也可以與 `Interface` 交互作用:

```text
type Biological = {
  name: string
  age: number
};
interface Human extends Biological { 
  IQ: number; 
}
```

```text
interface Biological {
  name: string;
  age: number;
}
type Human = Biological & { IQ: number };
```

話說前一篇章節好像沒有提到 `Interface` 的繼承，筆者在這邊順便補個範例:

```text
interface Biological {
  name: string;
  age: number;
}
interface Human extends Biological { 
  IQ: number; 
}
```

### 兩兄弟的差異

說了這麼多，那 `Interface` 與 `type` 究竟有什麼差別呢 ? 差別有下:

* Interface 支持合併
* type 可以只用來宣告基本的類型型別

  這點是只有 type 能夠做到的，因為只要使用了 `Interface` 就必定會看到 `{}` ，而 `type` 可以被用來為型別換個新別名:

  ```text
  type Name = string
  ```

  或是本章一開始提到的聯合型別，甚至是之後會提到的元組型別:

  ```text
  type Human = Male | Female
  type PetList = [Dog, Pet]
  ```

  最後， `type` 還能夠允許使用者使用 typeof 獲取實體的型別並賦值:

  ```text
  let div = document.createElement('div');
  type Div = typeof div
  ```

#### Interface 合併

若我們重複宣告兩次相同的介面:

```text
interface Cat {
    name: string;
}
interface Cat {
    age: number;
}
```

對於編譯器來說，其實是長這樣:

```text
interface Cat {
    name: string;
    age: number;
}
```

這就是 `Interface` 的合併。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Typescript 中的 interface 和 type 到底有什麼區別](https://www.jishuwen.com/d/2QdL/zh-tw)
*  [TypeScript 新手指南](https://willh.gitbook.io/typescript-tutorial/advanced/type-aliases)

