# URL Import

在我們學習完 TypeScript 的基礎語法以及 Deno CLI 的基本操作後，本章要跟各位筆者分享的是如何在 TypeScript 中多檔開發和 Deno 特有的 `url import` 和 Typescript 的制定選項。

### 進入正題

什麼是 `ES Modules` 呢？

ES Modules 是用來處理模組的 ECMAScript 標準，儘管 Node.js 最初是遵照 CommonJS 標準去實作 JS 的模組管控。但是近年來也隨著瀏覽器對於 JavaScript ES6 的支援度上升和 webpack 的崛起，就連 node.js 都開始支援 ES Modules ， Deno 原生就遵照 ES Modules 也是可預期的事情。

#### 匯出

```text
// moduleA.ts
export const name = 'Ian';
export class People {
  // ...
}
export function sum(){
  // ...
}
export const errLog = ()=>{
  // ...
}
```

我們可以使用 `export` 將我們要用到的`函式`、`物件`、`變數`通通匯出。

#### 匯入

有匯出就一定會有匯入，我們來看看要如何匯入上面的模組所匯出的變數吧！

```text
// moduleB.ts
import { name, People, sum, errLog } from './moduleA.ts'
```

* 知識點：[解構賦值](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

  > 解構賦值是一項非常好用的技巧，我們也可以在大量的原始碼中看到它的身影，非常推薦大家學習。

> 注意：以上範例中的 `moduleA.ts` 和 `moduleB.ts` 必須放在同一個目錄/資料夾下才能正常匯入唷！
>
> 至於為什麼，各位筆者可以參考該[連結](https://www.ithome.com.tw/voice/124831)。

#### 預設匯出 \(export default\)

若使用者只有單一變數、函式...等需要匯出，可以考慮使用預設匯出：

```text
const sum = () => {
  // ...
};
export default sum;
```

匯入與剛才的方法大同小異：

```text
import sum from './moduleA.ts'
sum();
```

如果筆者希望將多項東西統一匯出，而不是在單一模組中使用多次 `export` ，我們也可以利用 `export default` ＋ 物件做到：

```text
const name = 'Ian'; 
class People {
  // ...
}
function sum(){
  // ...
}
const errLog = ()=>{
  // ...
}
export default {
  name,
  People,
  sum,
  errLog,
};
```

匯入的話，這邊一樣用到解構賦值的技巧：

```text
import { name, People, sum, errLog } from './moduleA.ts'
```

#### URL Import

URL Import 是 Deno 的一大特色，我們可以透過相同的語法將遠端的模組引入到我們的程式碼：

```text
/**
 * remote.ts
 */
import {
  add,
  multiply,
} from "https://x.nest.land/ramda@0.27.0/source/index.js";

function totalCost(outbound: number, inbound: number, tax: number): number {
  return multiply(add(outbound, inbound), tax);
}

console.log(totalCost(19, 31, 1.2));
console.log(totalCost(45, 27, 1.15));
/**
 * Output
 *
 * 60
 * 82.8
 */
```

#### DenoLand

提到了 URL Import ，可能會有讀者好奇：那我要去哪邊找我需要的第三方套件呢？

這時， [DenoLand](https://deno.land/x) 就派上用場啦～！

**What is deno.land/x?**

> deno.land/x is a hosting service for Deno scripts. It caches releases of open source modules stored on GitHub and serves them at one easy to remember domain.
>
> -- [DenoLand](https://deno.land/x)

DenoLand 收錄並緩存了大量的第三方模組，筆者之前利用空閒時也有看到許多有趣的專案，其實 Deno 的第三方模組也越來越完全，真的很適合大家現在入坑呢 XDD

> 如果對於開源貢獻有興趣，現在挑坑 Deno 更是再好不過，因為在許多東西尚不完全時要參一腳協作是最容易的。
>
> 以 Linux Kernel 為例，數千萬行的程式碼就讓許多開發者望而卻步。講了這麼多，好 Deno ，不玩嗎?

#### 問題： URL import 在斷網時還能用嗎？

這個問題也是筆者在第一次接觸 Deno 時所發出的疑問。

不用擔心， Deno 具有將模組緩存的機制。當第三方模組第一次被使用時， Deno 便會將它緩存到本地端，之後只要使用到相同的模組也會使用本地緩存的模組。

當然，如果想要更新本地緩存的第三方模組，可以使用 `--reload` 標籤達到使用者的需求。

> 這些問題的答案，筆者是在[這裡](https://deno.land/manual#other-key-behaviors)找到的，在該教學的最後也不忘拿 Node.js 出來比較一番，真的十分有趣（？）。

### 使用 Map 集中第三方模組

我們可以將專案中使用到的第三方模組改用 MAP 進行管理。這樣一來，若有兩個檔案都使用到同一個第三方套件時，便無需重新輸入攏長的 Url 。

#### 範例

* import\_map.json

```text
{
   "imports": {
      "fmt/": "https://deno.land/std@0.74.0/fmt/"
   }
}
```

* color.ts

```text
import { red } from "fmt/colors.ts";

console.log(red("hello world"));
```

在運行程式碼時，需加入 `--importmap=` 旗標，像是:

```text
deno run --importmap=import_map.json --unstable color.ts
```

#### 目前限制

* 只能導入單一 Map
* 沒有後備網址
* Deno 不支援 `std:` 命名空間
* 僅支持 `file:` 、 `http:` 、 `https:` 。

#### 設定映射位置

```text
{
  "imports": {
    "/": "./"
  }
}
```

使用:

```text
import { MyUtil } from "/util.ts";
```

或是:

```text
{
  "imports": {
    "/": "./src"
  }
}
```

### 從 URL 導入數據

> 請先參考 [Data URLs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 的使用方式。

先考慮以下程式碼:

```text
export const a = "a";

export enum A {
  A,
  B,
  C,
}
```

上面的程式碼可以被表達成以下形式:

```text
"data:application/typescript;base64,ZXhwb3J0IGNvbnN0IGEgPSAiYSI7CgpleHBvcnQgZW51bSBBIHsKICBBLAogIEIsCiAgQywKfQo="
```

> 在JavaScript 中，要在編碼結果前加上 `data:application/javascript;base64,yourContent` 。

```text
import * as a from "data:application/typescript;base64,ZXhwb3J0IGNvbnN0IGEgPSAiYSI7CgpleHBvcnQgZW51bSBBIHsKICBBLAogIEIsCiAgQywKfQo=";

console.log(a.a);
console.log(a.A);
console.log(a.A.A);
```

執行結果:

```text
deno run https://deno.land/posts/v1.7/import_data_url.ts
a
{ "0": "A", "1": "B", "2": "C", A: 0, B: 1, C: 2 }
0
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [Deno Manual](https://deno.land/manual/examples/import_export)
* [PJ Chen 筆記](https://pjchender.github.io/2017/10/26/js-javascript-%E6%A8%A1%E7%B5%84%EF%BC%88es-module%EF%BC%89/)

  > PJ 大的筆記非常的清楚、易懂，這篇詳細介紹 ES Modules 的筆記也推薦讀者一同觀看。

