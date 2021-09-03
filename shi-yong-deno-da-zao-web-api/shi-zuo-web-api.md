# 實作 Web API

在介紹完 Denon 的使用方法以及 Oak 的概念後，我們就可以進一步來實作 Web API 了！

由於大部分的 Web API 都僅提供 `Get` 和 `POST` 方法，所以本系列只會針對這兩種方法各實作一條路由：

* user
* insert

`user` 路由供使用者查詢資料，而 `insert` 路由則可以供我們新增使用者。

### 進入正題

如果今天只是要實作一個簡易的 Web API ，單檔就能夠搞定了：

```text
import { Application, Router, Status } from "https://deno.land/x/oak/mod.ts";

const app = new Application();

const router = new Router();

router.get("/", (ctx) => {
    ctx.response.status = Status.OK;
    ctx.response.body = { message: "It's work !" };
});

app.use(router.routes());
app.use(router.allowedMethods());

await app.listen("0.0.0.0:3001");
```

不過，一開始也有提到筆者這次要實作的是能夠查詢和新增使用者的 Web API 。因此，我們還會需要串接資料庫，本系列文採用的是 MongoDB ，關於該資料庫的安裝以及串接教學，筆者會在之後的文章介紹。

#### 專案架構

本章預計實作的範例專案結構如下：

```text
|-- WebAPI
    |-- controllers/
    |   |-- models/
    |				| -- Database.ts
    |   |-- UserController.ts
    |-- routers/
    |   |-- Route.ts
    |-- app.ts
```

主要分為 `controllers` 、 `models` 、 `routers` 三大資料夾，分別負責了控制邏輯、資料庫的抽象層以及路由功能。

由於筆者還沒介紹資料庫的安裝與架接，所以本篇會先實作其他部分。

**app.ts**

```text
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
import { UserRoutes } from "./routers/Route.ts";

const app = new Application();
const router = new Router();
const userRoutes = UserRoutes(router);

app.use(userRoutes.routes());
app.use(userRoutes.allowedMethods());

await app.listen("127.0.0.1:3001");
```

**routers/Route.ts**

```text
import { Router, Status, RouterContext  } from "https://deno.land/x/oak/mod.ts";
import { userController } from "../controllers/UserController.ts";
const controller = new userController();
export function UserRoutes(router: Router) {
  return router
    .get("/user/:id", async (ctx: RouterContext) => {
      const users = await controller.findOne(ctx.params.id);
      if (users) {
        ctx.response.status = Status.OK;
        ctx.response.body = users;

      } else {
        ctx.response.status = Status.NotFound;
        ctx.response.body = [];
      }
    })
    .post("/insert", async(ctx: RouterContext)=>{
      const { name } = await ctx.request.body;
      if(name){
        await controller.insertData(name)
        ctx.response.status = Status.OK;
        ctx.response.body = 'Success!'
      }
    })
}
```

**controllers/UserController.ts**

```text
export class userController {
  findOne(props:any){
    // ...
  }
  insertData(props:any){
    // ...
  }
}
```

`UserController.ts` 負責接收路由的請求並根據請求對資料庫進行操作，由於筆者還沒提到 MongoDB 的教學，所以我會把該檔案的操作留到之後的文章一起總結。

### 補充： MVC 模式

> **MVC模式**（Model–view–controller）是[軟體工程](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E5%B7%A5%E7%A8%8B)中的一種[軟體架構](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E6%9E%B6%E6%9E%84)模式，把軟體系統分為三個基本部分：模型（Model）、視圖（View）和控制器（Controller）。
>
> MVC模式最早由[Trygve Reenskaug](https://zh.wikipedia.org/w/index.php?title=Trygve_Reenskaug&action=edit&redlink=1)在1978年提出\[[1\]](https://zh.wikipedia.org/wiki/MVC#cite_note-1)，是[全錄帕羅奧多研究中心](https://zh.wikipedia.org/wiki/%E5%B8%95%E7%BE%85%E5%A5%A7%E5%A4%9A%E7%A0%94%E7%A9%B6%E4%B8%AD%E5%BF%83)（Xerox PARC）在20世紀80年代為程式語言[Smalltalk](https://zh.wikipedia.org/wiki/Smalltalk)發明的一種軟體架構。**MVC模式**的目的是實現一種動態的程式設計，使後續對程式的修改和擴充簡化，並且使程式某一部分的重複利用成為可能。除此之外，此模式透過對複雜度的簡化，使程式結構更加直覺。軟體系統透過對自身基本部分分離的同時也賦予了各個基本部分應有的功能。專業人員可以依據自身的專長分組：
>
> * 模型（Model） - 程式設計師編寫程式應有的功能（實現演算法等等）、資料庫專家進行資料管理和資料庫設計\(可以實現具體的功能\)。
> * 視圖（View） - 介面設計人員進行圖形介面設計。
> * 控制器（Controller）- 負責轉發請求，對請求進行處理。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/MVC)

如果讀者仔細看，會發現本範例已經做到了 Model 與 Controller 的部分，至於為什麼沒有 View 的部分呢？

那是因為我們今天是希望做到前後端分離，所以我們只要將前端所需要的資料回傳即可，若讀者希望練習 MVC 模式，也可以參考本日的延伸閱讀將 View 的部分一同實作。

### API 的設計考量

設計 Web API 時，可以考量以下幾點進行設計：

* 簡潔易讀

  不易讀的網址：

  ```text
  https://ithelp.ithome.com.tw/u/20110850/im
  ```

  不要將 Web API 的 URL 內容過度精簡，會讓開發者與使用者不易理解。

  易讀的網址：

  ```text
  https://ithelp.ithome.com.tw/users/20110850/ironman
  ```

* 大小寫一致、規則統一

  大小寫不一致：

  ```text
  https://ithelp.ithome.com.tw/Users/20110850/Ironman
  ```

  大小寫字母容易混淆使用者，在設計 Web API 時應盡量統一為全部小寫：

  ```text
  https://ithelp.ithome.com.tw/users/20110850/ironman
  ```

* 不會將伺服器架構暴露出來

  API 網址盡量不要將伺服器中的目錄暴露出來，以避免黑客惡意攻擊。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [基于 Koa.js 的 Node.js MVC 框架](https://juejin.im/entry/6844903588326539272)

