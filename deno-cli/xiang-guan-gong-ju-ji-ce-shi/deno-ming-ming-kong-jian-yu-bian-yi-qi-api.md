# Deno 命名空間與編譯器 API



本篇章要為各位讀者帶來的是： Deno 的編譯 API 。筆者會先從 Deno namespace 開始介紹，再進入 Compiler APIs 的介紹！

### 全域空間： Deno

所有非 `Web 標準` 的 API 都會被歸類到命名空間 `Deno` 當中。

其中包含了像是： 檔案讀取、 `TCP Socket` 、運行子程序...等功能。

詳細的內容都被紀錄到了 [Deno doc](https://doc.deno.land/https/raw.githubusercontent.com/denoland/deno/master/cli/dts/lib.deno.ns.d.ts) 上面，若讀者對於這些 API 有興趣都可以在上面找到答案。

> 此外，官方文件也有提供該命名空間的 TypeScript 定義文件 \(.d.ts\) ，請參考[該連結](https://github.com/denoland/deno/blob/v1.4.5/cli/dts/lib.deno.ns.d.ts)。

#### 什麼是命名空間

相信如果各位接觸過 `C 語言` 的開發者對命名空間的概念肯定不陌生。如果沒有接觸過也沒關係，就跟著筆者繼續往下看吧！

> 命名空間（英語：Namespace），也稱名字空間、名稱空間等，它表示著一個識別碼（identifier）的可見範圍。 一個識別碼可在多個命名空間中定義，它在不同命名空間中的含義是互不相干的。
>
> --[wikipedia](https://zh.wikipedia.org/zh-tw/%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)

用白話文來說，假設我們有兩個來自不同外部模組且用途不同的變數或是函式。碰巧的是，他們的名稱一模一樣，這時如果直接執行便會造成衝突。

使用 namespace 就能很好的處理這個問題，開發者可以利用不同的命名空間讓名稱相同但功能不同的函式、變數區隔開來。

### 進入正題

Deno 原生支持了在 TypeScript 編譯器運行時訪問的功能。Deno 命名空間中提供了三種訪問方法。

#### Deno.compile\(\)

該功能與 `deno.cache` 相似，它能夠載入並將程式碼暫存下來、編譯程式碼，但無法執行它。

該 API 包含 3 個參數：

* rootName

  編譯後目標程式的名稱。

* sources

  Sources 是一個 Hash table ，它的 `Key` 為模組名稱、 `Value` 則為模組的內容。

  > 筆者不太確定為何在官方文件中會以 `Hash` 形容它。實際上，在填入選項時就只是輸入一個鍵值對而已，如果我有找到答案會在第一時間更新到這邊。

* options

  options 是一组 `Deno.CompilerOptions` 類型的選項，它包含了 Deno 支持的 TypeScript 編譯器選項。

介紹完三個可選參數後，我們來進一步看看範例：

```typescript
import { assert } from "https://deno.land/std@0.73.0/testing/asserts.ts";
const [diagnostics, emitMap] = await Deno.compile("/foo.ts", {
    "/foo.ts": `import * as bar from "./bar.ts";\nconsole.log(bar);\n`,
    "/bar.ts": `export const bar = "bar";\n`,
  },{
      outDir: "dist"
  }
  );

  assert(diagnostics == null); // ensuring no diagnostics are returned
  console.log(emitMap);
```

輸出：

```text
{
  "dist/bar.js.map": '{"version":3,"file":"bar.js","sourceRoot":"","sources":["file:///bar.ts"],"names":[],"mappings":"AAA...',
  "dist/bar.js": 'export const bar = "bar";\n//# sourceMappingURL=bar.js.map',
  "dist/foo.js.map": '{"version":3,"file":"foo.js","sourceRoot":"","sources":["file:///foo.ts"],"names":[],"mappings":"AAA...',
  "dist/foo.js": 'import * as bar from "./bar.ts";\nconsole.log(bar);\n//# sourceMappingURL=foo.js.map'
}
```

> 注意！本章介紹的三種 API 都是不穩定的功能，所以在執行時都需要加入 `--unstable` 唷！
>
> 像是： deno run --unstable compile.ts
>
> 此外，關於什麼是 map ，筆者也有將相關文章放到本日的延伸閱讀唷！

#### Deno.bundle\(\)

該 API 類似於 `Deno.compiler()` 。不同的是，它會直接回傳打包後的結果。

至於可選參數都與 `Deno.compiler()` 相同。

範例：

```typescript
import { assert } from "https://deno.land/std@0.73.0/testing/asserts.ts";
const [diagnostics, emitMap] = await Deno.bundle("/foo.ts", {
    "/foo.ts": `import * as bar from "./bar.ts";\nconsole.log(bar);\n`,
    "/bar.ts": `export const bar = "bar";\n`,
  },{
      outDir: "dist"
  }
  );

  assert(diagnostics == null); // ensuring no diagnostics are returned
  console.log(emitMap);
```

輸出：

```typescript
/ Copyright 2018-2020 the Deno authors. All rights reserved. MIT license.

// This is a specialised implementation of a System module loader.

"use strict";

// @ts-nocheck
/* eslint-disable */
let System, __instantiate;
(() => {
  const r = new Map();

  System = {
    register(id, d, f) {
      r.set(id, { d, f, exp: {} });
    },
  };
  async function dI(mid, src) {
    let id = mid.replace(/\.\w+$/i, "");
    if (id.includes("./")) {
      const [o, ...ia] = id.split("/").reverse(),
        [, ...sa] = src.split("/").reverse(),
        oa = [o];
      let s = 0,
        i;
      while ((i = ia.shift())) {
        if (i === "..") s++;
        else if (i === ".") break;
        else oa.push(i);
      }
      if (s < sa.length) oa.push(...sa.slice(s));
      id = oa.reverse().join("/");
    }
    return r.has(id) ? gExpA(id) : import(mid);
  }

  function gC(id, main) {
    return {
      id,
      import: (m) => dI(m, id),
      meta: { url: id, main },
    };
  }

  function gE(exp) {
    return (id, v) => {
      const e = typeof id === "string" ? { [id]: v } : id;
      for (const [id, value] of Object.entries(e)) {
        Object.defineProperty(exp, id, {
          value,
          writable: true,
          enumerable: true,
        });
      }
      return v;
    };
  }

  function rF(main) {
    for (const [id, m] of r.entries()) {
      const { f, exp } = m;
      const { execute: e, setters: s } = f(gE(exp), gC(id, id === main));
      delete m.f;
      m.e = e;
      m.s = s;
    }
  }

  async function gExpA(id) {
    if (!r.has(id)) return;
    const m = r.get(id);
    if (m.s) {
      const { d, e, s } = m;
      delete m.s;
      delete m.e;
      for (let i = 0; i < s.length; i++) s[i](await gExpA(d[i]));
      const r = e();
      if (r) await r;
    }
    return m.exp;
  }

  function gExp(id) {
    if (!r.has(id)) return;
    const m = r.get(id);
    if (m.s) {
      const { d, e, s } = m;
      delete m.s;
      delete m.e;
      for (let i = 0; i < s.length; i++) s[i](gExp(d[i]));
      e();
    }
    return m.exp;
  }
  __instantiate = (m, a) => {
    System = __instantiate = undefined;
    rF(m);
    return a ? gExpA(m) : gExp(m);
  };
})();

System.register("bar", [], function (exports_1, context_1) {
    "use strict";
    var bar;
    var __moduleName = context_1 && context_1.id;
    return {
        setters: [],
        execute: function () {
            exports_1("bar", bar = "bar");
        }
    };
});
System.register("foo", ["bar"], function (exports_2, context_2) {
    "use strict";
    var bar;
    var __moduleName = context_2 && context_2.id;
    return {
        setters: [
            function (bar_1) {
                bar = bar_1;
            }
        ],
        execute: function () {
            console.log(bar);
        }
    };
});

__instantiate("foo", false);
```

#### Deno.transpileOnly\(\)

該 API 是基於 TypeScript 的 `transpileModule()` ，該功能會將任何型別去除並輸出成 JavaScript 程式。同時，它也不具有型別檢查和依賴模組解析的功能，相較於前兩個 API ，它僅接受兩個參數： options 和 sources 。

接著，讓我們看看範例：

```typescript
const result = await Deno.transpileOnly({
    "/foo.ts": `enum Foo { Foo, Bar, Baz };\n`,
  });

  console.log(result["/foo.ts"].source);
  console.log(result["/foo.ts"].map);
```

執行後，我們會在命令列看到 enum 被輸出成了一個 IIFE 以及 map 檔案的內容：

```typescript
var Foo;
(function (Foo) {
    Foo[Foo["Foo"] = 0] = "Foo";
    Foo[Foo["Bar"] = 1] = "Bar";
    Foo[Foo["Baz"] = 2] = "Baz";
})(Foo || (Foo = {}));
;
```

```text
//# sourceMappingURL=foo.js.map
{"version":3,"file":"foo.js","sourceRoot":"","sources":["foo.ts"],"names":[],"mappings":"AAAA,IAAK,GAAqB;AAA1B,WAAK,GAAG;IAAG,2BAAG,CAAA;IAAE,2BAAG,CAAA;IAAE,2BAAG,CAAA;AAAC,CAAC,EAArB,GAAG,KAAH,GAAG,QAAkB;AAAA,CAAC"}
```

### 引用 TypeScript Library

當使用者有其他特殊需求，可能會需要將 Deno 默認的 Library 覆蓋掉、重新載入，這時我們可以在 `options` 加入 `lib` 選項：

```typescript
let greeter: HTMLElement | null = document.getElementById("greeter")!; // Please forgive the Non-Null Assertion Operator
greeter.innerText = "Hello world!";
```

```typescript
const [diagnostics, emitMap] = await Deno.compile("test-dom.ts", undefined, {
  lib: ["dom", "esnext"], // include "deno.ns" for deno namespace
  outDir: "dist",
});
```

> 注意：就像 `tsc` 一樣，當你提供一個 `lib` 選項時，它會將默認的選項覆蓋掉。這時，基本的 JavaScript Library 也會消失，使用者需要加上自己使用的 es version ，像是： `es5`, `es2015`, `es2016`, `es2017`, `es2018`... 等。
>
> 可以參考 [`lib` compiler option](https://www.typescriptlang.org/docs/handbook/compiler-options.html) 更進一步暸解 `lib` 選項。

#### 引用 Deno 命名空間

除了 TypeScript 提供的 Library 之外， Deno 也提供了 4 個 Library 供開發者使用：

* `deno.ns` -  `Deno` 命名空間。
* `deno.shared_globals` - 提供 Deno 運作時支持的全域介面和變數。
* `deno.window` - 公開全域變數和 Deno 主線程中可用的 Deno 命名空間，是編譯器的默認 API 。
* `deno.worker` - 公開在 Deno 下的工作線程中可用的全域變數。

#### 三斜杠引用

除了使用 `lib` 選項外，我們也可以利用該方法將需要使用的 Library 導入：

```typescript
/// <reference lib="dom" />

document.getElementById("foo");
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [Deno manual](https://deno.land/manual@v1.4.5/runtime/compiler_apis)
* [JavaScript Source Map 详解](https://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)

