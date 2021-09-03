---
description: 本頁面會定期更新 Deno 最新版本的相關資訊。
---

# Deno 更新日誌

Deno 當前最新版本: `1.10` 

2021/03/02: Deno 1.8 增加了以下功能:

* 支援 WebGPUAPI

  Deno 開發團隊期望能讓 Deno 為 tensorflow.js 做加速。

*  支援 [ICU](https://www.cnblogs.com/shanyou/p/13621728.html)

  打開終端機嘗試:

  ```text
  $ deno
  Deno 1.8.0
  exit using ctrl+d or close()
  > const d = new Date(Date.UTC(2020, 5, 26, 7, 0, 0));
  undefined
  > d.toLocaleString("de-DE", {
  	weekday: "long",
  	year: "numeric",
  	month: "long",
  	day: "numeric",
  });
  "Freitag, 26. Juni 2020"
  ```

* `--import-map` 已經穩定，使用時不需要加入`--unstable` 。
* 改良 `deno coverage` 。

2021/01/19: Deno 1.7增加了以下功能:

* 加強 deno compile 交叉編譯的相容性

   `deno compile` 可以從支援的架構（Windows x64，MacOS x64和Linux x64）交叉編譯到其他受支援的架構。

* 增加對 data URL 的支援 \(URL Import\) 
* 查詢 DNS Records
* `deno fmt` 支援 Markdown 語法。

2020/12/13: Deno 1.6增加了以下功能:

* 可編譯成可執行的單一檔案 \(相關介紹請參考`相關工具及測試`頁面\)
* 針對 APPLE M1 CPU \(ARM\)加強支援性
* 整合以支援 TypeScript 4.1
* Deno 語言服務器 \([LSP](https://microsoft.github.io/language-server-protocol/)\)

  * 程式碼自動完成\(補齊\)
  * 懸浮提示
  * 轉至定義
  * 轉至引用
  * deno fmt
  * deno lint

  > 若要在 VSCode體驗該功能，必須先安裝 [VSCode Deno Canary](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno-canary) 插件。



