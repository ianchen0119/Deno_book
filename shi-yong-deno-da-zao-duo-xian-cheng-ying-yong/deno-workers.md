# Deno Workers

在暸解何謂多線程以後，我們來看看要如何在 Deno 開發多線程的程式吧！

Deno 支援 [`Web Worker API`](https://developer.mozilla.org/en-US/docs/Web/API/Worker/Worker) ， Workers 可以讓程式運作在多個線程上，而每一個 Worker 實體都在獨立的線程上運作。

目前為止，Deno 只支持 `module` 類型的 Worker 。因此，在初始化新的 Worker 實體時需要加入 type option ：

```text
new Worker(new URL("worker.js", import.meta.url).href, { type: "module" });
```

> 需要注意的是： Deno 目前[不支持](https://github.com/denoland/deno/issues/5216)相對模塊說明符 。使用者可以改用 `URL` 構造器並使用 `import.meta.url` 引入本地端的程式碼。
>
> ```text
> // Good
> new Worker(new URL("worker.js", import.meta.url).href, { type: "module" });
>
> // Bad
> new Worker(new URL("worker.js", import.meta.url).href);
> new Worker(new URL("worker.js", import.meta.url).href, { type: "classic" });
> new Worker("./worker.js", { type: "module" });
> ```

舉例來說，有一個主程式碼 `main.ts` 以來用來派發的工作程式碼 `worker.ts` ：

```text
// main.ts
new Worker(new URL("worker.ts", import.meta.url).href, { type: "module" });
```

```text
// worker.ts
console.log("hello world");
self.close();
```

因為執行 `main.ts` 時會有讀取 `worker.ts` 的動作，所以要記得加入 `--allow-read` 標籤：

```text
deno run --allow-read main.ts
```

#### 載入遠端模組

現在，讓我們來嘗試將 `worker.ts` 移動到遠端上：

```text
// main.ts
new Worker("https://example.com/worker.ts", { type: "module" });
```

因為有網路存取需求，執行時需要加入 `--allow-net` ：

```text
deno run --allow-net main.ts
```

#### 將腳本發布到 gist

Github 有提供 [gist](https://ithelp.ithome.com.tw/articles/10206233) 服務，讓開發者可以張貼自己的程式碼，方便分享或是用於撰寫技術文章。

筆者嘗試將程式碼丟到 gist 上：

```text
// main.ts
new Worker("https://gist.githubusercontent.com/ianchen0119/5935736ab973fcbcbbbb5294d8bd2655/raw/a94a6547d8b66f50441f4cef8a5218ab19b49c11/worker.ts", { type: "module" });
```

gist 上的程式碼：

```text
await console.log("hello world");
self.close();
```

執行時同樣需要加入 `--allow-read` 標籤唷！

> 眼尖的讀者應該有發現該範例在 `console.log("hello world");` 前面多了一個 `await` 。
>
> 可以參考 [Link](https://ithelp.ithome.com.tw/articles/10206233) 。

#### 傳遞參數

Workers 也可以讓我們將需要的參數傳入要派發的任務（ Worker.ts ）當中：

```text
// main.js
const worker = new Worker(new URL("worker.js", import.meta.url).href, { type: "module" });
worker.postMessage('Hello World!');
```

```text
// worker.js
self.onmessage = async (e) => {
	let { data } = e;
	console.log(data);
	self.close();
  };
```

#### 在 worker 中使用 Deno

一般情況下，命名空間 `Deno` 在 Worker 範圍內是不可用的。

不過我們可以利用 `deno: true` 選項做到這點：

```text
// main.js
const worker = new Worker(new URL("worker.js", import.meta.url).href, {  type: "module",  deno: true,});
worker.postMessage({ filename: "./log.txt" });
```

```text
// worker.js
self.onmessage = async (e) => {
  const { filename } = e.data;  
  const text = await Deno.readTextFile(filename);  
  console.log(text);  
  self.close();
};
```

由於該功能還不穩定，執行時需要加入 `--unstable` 旗標：

```text
$ deno run --allow-read --unstable main.js
```

> 補充：當 `Deno` 命名空間在 Worker 作用域中可用時，將會繼承 `main.js` 的權限（使用`--allow-*`標誌指定的權限）。

### 多線程在 Deno 上有任何限制嗎?

筆者在 Deno 的 Github Repo 上[公開發問](https://github.com/denoland/deno/issues/7419)了關於多執行緒的問題，主要提到 Deno 是否有限制執行緒的數量以及每條執行緒是否有記憶體限制， Deno 的主要貢獻者回答:

> There are no limits, you can create as many as you want  
>  Again, there are no limits. We now have facilities to set heap limits on the isolate, but it's not possible to set them for workers.

在這邊提供給有需要的讀者做參考。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Deno Manual](https://deno.land/manual@v1.4.6/runtime/workers)

