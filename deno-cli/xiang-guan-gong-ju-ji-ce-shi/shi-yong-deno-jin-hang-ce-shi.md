# 使用 Deno 進行測試

本篇會向各位讀者介紹 Deno 內建的單元測試功能。

在正式開始前，我們先看看什麼是`斷言`：

> 在[程式設計](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88)中，**斷言**（**assertion**）是一種放在程式中的[一階邏輯](https://zh.wikipedia.org/wiki/%E4%B8%80%E9%9A%8E%E9%82%8F%E8%BC%AF)（如一個結果為真或是假的邏輯判斷式），目的是為了標示與驗證程式開發者預期的結果－當程式執行到斷言的位置時，對應的斷言應該為真。若斷言不為真時，程式會中止執行，並給出錯誤訊息。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E6%96%B7%E8%A8%80_%28%E7%A8%8B%E5%BC%8F%29)

簡單來說，當我們在程式執行的某個階段下可以肯定某個變數、函式輸出... 的值時，就能夠使用斷言。

筆者認為斷言可以翻譯成：`斷定的認為`會更好理解。

### 進入正題

為了讓開發者編寫測試， Deno 的標準庫內建了[斷言模組](https://deno.land/std@0.73.0/testing/asserts.ts)。

```text
import { assert } from "https://deno.land/std@0.73.0/testing/asserts.ts";

Deno.test("Hello Test", () => {
  assert("Hello");
});
```

`assert()` 是一個簡單的布林值斷言，可以用於斷言任何可推導成 `true` 的值。

#### 相等性

Deno 提供了三種相等性斷言供開發者使用：

* `assertEquals(para1, para2, errMsg)`

  比較兩者是否相等。

* `assertNotEquals(para1, para2, errMsg)`

  比較兩者是否為不相等。

* `assertStrictEquals(para1, para2, errMsg)`

  該斷言方法不會對比較值進行強制轉型，這也就意味著使用者需要確保兩個比較輸入值的型別是完全相同的。

> 補充： para1 \(實際輸出\) 和 para2 \(期望值\) 為待測值，當斷言結果不符合預期會在命令列印出自定義的 `errMsg` ，當然錯誤訊息是可省略的。

在實際應用上，我們可以將外部模組的待測試函式 `export` 後，並使用 `import` 導入到我們撰寫的測試檔:

```text
// 受測邏輯
export function sum(para1: number, para2: number): number{
    return para1 + para2 ;
}
```

```text
// 測試程式
import { assertEquals } from "https://deno.land/std@0.73.0/testing/asserts.ts";
import { sum } from "./testLogi.ts";
  Deno.test("check on sum",()=>{
    let result = sum(1,2)
    assertEquals(result,4,`result should be 4, but output is ${result}.`);
  });
```

準備好程式碼後，使用 deno test 開始測試：

```text
deno test test.ts
```

> 注意： 請務必使用 deno test 去執行測試程式。愚蠢如筆者，昨天反覆的使用 deno run 執行測試檔，果不其然，什麼都沒發生。
>
> 害我自己很擔心本日的文章產不出來 XDD

在上面的範例中，我們斷言 `result` 的值為 `4` ，不過 `sum(1,2)` 卻回傳了 `3` 。因此，我們可以看到命列中印出了錯誤：

```text
test check on sum ... FAILED (2ms)

failures:

check on sum
AssertionError: result should be 4, but output is 3.
    at assertEquals (asserts.ts:196:9)
    at test.ts:12:5
    at asyncOpSanitizer (deno:cli/rt/40_testing.js:34:13)
    at Object.resourceSanitizer [as fn] (deno:cli/rt/40_testing.js:68:13)
    at TestRunner.[Symbol.asyncIterator] (deno:cli/rt/40_testing.js:240:24)
    at AsyncGenerator.next ()
    at Object.runTests (deno:cli/rt/40_testing.js:317:22)

failures:

        check on sum

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out (4ms)
```

我們也可以一次進行多個測試：

```text
import { assertEquals } from "https://deno.land/std@0.73.0/testing/asserts.ts";
import {sum} from "./testLogi.ts";
Deno.test("check on foo", () => {
    class Foo {}
    const foo1 = new Foo();
    const foo2 = new Foo();
    assertEquals(foo1, foo2, "not eq");
  });
  Deno.test("check on sum",()=>{
    let result = sum(2,2)
    assertEquals(result,4,`result should be 4, but output is ${result}.`);
  });
```

執行結果：

```text
running 2 tests
test check on foo ... ok (3ms)
test check on sum ... ok (1ms)

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out (4ms)
```

#### 選擇單一測試

就上述的測試範例來看，一共有 `"check on foo"` 和 `"check on sum"` 兩個測試流程，若我們希望單獨拉出某個流程進行測試，這時候 `-- filter` 就派上用場了：

```text
deno test --filter "check on sum" test.ts
```

結果：

```text
running 1 tests
test check on sum ... ok (4ms)

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out (5ms)
```

### 其他斷言

Deno 一共提供了 10 種斷言方法：

*  `assert(expr: unknown, msg = ""): asserts expr`
*  `assertEquals(actual: unknown, expected: unknown, msg?: string): void`
*  `assertNotEquals(actual: unknown, expected: unknown, msg?: string): void`
*  `assertStrictEquals(actual: unknown, expected: unknown, msg?: string): void`
*  `assertStringContains(actual: string, expected: string, msg?: string): void`
*  `assertArrayContains(actual: unknown[], expected: unknown[], msg?: string): void`
*  `assertMatch(actual: string, expected: RegExp, msg?: string): void`
*  `assertNotMatch(actual: string, expected: RegExp, msg?: string): void`
*  `assertThrows(fn: () => void, ErrorClass?: Constructor, msgIncludes = "", msg?: string): Error`
*  `assertThrowsAsync(fn: () => Promise, ErrorClass?: Constructor, msgIncludes = "", msg?: string): Promise`

在暸解一般的斷言後，也可以練習看看其他的斷言方法。礙於篇幅問題，筆者就不拉出來一一說明了，還請見諒。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Deno manual](https://deno.land/manual@v1.4.4/testing/assertions)
*  [How to Write Tests in Deno](https://dev.to/robdwaller/how-to-write-tests-in-deno-pen)
*  [單元測試的藝術-讀後整理](https://ithelp.ithome.com.tw/articles/%5Bhttps://medium.com/@SunXiaoShan/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E7%9A%84%E8%97%9D%E8%A1%93-%E8%AE%80%E5%BE%8C%E6%95%B4%E7%90%86-ba2a4d3491c5%5D%28https://medium.com/@SunXiaoShan/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E7%9A%84%E8%97%9D%E8%A1%93-%E8%AE%80%E5%BE%8C%E6%95%B4%E7%90%86-ba2a4d3491c5%29)

