# Oak 框架介紹

### 什麼是 Oak？

Oak 是一款用來開發 http server 的中間件框架，其包含了 Router 路由中間件。

此外，如果讀者有使用 Node.js 的經驗，可能有聽過 Koa 、 Express 等後端框架。 Oak 其實就是致敬 Koa 開發出來的，因此在使用上，兩者有許多重疊的地方，除了第三方的中間件不可共用以外，大部分的觀念都能套用到 Oak 身上。

> 筆者第一次看到 Oak 時，真的直接笑出來。想說這群開發者到底是奪懶，也不花點時間想名字就把 Koa 的名字重組 XDD

### 中間件的概念

![](https://cdn.deno.js.cn/uploads/default/original/1X/4e00d3ec9067bb10a190ffb71894c9a4a8dfc7d8.jpeg)

如圖，每個 Oak 的中間層都像是洋蔥的其中一圈，當後端程式收到 Http Request 會經過每層中間件的處理，最後變為 Response 傳送回去。

> 本圖取自[該網站](https://www.google.com/url?sa=i&url=https%3A%2F%2Fdeno.js.cn%2Ft%2Ftopic%2F35&psig=AOvVaw3etFLrSmVMeiAQopLzvsvo&ust=1602767758699000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCNDFiK6WtOwCFQAAAAAdAAAAABAD)。

讓我們進一步透過範例觀察：

```text
import { Application } from "https://deno.land/x/oak/mod.ts";

const app = new Application();

// Logger
app.use(async (ctx, next) => {
  await next();
  const rt = ctx.response.headers.get("X-Response-Time");
  console.log(`${ctx.request.method} ${ctx.request.url} - ${rt}`);
});

// Timing
app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx.response.headers.set("X-Response-Time", `${ms}ms`);
});

// Hello World!
app.use((ctx) => {
  ctx.response.body = "Hello World!";
});

await app.listen({ port: 8000 });
```

我們可以在範例中看到 `use()` 以及 `next()` 方法， `use()` 用於註冊中間件， `next()` 則用來告訴 Oak 可以先跳到下一個中間件，假設該範例程式正在執行並收到一個請求，中間件的執行順序如下：

```text
Logger -> Timing -> Hello World! -> Timing -> Logger
```

這樣的順序就如同洋蔥圖所表示的一樣，我們更可以利用這個特性去紀錄從 Request 到 Response 一共花了多少時間：

```text
// Timing
app.use(async (ctx, next) => {
  const start = Date.now();// 紀錄收到時間
  await next();// 跳到下一個中間件
  const ms = Date.now() - start;// 回到該中間件時再次紀錄時間
  ctx.response.headers.set("X-Response-Time", `${ms}ms`);
});
```

以上程式碼需要注意的事項有兩點：

1. 開始使用前必須記得將 Oak 引入：

   ```text
   import { Application } from "https://deno.land/x/oak/mod.ts";
   ```

2. 啟動時需加入 `--allow-net` 標籤。

### 路由概念

在實作前後端分離的系統時，以 SPA \(Single Page Application\) 為例，都會有多個虛擬路由，每個路由都代表了不同的頁面，像是： `Home` 、 `About` 、 `Contact` 等等。

因此，我們在實作 API 時也可以將它拆分成對應的路由， Oak 內建了高度致敬 Koa-Router 的路由中間件： `Oak-Router` 。

```text
import { Application, Router } from 'https://deno.land/x/oak/mod.ts';

const app = new Application();
const router = new Router();

router
    .get('/home', (context) => { context.response.body = "This is Home!"; })
		.get('/about', (context) => { context.response.body = "This is About Page."; })
app.use(async (context, next) => {
    await next();
    console.log(`${context.request.method} ${context.request.url}`);
});
app.use(router.routes());
app.use(router.allowedMethods());
await app.listen({ port: 8000 });
```

上面的範例中，一共實作了兩個路由： `home` 以及 `about` 。

實際上，最常見的請求方法有分為 `get` 以及 `post` 方法， `get` 為上面範例所使用到的：

```text
// ...
router
    .get('/home', (context) => { context.response.body = "This is Home!"; })
// ...
```

而 `post` 的用法並不難，收先要做的就是將 `get` 關鍵字換成 `post` 關鍵字：

```text
// ...
.post('/api/movies', async (context) => {
        const data = await context.request.body();
        // ...
        context.response.body = //... ;
    });
// ...
```

#### get 與 post 的差異

get 常用來做查詢資料，因為 get 的查詢參數可以直接在網址上看到，像是：

```text
https://www.findprice.com.tw/g/Ipad
```

此為比價網站 `findprice` 的網址，而網址中的 `Ipad` 表示查詢的關鍵字，在後端程式中，程式將它作為參數作為數入並進行相關操作。

Post 的話則比較常用來做寫入操作，因為相關參數會被放到 http 請求當中，所以我們常常看到大家建議使用 post 而不是使用 get ，因為 get 方法會將請求參數暴露給使用者知道，會更容易出現 `SQL Injection` 以及 `XSS` 等安全漏洞。

> 只要開發者喜歡，要用 get 或是 post 做查詢、寫入都可以。只是使用 get 做查詢相對直觀一些，因為使用者可以透過網址知道現在在瀏覽網站的第幾頁、自己搜尋了什麼關鍵字...等等。

### 使用 Oak 前必備的 JS 觀念

我們都知道， JavaScript 是使用非同步處理機制的，至於要如何讓 JS 做到同步呢？

我們可以善用 `Promise` 以及 `async/await` ，會特別提到同步機制的原因如下：

從一開始的洋蔥圖可以知道，一個中間件是有可能被通過兩次的。若在請求通過中間件第二次時，第一次的操作根本還沒做完便有可能造成嚴重的影響，像是：

```text
// ...
app.use((ctx, next) => {
  const start = Date.now();// 紀錄收到時間
  Insert(start);// 將時間存放至資料庫
  await next();// 跳到下一個中間件
  const ms = Date.now() - start;// 回到該中間件時再次紀錄時間
  ctx.response.headers.set("X-Response-Time", `${ms}ms`);
});
// ...
```

若沒有確保在 `Insert()` 完成前就跳到下一個中間件，便有可能在實際應用中出現巨大的問題，這時候我們可以使用 `Promise` 以及 `async/await` 改善：

```text
// ...
app.use(async (ctx, next) => {
  const start = Date.now();// 紀錄收到時間
  await Insert(start);// 將時間存放至資料庫
  await next();// 跳到下一個中間件
  const ms = Date.now() - start;// 回到該中間件時再次紀錄時間
  ctx.response.headers.set("X-Response-Time", `${ms}ms`);
});
// ...
```

> 這邊要注意的是： 我們必須確保使用到 `await` 的函式在實作中是以 `Promise` 方法達成的，筆者會將相關文章放到本日的延伸閱讀。

如果有讀者對 `同步` 一詞有疑問， `同步`在這邊代表做完一件事才能做下一件事，如果對非同步的概念很模糊，可以回去看[強型闖入DenoLand\[24\] - 使用 Deno 打造多線程應用\(1\)](https://ithelp.ithome.com.tw/articles/10251678)，在本篇筆者有提到 Chrome V8 是如何做任務排程的。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [使用 Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Using_promises)
*  [鐵人賽：JavaScript Await 與 Async](https://wcc723.github.io/javascript/2017/12/30/javascript-async-await/)
*  [《Koa2进阶学习笔记》](https://chenshenhai.github.io/koa2-note/note/start/info.html)
*  [Oak Repo](https://github.com/oakserver/oak)

