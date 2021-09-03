# 快速開始

本篇開始，將會針對 Deno 官方文件的子項目逐一介紹：

* CLI and Sandbox
* TypeScript Options
* Import and Export Module
* Testing
* Related Tool

### Deno CLI 介紹

Deno 是一個命令列介面的應用程式，它整合了許多功能，像是：

* 作為 JavaScript 以及 TypeScript 程式碼的執行環境，並且導入了 Sandbox 。
* 自帶測試功能，不需要依賴相依套件
* 自帶類似於 ESLint 的程式檢查工具
* 以 ESModule 為基準，並且支持 Url Import 導入相關外部套件
* DenoLand 上琳瑯滿目的第三方套件

Deno 提供了多種方法讓使用者查詢常見的主要說明：

```text
# 常見說明
deno help 
# 更為精簡的指令，功能與上面的指令一樣
deno -h 
# 相較前者，該命令會印出更多細節資訊
deno --help
```

若要查看指定命令的相關資訊，我們可以將上面的 help 參數作為子命令使用。

下面以 `deno bundle` 為例：

```text
deno help bundle
deno bundle -h
deno bundle --help
```

> 如果讀者對其他命令有興趣，可以參考[官方文件](https://deno.land/manual/tools)。
>
> 不過筆者是覺得不需要太心急，我會在這幾天把這些項目向讀者交代清楚 XD

* 知識點：[CLI](https://zh.wikipedia.org/zh-tw/%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2)

#### 不同的腳本來源

Deno 可以抓取不同來源的程式碼，像是：

* 本地檔案
* 線上檔案（ Deno Land, Other Sources）
*  [stdin](https://zh.wikipedia.org/wiki/%E6%A8%99%E6%BA%96%E4%B8%B2%E6%B5%81)

```text
deno run main.ts
deno run https://mydomain.com/main.ts
cat main.ts | deno run -
```

#### 腳本參數

與 Deno 的子命列不同， Deno 允許使用者將使用者空間的參數傳送到程式碼並運行：

```text
deno run main.ts a b -c --quiet
```

在 main.ts 中：

```text
console.log(Deno.args); // [ "a", "b", "-c", "--quiet" ]
```

> 這邊要注意的是，若將參數放到腳本名稱後面，會被當作上面的範例一樣被傳入程式碼。

假設：  
 我們今天要讓執行的程式具有網路權限，應該這麼做：

```text
deno run --allow-net net_client.ts
```

而不是：

```text
deno run net_client.ts --allow-net
```

這樣做會讓 `--allow-net` 被當作參數傳入程式碼：

```text
console.log(Deno.args); // [ "--allow-net" ]
```

#### 監聽模式

我們可以在運行程式碼時加入 `--watch` ，如此一來便會啟動內建的檔案監聽程式。

```text
deno run --watch net_client.ts
```

上面的命令會讓 deno 以 net\_client.ts 作為入口點，靜態的導入所有本地檔案，當有任何一個檔案被更動， deno 就會重新啟動 net\_client.ts 。

#### 完整性旗標

該功能可以讓我們確保任何被用到的相依套件的完整性、一致性。

並且，該旗標可以用在以下命令：

```text
deno run
deno cache
deno test
```

假設我們的主程式 main.ts 利用 url import 導入其他第三方套件：

```text
// main.ts
export { xyz } from "https://unpkg.com/xyz-lib@v0.9.0/lib.ts";
```

這時候，我們可以加入 `--lock=lock.json --lock-write` 去產生或是更新 lock.json ：

```text
deno cache --lock=lock.json --lock-write src/deps.ts
```

在 lock.json 中，會列出所有 main.ts 用到的相依套件以及它的 hash value ：

```text
{
  "https://deno.land/std@0.71.0/textproto/mod.ts": "3118d7a42c03c242c5a49c2ad91c8396110e14acca1324e7aaefd31a999b71a4",
  "https://deno.land/std@0.71.0/io/util.ts": "ae133d310a0fdcf298cea7bc09a599c49acb616d34e148e263bcb02976f80dee",
  "https://deno.land/std@0.71.0/async/delay.ts": "35957d585a6e3dd87706858fb1d6b551cb278271b03f52c5a2cb70e65e00c26a",
   ...
}
```

為何要有 hash value 呢？理想的 hash function 可以將所有不同的東西經過運算並輸出成獨一無二的值。

因此，我們同樣可以利用 hash 去驗證本地用到的相依套件與目標的相依套件內容是否一致。

* 知識點：[hash](https://zh.wikipedia.org/wiki/%E5%93%88%E5%B8%8C%E8%A1%A8)

上面提到，該旗標也可以用在 `deno run` ，如此一來便能讓程式碼運行時自動驗證套件的正確性。

> 注意：這僅針對先前加入到 `lock.json` 中的相關套件進行驗證。如果是新加入的套件將會被快取起來，不會經過驗證。

若我們希望主程式碼使用的相依套件都是經過本地快取的，可以加入 `--cached-only` 達成目標：

```text
deno run --lock=lock.json --cached-only main.ts
```

#### 快取與編譯旗標

其他能夠影響 `deno run`, `deno test`, `deno cache` 的旗標：

```text
--config                Load tsconfig.json configuration file
--importmap             UNSTABLE: Load import map file
--no-remote                   Do not resolve remote modules
--reload=    Reload source code cache (recompile TypeScript)
--unstable                    Enable unstable APIs
```

**其他旗標**

能影響執行環境的旗標：

```text
--cached-only                Require that remote dependencies are already cached
--inspect=        activate inspector on host:port ...
--inspect-brk=    activate inspector on host:port and break at ...
--seed               Seed Math.random()
--v8-flags=        Set V8 command line options. For help: ...
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Deno Manual](https://deno.land/manual/getting_started/command_line_interface)

