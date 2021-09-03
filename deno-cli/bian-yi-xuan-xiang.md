# 編譯選項

### 進入正題

筆者在前一篇文章向大家介紹了如何在 Deno 中使用多模組開發、 `Url import` 、 `DenoLand` 的介紹。

本篇就來談談 Deno 對 TypeScript 支援的其他項目吧！

#### 母湯檢查喔！

我們都知道， TypeScript 程式碼執行時會先經過型別檢查。然而，檢查的行為其實是非常消耗效能的。假設我們已經對程式碼進行過測試，就不需要在每次執行時都進行型別檢查。這時，我們可以使用 `--no-check` 標籤：

```text
deno run --no-check main.ts
```

該標籤適用於以下指令：

```text
deno run
deno test
deno cache
deno bundle
```

> 需要注意的是：
>
> 在使用 `--no-check` 以後， TypeScript 不會進行型別檢查。也因為這樣，我們不能自動刪除僅導入 `import` 和 `export` 的 `type` 。
>
> 對此， 貼心的 TypeScript 也提供了 [import type 和 export type 語法](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-exports)。\(要使用此語法，需要確保 Deno 的版本在 `1.4.0` 以上並使用 `--unstable` 標籤以及對 TypeScript 編譯器進行以下設定：
>
> * importsNotUsedAsValues: error
> * isolatedModules: true
>
> \)

#### 使用外部類型定義

TypeScript 天生就擁有健全型別系統。若我們在開發相關應用時需要用到原生 JavaScript 編寫的第三方模組，該如何讓它也支援類型推論呢。為了解決這個問題， Deno 提供三種引用類型定義文件的方法：

**Compiler hint**

如果我們要導入 JavaScript 模組，並且知道該模組在哪進行類型定義，我們就可以在導入時指定類型定義。此方法會告知 Deno `.d.ts` 文件的位置以及與它們相關的 JavaScript 模組程式碼。使用方法如下：

例設： 我們有一個第三方模組 `foo.js` 。並且，我們知道它的類型定義文件 `foo.d.ts` ，我們可以這麼做：

```typescript
// @deno-types="./foo.d.ts"
import * as foo from "./foo.js";
```

> 什麼是類型定義文件？
>
> 可以參考[連結](https://tasaid.com/blog/20171102225101.html)

**Triple-slash reference directive in JavaScript files**

假設使用者要提供可在 Deno 中使用的第三方模組，並且為 Deno 標記好類型定義，可以在程式碼中使用三斜杠指令，就像是：

```text
/// <reference types="./foo.d.ts" />
export const foo = "foo";
```

**X-TypeScript-Types custom header**

除了使用三斜杠指令以外，我們也可以使用自定義 HTTP 標題的 `X-TypeScript-Types` 達到同樣的目的。

自定義標題的運作方式與三斜杠引用方法相同，表示 JavaScript 模組本身的內容不需要修改，類型定義的位置可以由服務器本身確定。

> 注意：並不是所有類型定義都支持該功能！ Deno 使用 `compiler hint` 載入指定的 `.d.ts` 檔案，但是有些 `.d.ts` 檔案包含了 Deno 不支援的功能。具體來說，某些 `.d.ts` 檔案希望使用模組解析的邏輯從其他模組中載入、引用類型定義。 筆者必須老實說：因為研究所推甄的原因，我還沒有嘗試這項功能，之後有空我會找時間去跳坑再把心得補上。

#### 客製化 TypeScript Compiler Options

在 Deno 的生態系中，任何有關嚴格模式的 TypeScript 編譯選項都是開啟的。如果讀者希望可以對 TypeScript Compiler Options 做到更寬鬆的訂製，我們可以用 `tsconfig.json` 做到：

```javascript
{
  "compilerOptions": {
    "allowJs": false,
    "allowUmdGlobalAccess": false,
    "allowUnreachableCode": false,
    "allowUnusedLabels": false,
    "alwaysStrict": true,
    "assumeChangesOnlyAffectDirectDependencies": false,
    "checkJs": false,
    "disableSizeLimit": false,
    "generateCpuProfile": "profile.cpuprofile",
    "jsx": "react",
    "jsxFactory": "React.createElement",
    "jsxFragmentFactory": "React.Fragment",
    "lib": [],
    "noFallthroughCasesInSwitch": false,
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitUseStrict": false,
    "noStrictGenericChecks": false,
    "noUnusedLocals": false,
    "noUnusedParameters": false,
    "preserveConstEnums": false,
    "removeComments": false,
    "resolveJsonModule": true,
    "strict": true,
    "strictBindCallApply": true,
    "strictFunctionTypes": true,
    "strictNullChecks": true,
    "strictPropertyInitialization": true,
    "suppressExcessPropertyErrors": false,
    "suppressImplicitAnyIndexErrors": false,
    "useDefineForClassFields": false
  }
}
```

上面是目前 Deno 支援的編譯選項，在編輯好你的 `tsconfig.json` 後，在執行程式碼時需加入 `-c tsconfig.json` ：

```text
deno run -c tsconfig.json mod.ts
```

> 換句話說：其他 [TypeScript Docs](https://www.typescriptlang.org/docs/handbook/compiler-options.html) 所提到的編譯選項，移到 Deno 上執行時有兩種可能：
>
> 1. Deno 真的不支援
> 2. TypeScript 告訴你這是實驗性功能

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [Deno Manual](https://deno.land/manual@v1.4.4/getting_started/typescript)
* [TypeScript Docs](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
* [Said:听说 - 類型定義文件](https://tasaid.com/blog/20171102225101.html)

