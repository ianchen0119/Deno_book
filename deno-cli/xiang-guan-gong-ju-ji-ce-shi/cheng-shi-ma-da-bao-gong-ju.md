# 程式碼打包工具

### 程式碼打包工具

相似於 Webpack ， Deno 提供了簡易的程式碼打包工具:

```text
> deno bundle https://deno.land/std@0.73.0/examples/colors.ts colors.bundle.js
Bundle https://deno.land/std@0.73.0/examples/colors.ts
Download https://deno.land/std@0.73.0/examples/colors.ts
Download https://deno.land/std@0.73.0/fmt/colors.ts
Emit "colors.bundle.js" (9.83KB)
```

輸入 `deno bundle [URL] colors.bundle.js` 以後，我們就可以得到 `colors.bundle.js` 。

> 如果沒有聽過 Webpack 也沒關係，讀者可以參考今天的延伸閱讀。

出於好奇心，筆者也將之後會用到的範例程式拿出來打包看看:

原始檔 1 :

```text
let a = function(i: number,j: number): number{
	return 3.5*i+(-6.6*j)
	}
function b(i: number,j: number) :number{
	return 6.6+(8.8*i)-3.5*j
	}
function MatrixC(i :number,j :number){
	let result = 0;
	for (var x = 1;x <= 60; x++) {
		let c = a(i,x)*b(x,j)
		result = result + c;
	}
	return result
}
export { a,b,MatrixC }
```

原始檔 2 :

```text
import { MatrixC } from './index.ts';
let count = 0;
let start = new Date();
async function run(){
	for (var i = 1;i<=35; i++) {
		for (var j = 1; j<=35; j++) {
		let result = MatrixC(i,j);
		console.log(`C[${i},${j}]:${result}`)
	}
	}
	return new Date();
}

let end = await run()
console.log(end.getTime()-start.getTime())
```

打包後:

```text
// Copyright 2018-2020 the Deno authors. All rights reserved. MIT license.

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
      v = typeof id === "string" ? { [id]: v } : id;
      for (const [id, value] of Object.entries(v)) {
        Object.defineProperty(exp, id, {
          value,
          writable: true,
          enumerable: true,
        });
      }
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

System.register("index", [], function (exports_1, context_1) {
    "use strict";
    var a;
    var __moduleName = context_1 && context_1.id;
    function b(i, j) {
        return 6.6 + (8.8 * i) - 3.5 * j;
    }
    exports_1("b", b);
    function MatrixC(i, j) {
        let result = 0;
        for (var x = 1; x <= 60; x++) {
            let c = a(i, x) * b(x, j);
            result = result + c;
        }
        return result;
    }
    exports_1("MatrixC", MatrixC);
    return {
        setters: [],
        execute: function () {
            a = function (i, j) {
                return 3.5 * i + (-6.6 * j);
            };
            exports_1("a", a);
        }
    };
});
System.register("for", ["index"], function (exports_2, context_2) {
    "use strict";
    var index_ts_1, count, start, end;
    var __moduleName = context_2 && context_2.id;
    async function run() {
        for (var i = 1; i <= 35; i++) {
            for (var j = 1; j <= 35; j++) {
                let result = index_ts_1.MatrixC(i, j);
                console.log(`C[${i},${j}]:${result}`);
            }
        }
        return new Date();
    }
    return {
        setters: [
            function (index_ts_1_1) {
                index_ts_1 = index_ts_1_1;
            }
        ],
        execute: async function () {
            count = 0;
            start = new Date();
            end = await run();
            console.log(end.getTime() - start.getTime());
        }
    };
});

await __instantiate("for", true);

```

執行結果: 正常!

> 需要注意的是，匯出程式碼的附檔名必須為 `.js` ，若是 `.ts` 會無法順利執行。
>
> 為什麼筆者知道呢?
>
> 因為: 我踩過的坑，我知道。

