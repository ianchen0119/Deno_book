# 介面

> 提供給軟體元件間的介面會被存取到的事物的種類可以包括：[常數](https://zh.wikipedia.org/w/index.php?title=%E5%B8%B8%E6%95%B8_%28%E9%9B%BB%E8%85%A6%E7%A7%91%E5%AD%B8%29&action=edit&redlink=1)、[資料型別](https://zh.wikipedia.org/wiki/%E8%B3%87%E6%96%99%E5%9E%8B%E5%88%A5)、[程式](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F)的種類、[例外](https://zh.wikipedia.org/wiki/%E4%BE%8B%E5%A4%96%E8%99%95%E7%90%86)規格、[型別簽名](https://zh.wikipedia.org/w/index.php?title=%E5%9E%8B%E5%88%A5%E7%B0%BD%E5%90%8D&action=edit&redlink=1)。在某些個案，定義[變數](https://zh.wikipedia.org/wiki/%E8%AE%8A%E6%95%B8)作為介面的一部份可能會很有用。介面常會透過註解或（於某些實驗性語言）透過正式的邏輯斷言指明那些程式和方法的功能。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E4%BB%8B%E9%9D%A2_%28%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88%29)

如果你是一名使用過物件導向語言做開發的開發者，肯定對 `Interface` 不陌生。

`Interface` 可以讓我們先對物件規定好範疇，再進行實作。

以 PHP 為例，若我們今天想要實作一個 Linked List ：

```text
interface linkedListInterface{
    function printList();
    function pushFront($data);
    function delete($inputValue);
    function pushBack($data);
    function reverse();
    function clearList();
}
class linkedList implements linkedListInterface{
// 實作...
}
```

上述程式碼中，我們先定義好了一個資料結構中有的抽象方法，再使用 `class` 進行實作。

* 知識點： [資料結構](https://ithelp.ithome.com.tw/articles/%5Bhttps://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%5D%28https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%29)

或許有人會覺得先定義好再做實作很多此一舉，但是當今天有多人開發的需求， `Interface` 就是一個相當好用的工具，因為先定義好了相關屬性和方法，可以有效避免多人開發的衝突。

* 知識點： [Linked List](https://ithelp.ithome.com.tw/articles/%5Bhttps://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8%5D%28https://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8%29)

### 介面實戰

而在 TypeScript 中，我們同樣可以介面來定義物件的型別，至於到底該如何使用介面，就讓我們一起看看以下範例吧！

每個人都有姓名跟年齡， `ian` 也是人，所以在用物件定義 `ian` 之前，我們可以把這些確定的事物給抽離出來：

```text
interface Person {
    name: string;
    age: number;
}
```

定義好一個人必備的元素後，實作物件 `ian` 時，我們就可以遵尋定義好的介面 `Person` ：

```text
let ian: Person = {
    name: 'Ian',
    age: 21
};
```

需要注意的是，在遵循介面實作物件時，不能有多餘或是缺少的屬性，像是：

```text
let ian: Person = {
    name: 'Ian',
    age: 21,
  	birthday: 19990119
};
```

或是：

```text
let ian: Person = {
    name: 'Ian',
};
```

如果讀者認為某些屬性就算沒有被實作出來也沒關係，也可以考慮使用可選屬性： `?` ：

```text
interface Person {
    name: string;
    age?: number;
}
```

如此一來，不實作 `age` 也沒關係啦！

```text
let peter: Person = {
    name: 'Peter',
};
```

假設讀者們更難搞，希望實作物件時可以有預期之外的屬性出現，那我們可以使用`任意屬性`：

```text
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}
```

> 注意，若定義了任意屬性，那麼確定屬性和可選屬性的型別都必須是它的型別的子集。

什麼意思呢？

我們透過歸類可以知道：

1.  `name` 的型別是 `string`。
2.  `age` 的型別是 `number`。

所以，任意屬性的型別至少必須是 `string` 和 `number` 的聯集：

```text
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}
```

更懶一點的作法，可以讓任意屬性的型別指定為 `any` 。

> 不過筆者不建議這麼做就是了。

```text
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}
```

### 補充：唯讀屬性

若今天讀者在實作物件時，希望該物件的特定屬性在物件初始化以後就不能重新進行賦值，可以使用唯獨屬性： `readonly` 。

```text
interface Person {
    readonly name: string;
    age?: number;
    [propName: string]: string | number;
}
```

> 不過在現實世界中這樣做的話，應該會有人感到痛不欲生吧，像是：
>
> [黃宏成台灣阿成世界偉人財神總統](https://zh.wikipedia.org/zh-tw/%E9%BB%83%E5%AE%8F%E6%88%90%E5%8F%B0%E7%81%A3%E9%98%BF%E6%88%90%E4%B8%96%E7%95%8C%E5%81%89%E4%BA%BA%E8%B2%A1%E7%A5%9E%E7%B8%BD%E7%B5%B1)

而最好的作法應該是這樣：

```text
interface Female {
    name: string;
    readonly age?: number;
}
let cindy: Female = {
    name: 'Cindy',
  	age: 18
};
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [讓 TypeScript 成為你全端開發的 ACE！](https://ithelp.ithome.com.tw/users/20120614/ironman/2685)

  *  [Day 11. 前線維護・特殊型別 X 無法無天 - Any & Unknown Type](https://ithelp.ithome.com.tw/articles/10215485)
  *  [Day 12. 機動藍圖・介面宣告 X 使用介面 - TypeScript Interface Intro.](https://ithelp.ithome.com.tw/articles/10215584)

  > 如果讀者希望看到繁體中文的相關資料，我認為這系列真的必看，講的非常詳細。
  >
  > 筆者在這 30 天中很難將 `TypeScript` 的各項觀念一一帶出，如果有需要繼續向下探索，
  >
  > 真的大推、怒推這個系列文。

