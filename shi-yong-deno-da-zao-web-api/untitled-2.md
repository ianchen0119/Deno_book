# 淺談跨來源資源共用（CORS）與解決辦法

### 進入正題

關於 `CORS` ，在 [MDN web docs](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS) 上有詳細的解說，筆者把上面的內容引用過來逐一講解：

> 跨來源資源共用， Cross-Origin Resource Sharing \([CORS](https://developer.mozilla.org/zh-TW/docs/Glossary/CORS)\) 是一種使用額外 [HTTP](https://developer.mozilla.org/zh-TW/docs/Glossary/HTTP) 標頭令目前瀏覽網站的[使用者代理](https://developer.mozilla.org/en-US/docs/Glossary/user_agent)取得存取其他來源（網域）伺服器特定資源權限的機制。當使用者代理請求一個不是目前文件來源——例如來自於不同網域（domain）、通訊協定（protocol）或通訊埠（port）的資源時，會建立一個**跨來源 HTTP 請求（cross-origin HTTP request）**。

白話文來解釋的話：就是使用者訪問了一個網站，但是該網站中有部分圖片或是其他資源並不存在於同一台伺服器上。這時候，瀏覽器就會為我們做跨來源的 HTTP 請求。

舉例：

* CDN

  網站 A 透過 CDN 引用了相關框架做開發，這時我們可以在網頁原始碼中發現：`<script src="https://xxx.com"></script>`

* 圖片資源

  網站 A 內嵌入了一些圖片，這些圖片來自於其他伺服器上：`<img src="https://s4.itho.me/sites/default/files/styles/picture_size_large/public/field/image/v1_wide.jpg?itok=aqrO_0jM"/>`

![img](https://mdn.mozillademos.org/files/14295/CORS_principle.png)

> 上圖同樣取自 MDN Web Docs 。

基於安全性考量，程式碼所發出的跨來源 HTTP 請求會受到瀏覽器限制。像是 [`XMLHttpRequest`](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest) 及 [`Fetch`](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API) 都遵守必須[同源政策（same-origin policy）](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy)。這代表網路應用程式所使用的 API 除非使用 CORS 標頭，否則只能請求與應用程式相同網域的 HTTP 資源。

#### 常見解法

> 筆者第一次處理 CORS 是因為參加了 KKBOX 線上黑客松，這個比賽需要用 KKBOX OPEN API 去開發應用，那時主辦方就有特別提到 `CORS` 的問題。後來在製作畢業專題時，因為有一部分使用 Vue.js 開發的應用是利用 Github-page 讓使用者做存取，導致後端程式與前端程式不同源，產生了 `CORS` 問題。

就筆者的粗淺觀念，處理 `CORS` 前必須先確定開發者的身份：

1. 我是前端開發者
2. 我是後端、全端開發者

確定身份後，再針對身份客製出不同的解決辦法：

**我是前端開發者**

如果你是前端開發者，有兩種作法：

* 請後端工程師在 API 加入 CORS 標頭
* 使用 [CORS Anywhere](https://github.com/Rob--W/cors-anywhere)

  如果今天串接的 API 是 OPEN API 或是使用者無法聯絡到 API 的開發者，那可以使用 CORS Anywhere 這套工具。

  使用方法非常簡單，假設使用者原本要存取的 API Domain 為：

  ```text
  https://domain.com
  ```

  我們只要在該域名前面加上 CORS Anywhere 服務的域名即可，像是：

  ```text
  https://cors-anywhere.herokuapp.com/https://domain.com
  ```

  此外， CORS Anywhere 也是開源專案，這代表只要你有興趣，可以自己建立一個服務。

  > 詳細方法請參閱 [Github Repo](https://github.com/Rob--W/cors-anywhere) 。

**我是後端、全端開發者**

如果使用者本身就有對後端程式修改的權限，那問題就變的很簡單了，以 Oak 所建立的 Web API 為例，我們將目光轉移到 `app.ts` :

```text
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
import { UserRoutes } from "./routers/Route.ts";

const app = new Application();
const router = new Router();
const userRoutes = UserRoutes(router);

app.use(userRoutes.routes());
app.use(userRoutes.allowedMethods());

await app.listen("127.0.0.1:3001");
console.log("? Deno start !");
```

筆者在 Github 上找到了一套名為 [cors](https://github.com/tajpouria/cors) 的第三方套件，它的介紹開門見山、十分易懂：

> CORS is a Deno.js module for providing a [Oak](https://github.com/oakserver/oak)/[Opine](https://github.com/asos-craigmorten/opine)/[Abc](https://github.com/zhmushan/abc)/[Attain](https://github.com/aaronwlee/Attain)/[Mith](https://github.com/jwebcoder/mith) middleware that can be used to enable [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) with various options.
>
> -- [cors](https://github.com/tajpouria/cors)

使用方法很簡單，主要分為兩步驟：

1. 引用套件

   ```text
   import { oakCors } from "https://deno.land/x/cors/mod.ts";
   ```

2. 在 app 實例化後做處理：

   ```text
   const app = new Application();
   app.use(oakCors()); // Enable CORS for All Routes
   app.use(router.routes());
   ```

完成後，我們的 Web API 會變成這樣：

```text
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
import { UserRoutes } from "./routers/Route.ts";
import { oakCors } from "https://deno.land/x/cors/mod.ts";

const app = new Application();
const router = new Router();
const userRoutes = UserRoutes(router);
app.use(oakCors()); // Enable CORS for All Routes
app.use(userRoutes.routes());
app.use(userRoutes.allowedMethods());

await app.listen("127.0.0.1:3001");
console.log("? Deno start !");
```

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [\[Vue.js\]\[日記\]擁抱全家桶系列-你問我資料哪裡來?\(2\)](https://ithelp.ithome.com.tw/articles/10226283)

  > 該系列為筆者去年寫下的傷眼文章。

* [\[教學\] CORS 是什麼? 如何設定 CORS?](https://shubo.io/what-is-cors/)

