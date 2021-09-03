# Deno 跟 Node.js 的主要差異

### 什麼是 Deno ？

Deno 是一個全新的 JavaScript, TypeScript 執行環境，改善了許多 Node.js 為人詬病的缺點。此外， Deno 內建了許多實用的功能，非常建議大家學習。

> 貼心提醒：如果你在閱讀這篇文章之前不曾寫過JS、TS，你可能會有很大的疑惑。
>
> 因此，筆者建議你可以在閱讀完之後TypeScript基礎語法的全部篇章後再回來閱讀本文。

### 進入正題

筆者第一次聽到 Deno 是在 YouTube 上看到 JSConf 的議程 -- [10 Things I Regret About Node.js](https://www.youtube.com/watch?v=M3BM9TB-8yA&vl=en)

當中提到了很多 Node.Js 的缺陷，其中像是：

* 沒有在 API 中支持 JavaScript 的 `Promise` 。

  在2009/6時， Node.Js 是支援 `Promise` 的，不過開發團隊在2020/2將它移除了。

  雖然在之後開發團隊還是將 `Promise` 加了回來，但也因為這樣，許多 Node.Js 的原生 API 都是使用 CallBack Function 而不是 `Promise` 去進行實作。

  > 毫無疑問的， Deno 原生支持 Promise 。

* 安全問題

  V8 引擎本身是很安全的 SandBox ，不過 Node.Js 在應用層面上並沒有承襲這樣良好的傳統。

  使用 V8 引擎 實作的 Node.js 卻能夠在沒有`授權`的情況下，直接訪問網路、檔案系統，甚至是取得記憶體的相關資訊。

  > 筆者認為這邊是十分重要的關鍵，這個缺陷也影響到了我們之後使用 Deno 的方式。

* 建構系統

  使用 GYP 作為建構項目的系統而不是使用 GN \(Generate Ninja\)。

  這個部分作者認為是 Node.Js 的最大疏失，我們先看看在 WiK i上是如何介紹 `Ninja` 的：

  > Evan Martin 從2007年到2012年在 Chrome 團隊工作。在加入初期， Chrome 只能夠在 Windows 上執行，他的主要任務是把代碼移植到其它平台，而面臨的第一個任務就是確定構建系統。
  >
  > Chrome 團隊的成員提出了 GYP 增量解決方案，它的作用是從進階的描述規則生成平台相關的構建檔案。
  >
  > 在 Linux 上，他最開始嘗試把 [Scons](https://zh.wikipedia.org/wiki/SCons) 作為 GYP 的目標構建系統，但當檔案發生變化，啟動構建就需要花費30秒時間。因為他的工作是移植代碼，涉及到頻繁的更改檔案和重新編譯，所以這被認為是不可接受的。
  >
  > 後來，他又嘗試 [make](https://zh.wikipedia.org/wiki/Make) 作為 GYP 的目標構建系統。在剛開始的時候速度相當快，但當檔案越來越多時，它變慢了。後來，他注意到make中的一些問題，覺得可以最佳化，因此有了開發Ninja的想法。
  >
  > 在使用 Ninja 後，修改檔案後 Chrome 增量構建的時間降到了6秒鐘。
  >
  > 使用「Ninja」命名是因為作者覺得它速度很快。
  >
  > -- [wikipedia](https://zh.wikipedia.org/wiki/Ninja_%28%E6%9E%84%E5%BB%BA%E7%B3%BB%E7%BB%9F%29)

  使用 C++ 所編寫 Ninja 比起由 Python 編寫的 GYP 效能差了將近20倍，你可能會有疑問說：對啊！那為什麼 Node.Js 在初步建構時是採用 GYP 呢？

  由於 Chrome V8 最初也是由 GYP 所建構，不過我們可以從 wikipedia 所引用的資料得知， Chrome V8 後來改採 Ninja 建構系統，只可惜當時 Node.Js 的生態鏈已經形成並且很難做修正了，因此我們才會看到現在這樣的結果。

* NPM, `Package.json`, node\_modules 的設計缺陷

  * NPM 過度極權，只要 NPM 在安全性上產生漏洞，全球用戶也跟著遭殃。
  * `Package.json` 成為 Node.Js 專案的必需品，裡面常常放了很多多餘的資訊。
  * 因為採用 CommonJS 標準，在引入其他 Module 時我們會自動忽略副檔名，導致系統需要浪費效能來做判斷。
  * node\_modules 沒有標準的規範，導致不同套件都有不同的結構。

  > 因為這些小小的瑕疵，也造就了 Deno 與 Node.Js 最大的不同：
  >
  > Deno 沒有額外的套件管理系統，並且預設的模組系統是採用 `ES Module` 而不是 [CommonJS](https://zh.wikipedia.org/wiki/CommonJS)。

看完這個議程紀錄片以後，不知為何我想到大家常常說的一句話：

> 每場分手都是經過精心策劃的。

> 分手是不需要理由的。

不過 Deno 的出生並不是沒有理由的， Node.Js 的確有著許多設計上的小瑕疵。

Node.Js 的生態圈中也很努力的想要把這些問題給解決掉 ~~\(遠望 PHP\)~~。

### 總結

本章的最後，筆者來幫大家做個總整理：

* Deno 所使用的模組系統為 `ES Module` 。

  > 在這邊幫 Node.Js 澄清一下：
  >
  > Node.Js 並不是完全遵守 CommonJS 的規範，它是參考了 CommonJS 的規範，請知悉。
  >
  > Node.Js 也逐漸加強了對於 ES Module 的支援，請參考 [Node.js v14.5.0 Documentation](https://nodejs.org/api/esm.html#esm_dual_commonjs_es_module_packages)。

* 我們在 Deno 上執行 `JS` or `TS` 程式碼時，若有需要用到網路、檔案讀取，都需要在 CLI 中下額外指令以確保安全性。
* Deno 並沒有額外的套件管理系統，若需要導入其他模組，只需要將該模組的 Url 引入就好。
* 原生支持 `Promise` 並且能夠直接使用 TypeScript 進行撰寫，無需額外編譯。

> 對於初學者而言，上面琳瑯滿目的專業名詞可能會讓你感到緊張。
>
> 不過這些名詞在之後的 TypeScript 基礎篇中筆者都會額外進行補充，大家不用擔心。

 [![](https://ithelp.ithome.com.tw/upload/images/banners/sjzFcuxo.gif)](https://ithelp.ithome.com.tw/api/banners/sjzFcuxo/click)

